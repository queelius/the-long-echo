---
title: "CTK: A Toolkit for Managing AI Conversations Across Platforms"
date: 2025-10-09
draft: false
series: ["the-long-echo"]
series_weight: 22
tags: ['Python', 'AI', 'LLM', 'conversation-management', 'ChatGPT', 'Claude', 'Copilot', 'TUI', 'SQLite', 'plugin-architecture', 'privacy', 'MCP']
categories: ['artificial-intelligence', 'tools', 'software-design']
linked_project: [ctk]
description: "A plugin-based system for importing, storing, searching, and exporting AI conversations from multiple providers in a unified tree format. Part of the Long Echo project."
---

**[CTK](https://github.com/queelius/ctk)** is a tool I built to solve a specific annoyance: my AI conversations are scattered across ChatGPT, Claude, Copilot, and various local LLMs. They're unsearchable across platforms, at risk of disappearing if a provider changes their export format, and structurally incompatible with each other.

CTK imports from all of these into a single SQLite database with a unified tree representation, then lets you search, organize, and export them.

## The tree representation

The key insight: all conversations are trees. Linear chats are single-path trees. Branching conversations (like ChatGPT's "regenerate" feature) are multi-path trees:

```
User: "Write a poem"
  |-- Assistant (v1): "Roses are red..."
  +-- Assistant (v2): "In fields of gold..."  [regenerated]
      +-- User: "Make it longer"
          +-- Assistant: "In fields of gold, where sunshine..."
```

This preserves all branching structure from any provider while giving you a uniform interface for search, export, and analysis.

## Getting started

```bash
git clone https://github.com/queelius/ctk.git
cd ctk
make setup
source .venv/bin/activate

# Import from multiple providers
ctk import chatgpt_export.json --db my_chats.db
ctk import claude_export.json --db my_chats.db --format anthropic
ctk import ~/.vscode/workspaceStorage --db my_chats.db --format copilot

# Search across everything
ctk search "python async" --db my_chats.db --limit 10

# Interactive TUI
ctk chat --db my_chats.db

# Export for fine-tuning
ctk export training.jsonl --db my_chats.db --format jsonl
```

## Plugin architecture

Adding support for a new provider is straightforward. CTK auto-discovers importers and exporters:

```python
# File: ctk/integrations/importers/my_format.py
from ctk.core.plugin import ImporterPlugin
from ctk.core.models import ConversationTree, Message, MessageContent, MessageRole

class MyFormatImporter(ImporterPlugin):
    name = "my_format"
    description = "Import from My Custom Format"
    version = "1.0.0"

    def validate(self, data):
        """Check if data is your format"""
        return "my_format_marker" in str(data)

    def import_data(self, data, **kwargs):
        """Convert data to ConversationTree objects"""
        tree = ConversationTree(id="conv_1", title="Imported")
        msg = Message(
            role=MessageRole.USER,
            content=MessageContent(text="Hello")
        )
        tree.add_message(msg)
        return [tree]
```

Drop the file in the integrations folder and it's discovered at runtime.

## Supported providers

### Importers

| Provider | Format | Branch Support | Notes |
|----------|--------|----------------|-------|
| **OpenAI (ChatGPT)** | `openai` | Full tree | Preserves all regenerations |
| **Anthropic (Claude)** | `anthropic` | Full tree | Supports conversation forking |
| **GitHub Copilot** | `copilot` | Linear | Auto-finds VS Code storage |
| **Google Gemini** | `gemini` | Partial | Bard conversations |
| **Generic JSONL** | `jsonl` | Linear | For local LLMs (Ollama, LM Studio) |
| **Coding Agents** | `coding_agent` | Linear | Cursor, Windsurf, etc. |

### Exporters

| Format | Use Case |
|--------|----------|
| **JSONL** | Fine-tuning datasets |
| **JSON** | Backup, transfer between databases |
| **Markdown** | Documentation, sharing |
| **HTML5** | Interactive browsing with search (works offline) |

## Search and discovery

Full-text search across all providers with SQLite FTS:

```bash
ctk search "machine learning" --db chats.db
ctk search "python" --db chats.db --source ChatGPT --model GPT-4
ctk search "async" --db chats.db --tags "code,tutorial" --limit 20
ctk search "AI" --db chats.db --date-from 2024-01-01 --date-to 2024-12-31
```

There's also a `say` command that uses LLM tool calling for natural language queries:

```bash
ctk say "show me starred conversations" --db chats.db
ctk say "find discussions about async python" --db chats.db
ctk say "star the last conversation about machine learning" --db chats.db
```

The LLM interprets your command and calls the appropriate CTK functions. This goes beyond queries: you can star, tag, and export through natural language.

## The interactive TUI

The `chat` command gives you a conversational interface over your entire conversation database:

```bash
ctk chat --db chats.db
```

You can ask questions about your history ("What did I discuss with Claude about decorators last month?"), get summaries, find patterns, and perform operations. The LLM has access to the full database through tool calling.

The TUI also provides visual management: browsing with Rich tables, tree views for branching conversations, path navigation, star/pin/archive operations, live chat with any LLM provider, MCP tool support, forking, and message editing.

### TUI commands

```bash
/browse              # Browse conversations table
/show <id>           # Show conversation
/tree <id>           # View tree structure
/search <query>      # Full-text search
/say <command>       # Natural language commands
/star <id>           # Star conversation
/pin <id>            # Pin conversation
/archive <id>        # Archive conversation
/fork                # Fork current conversation
/regenerate          # Regenerate last message
/export <format>     # Export current conversation
/model <name>        # Switch LLM model
```

## Privacy

Everything is local. No telemetry, no cloud dependencies. There's optional secret masking for when you want to share exports:

```bash
ctk export clean_export.jsonl --db chats.db --format jsonl --sanitize
```

This strips API keys, passwords, tokens, SSH keys, database URLs, and similar patterns. You can add custom sanitization rules for company-specific patterns.

## Path selection for branching conversations

When exporting, you choose which path to include from branching conversations:

```bash
# Longest path (most comprehensive)
ctk export out.jsonl --db chats.db --path-selection longest

# First path (original)
ctk export out.jsonl --db chats.db --path-selection first

# Most recent path (latest regeneration)
ctk export out.jsonl --db chats.db --path-selection last
```

This matters for fine-tuning datasets. ChatGPT conversations often have multiple regenerated responses, and you want to pick the right variant.

## Database operations

```bash
# Merge databases
ctk merge source1.db source2.db --output merged.db

# Compare databases
ctk diff db1.db db2.db

# Create filtered database
ctk filter --db all_chats.db --output work_chats.db --tags "work"
ctk filter --db all_chats.db --output starred.db --starred
```

## Python API

```python
from ctk import ConversationDB, registry

with ConversationDB("chats.db") as db:
    results = db.search_conversations("python async")
    conv = db.load_conversation("conv_id_123")
    paths = conv.get_all_paths()
    longest = conv.get_longest_path()

    from ctk import Message, MessageContent, MessageRole
    msg = Message(
        role=MessageRole.USER,
        content=MessageContent(text="New question")
    )
    conv.add_message(msg, parent_id="previous_msg_id")
    db.save_conversation(conv)
```

## The Long Echo connection

CTK was built for the [Long Echo](/post/2025-01-long-echo/) project, which is about preserving digital artifacts for the long term. The export formats are chosen for durability:

- **HTML5**: self-contained, works in any browser offline
- **Markdown**: plain text with formatting, readable anywhere
- **JSON**: structured, easy to parse decades later

The physical backup strategy includes USB drives, optical media, multiple cloud providers, and local NAS systems. The goal is that these conversations remain accessible regardless of what happens to any particular platform.

## MCP integration

CTK supports Model Context Protocol for tool calling during live chat:

```bash
ctk chat --db chats.db --mcp-config mcp.json
```

The LLM can call external tools (file system operations, web search, database queries, custom functions) during the conversation.

## Development

```bash
make test              # Run tests
make test-unit         # Unit tests only
make test-integration  # Integration tests
make coverage          # Coverage report
make format            # black + isort
make lint              # flake8 + mypy
```

## Status

**Done**: TUI, Rich output, natural language commands via `say`, star/pin/archive, multiple export formats, MCP tool integration, auto-tagging, database merge/diff.

**In progress**: embeddings and similarity search, test coverage, performance for large databases.

**Planned**: web UI, conversation deduplication, LangChain/LlamaIndex integration, analytics dashboard.

## Links

- **Repository**: [github.com/queelius/ctk](https://github.com/queelius/ctk)
- **Long Echo**: [blog post](/post/2025-01-long-echo/)

MIT licensed.

---

*Part of the Long Echo project. Your conversations with AI are knowledge worth preserving.*
