---
title: "CTK: Conversation Toolkit for Managing AI Conversations Across All Platforms"
date: 2025-10-09
draft: false
series: ["the-long-echo"]
series_weight: 22
tags: ['Python', 'AI', 'LLM', 'conversation-management', 'ChatGPT', 'Claude', 'Copilot', 'TUI', 'SQLite', 'plugin-architecture', 'privacy', 'MCP']
categories: ['artificial-intelligence', 'tools', 'software-design']
linked_project: [ctk]
description: "A powerful, plugin-based system for managing AI conversations from multiple providers. Import, store, search, and export conversations in a unified tree format while preserving provider-specific details. Built for the Long Echo project—preserving AI conversations for the long term."
---

**[CTK (Conversation Toolkit)](https://github.com/queelius/ctk)** is a powerful, plugin-based system for managing AI conversations from multiple providers. Import, store, search, and export your conversations in a unified tree format while preserving provider-specific details. Built to solve the fragmentation of AI conversations across ChatGPT, Claude, Copilot, and other platforms.

## The Fragmentation Problem

If you use multiple AI assistants, you've experienced this pain:

- **ChatGPT** conversations live in OpenAI's web app
- **Claude** conversations are siloed in Anthropic's interface
- **GitHub Copilot** chat history is buried in VS Code storage
- **Local LLMs** (Ollama, etc.) have no standard export format

**Result**: Your valuable conversations are scattered across incompatible platforms, unsearchable, and at risk of being lost if a provider shuts down or changes their export format.

## CTK's Solution: Universal Tree Format

CTK provides a **universal tree representation** for all conversations:

```
User: "What is Python?"
  └── Assistant: "Python is a programming language..."
      └── User: "How do I install it?"
          └── Assistant: "You can install Python by..."
```

**Key insight**: Linear chats are just single-path trees. Branching conversations (like ChatGPT's "regenerate" feature) are multi-path trees:

```
User: "Write a poem"
  ├── Assistant (v1): "Roses are red..."
  └── Assistant (v2): "In fields of gold..."  [regenerated]
      └── User: "Make it longer"
          └── Assistant: "In fields of gold, where sunshine..."
```

This tree representation preserves **all branching structure** from any provider while providing a uniform interface for search, export, and analysis.

## Quick Start

```bash
# Setup
git clone https://github.com/queelius/ctk.git
cd ctk
make setup
source .venv/bin/activate

# Import from multiple providers
ctk import chatgpt_export.json --db my_chats.db
ctk import claude_export.json --db my_chats.db --format anthropic
ctk import ~/.vscode/workspaceStorage --db my_chats.db --format copilot

# Search with beautiful tables
ctk search "python async" --db my_chats.db --limit 10

# Full-text search
ctk search "machine learning" --db my_chats.db --limit 10

# Interactive TUI
ctk chat --db my_chats.db

# Export for fine-tuning
ctk export training.jsonl --db my_chats.db --format jsonl
```

## Core Features

### 🌳 Universal Tree Format

All conversations stored as trees—linear chats are single-path trees, branching conversations preserve all paths.

**Benefits**:
- Preserves all regenerations and variants from ChatGPT
- Supports conversation forking in Claude
- Captures branching from any provider
- Enables path selection during export (longest, first, latest)

### 🔌 Plugin Architecture

CTK auto-discovers importers and exporters. Adding support for a new provider is trivial:

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

**Place the file in the integrations folder—done!** The plugin is automatically discovered at runtime.

### 💾 SQLite Backend

Fast, searchable local database with proper indexing:

**Schema**:
- `conversations` - Metadata, title, timestamps, source, model
- `messages` - Content, role, parent/child relationships
- `tags` - Searchable tags per conversation
- `paths` - Cached conversation paths for fast retrieval

**Performance**:
- Full-text search across thousands of conversations (instant)
- Indexed queries for filtering by source, model, date, tags
- Efficient tree traversal with cached paths

### 🔒 Privacy First

**100% local**:
- No data leaves your machine
- No analytics or telemetry
- No cloud dependencies

**Optional secret masking**:

```bash
# Remove secrets before sharing
ctk export clean_export.jsonl --db chats.db --format jsonl --sanitize

# Removes:
# - API keys (OpenAI, Anthropic, AWS, etc.)
# - Passwords and tokens
# - SSH keys
# - Database URLs
# - Credit card numbers
```

Custom sanitization rules:

```python
from ctk.core.sanitizer import Sanitizer, SanitizationRule
import re

sanitizer = Sanitizer(enabled=True)

# Company-specific patterns
sanitizer.add_rule(SanitizationRule(
    name="internal_urls",
    pattern=re.compile(r'https://internal\.company\.com/[^\s]+'),
    replacement="[INTERNAL_URL]"
))
```

## Search & Discovery

### Full-Text Search

```bash
# Search with Rich table output
ctk search "machine learning" --db chats.db

# Advanced filtering
ctk search "python" --db chats.db --source ChatGPT --model GPT-4
ctk search "async" --db chats.db --tags "code,tutorial" --limit 20

# Date ranges
ctk search "AI" --db chats.db --date-from 2024-01-01 --date-to 2024-12-31
```

**Output**: Beautiful Rich tables with color-coded sources, models, and message counts.

### Natural Language Commands with `say`

**The killer feature**: Use LLM-powered tool calling to manage your conversations naturally:

```bash
ctk say "show me starred conversations" --db chats.db
ctk say "find discussions about async python" --db chats.db
ctk say "star the last conversation about machine learning" --db chats.db
```

**How it works**: The LLM interprets your command and calls the appropriate CTK tools to execute it. This goes beyond queries - you can actually perform operations like starring, tagging, and exporting through natural language.

### Smart Tagging

Three ways to organize conversations:

1. **Auto-tags by provider and model**: `ChatGPT`, `GPT-4`, `Claude`, `Sonnet-3.5`
2. **Manual tags**: `ctk import --tags "work,2024"`
3. **LLM auto-tagging**: Analyzes conversation content and suggests relevant tags

## Interactive Chat - The Core Feature

**The most powerful feature**: The `chat` command lets you talk with all your conversations using an LLM:

```bash
ctk chat --db chats.db
```

This creates a conversational interface where you can:
- **Ask questions about your conversation history**: "What did I discuss with Claude about Python decorators last month?"
- **Get summaries**: "Summarize my recent conversations about machine learning"
- **Find patterns**: "What topics come up most often in my ChatGPT conversations?"
- **Perform operations**: "Star all conversations about async programming"

The LLM has access to your entire conversation database and can search, filter, and manipulate it through natural language.

## Terminal TUI

In addition to the chat interface, there's a full terminal UI for visual management:

### TUI Features

**Navigation & Browsing**:
- Browse conversations with Rich table view
- Emoji flags for status: ⭐ (starred) 📌 (pinned) 📦 (archived)
- Quick search and natural language commands via `say`
- Tree view for branching conversations
- Path navigation in multi-branch trees

**Conversation Management**:
- Create, rename, delete conversations
- Star, pin, archive operations in real-time
- Auto-tagging with LLM
- Export to various formats from within TUI

**Live Chat**:
- Chat with any LLM provider (Ollama, OpenAI, Anthropic)
- **Model Context Protocol (MCP)** tool support
- Fork conversations to explore alternatives
- Edit and regenerate messages
- Switch between conversation paths

### TUI Commands

```bash
# Navigation
/browse              # Browse conversations table
/show <id>           # Show conversation
/tree <id>           # View tree structure
/paths <id>          # List all paths

# Search & Query
/search <query>      # Full-text search
/say <command>       # Natural language commands (LLM tool calling)

# Organization
/star <id>           # Star conversation
/pin <id>            # Pin conversation
/archive <id>        # Archive conversation
/title <id> <title>  # Rename conversation

# Chat Operations
/fork                # Fork current conversation
/regenerate          # Regenerate last message
/edit <msg_id>       # Edit a message
/model <name>        # Switch LLM model

# Export & Tools
/export <format>     # Export current conversation
/tag                 # Auto-tag with LLM
/help                # Show all commands
/quit                # Exit TUI
```

## Supported Providers

### Importers

| Provider | Format | Branch Support | Notes |
|----------|--------|----------------|-------|
| **OpenAI (ChatGPT)** | `openai` | ✅ Full tree | Preserves all regenerations |
| **Anthropic (Claude)** | `anthropic` | ✅ Full tree | Supports conversation forking |
| **GitHub Copilot** | `copilot` | ❌ Linear | Auto-finds VS Code storage |
| **Google Gemini** | `gemini` | ✅ Partial | Bard conversations |
| **Generic JSONL** | `jsonl` | ❌ Linear | For local LLMs (Ollama, LM Studio) |
| **Coding Agents** | `coding_agent` | ❌ Linear | Cursor, Windsurf, etc. |

### Exporters

| Format | Description | Use Case |
|--------|-------------|----------|
| **JSONL** | One conversation per line | Fine-tuning datasets |
| **JSON** | Native CTK format | Backup, transfer between databases |
| **Markdown** | Human-readable with tree visualization | Documentation, sharing |
| **HTML5** | Interactive browsing with search | Web publishing, archival |

## Import Examples

### ChatGPT/OpenAI

Export from [chat.openai.com/settings](https://chat.openai.com/settings) → Data Controls → Export

```bash
# Auto-detect format
ctk import conversations.json --db chats.db

# Explicit format with tags
ctk import chatgpt_export.json --db chats.db --format openai --tags "work,2024"
```

### Claude/Anthropic

```bash
ctk import claude_export.json --db chats.db --format anthropic
```

### GitHub Copilot (from VS Code)

```bash
# Import from VS Code workspace storage
ctk import ~/.vscode/workspaceStorage --db chats.db --format copilot

# Auto-find Copilot data
python -c "from ctk.integrations.importers.copilot import CopilotImporter; \
          paths = CopilotImporter.find_copilot_data(); \
          print('\n'.join(map(str, paths)))"
```

### Local LLM Formats (JSONL)

```bash
# Import JSONL for fine-tuning datasets
ctk import training_data.jsonl --db chats.db --format jsonl

# Batch import
for file in *.jsonl; do
    ctk import "$file" --db chats.db --format jsonl
done
```

## Organization Features

### Star Conversations

```bash
# Star for quick access
ctk star abc123 --db chats.db

# Star multiple
ctk star abc123 def456 ghi789 --db chats.db

# Unstar
ctk star --unstar abc123 --db chats.db

# List starred
ctk list --db chats.db --starred
```

### Pin Conversations

```bash
# Pin important conversations to the top
ctk pin abc123 --db chats.db

# Unpin
ctk pin --unpin abc123 --db chats.db

# List pinned
ctk list --db chats.db --pinned
```

### Archive Conversations

```bash
# Archive old conversations
ctk archive abc123 --db chats.db

# Unarchive
ctk archive --unarchive abc123 --db chats.db

# List archived (excluded from default views)
ctk list --db chats.db --archived
```

## Database Operations

### Merge Databases

```bash
# Combine multiple databases
ctk merge source1.db source2.db --output merged.db

# Automatically handles duplicates by conversation ID
```

### Database Diff

```bash
# Compare two databases
ctk diff db1.db db2.db

# Shows:
# - Conversations only in db1
# - Conversations only in db2
# - Conversations with different content
```

### Filter and Extract

```bash
# Create filtered database
ctk filter --db all_chats.db --output work_chats.db --tags "work"
ctk filter --db all_chats.db --output starred.db --starred
ctk filter --db all_chats.db --output recent.db --date-from 2024-01-01
```

## Export for Fine-Tuning

### JSONL Format

```bash
# JSONL format for local LLMs
ctk export training.jsonl --db chats.db --format jsonl

# Include only assistant responses
ctk export responses.jsonl --db chats.db --format jsonl --path-selection longest

# Export with metadata
ctk export full_export.jsonl --db chats.db --format jsonl --include-metadata
```

### Export with Filtering

```bash
# Export specific conversations
ctk export selected.jsonl --db chats.db --ids conv1 conv2 conv3

# Filter by source
ctk export openai_only.json --db chats.db --filter-source "ChatGPT"

# Filter by model
ctk export gpt4_convs.json --db chats.db --filter-model "GPT-4"

# Filter by tags
ctk export work_chats.json --db chats.db --filter-tags "work,important"
```

## Path Selection for Branching Conversations

When exporting branching conversations, choose which path to include:

```bash
# Export longest path (most comprehensive)
ctk export out.jsonl --db chats.db --path-selection longest

# Export first path (original)
ctk export out.jsonl --db chats.db --path-selection first

# Export most recent path (latest regeneration)
ctk export out.jsonl --db chats.db --path-selection last
```

**Why this matters**: ChatGPT often has multiple regenerated responses. Path selection lets you choose which variant to include in your training data or export.

## Python API

```python
from ctk import ConversationDB, registry

# Load conversations
with ConversationDB("chats.db") as db:
    # Search
    results = db.search_conversations("python async")

    # Load specific conversation
    conv = db.load_conversation("conv_id_123")

    # Get all paths in branching conversation
    paths = conv.get_all_paths()
    longest = conv.get_longest_path()

    # Add new message to existing conversation
    from ctk import Message, MessageContent, MessageRole

    msg = Message(
        role=MessageRole.USER,
        content=MessageContent(text="New question")
    )
    conv.add_message(msg, parent_id="previous_msg_id")
    db.save_conversation(conv)
```

### Batch Operations

```python
import glob
from ctk import ConversationDB, registry

# Import all exports from a directory
with ConversationDB("all_chats.db") as db:
    for file in glob.glob("exports/*.json"):
        format = "openai" if "chatgpt" in file.lower() else None
        convs = registry.import_file(file, format=format)

        for conv in convs:
            # Add file source as tag
            conv.metadata.tags.append(f"file:{file}")
            db.save_conversation(conv)

    # Get statistics
    stats = db.get_statistics()
    print(f"Imported {stats['total_conversations']} conversations")
```

## Statistics

```bash
ctk stats --db chats.db
```

**Output**:

```
Database Statistics:
  Total conversations: 851
  Total messages: 25890
  Starred: 23
  Pinned: 5
  Archived: 142

Messages by role:
    assistant: 12388
    user: 9574
    system: 1632

Conversations by source:
    ChatGPT: 423
    Claude: 287
    Copilot: 141
```

## The Long Echo Connection

CTK was built for the **Long Echo** project—preserving AI conversations for the long term. Key strategies:

### 1. Multiple Export Formats

Export to formats that will survive platform changes:

- **HTML5**: Self-contained, works in any browser (even offline)
- **Markdown**: Plain text with formatting, readable anywhere
- **JSON**: Structured data, easy to parse decades later
- **Plain text**: Ultimate fallback for maximum longevity

```bash
# Export to blog (Hugo static site)
ctk export blog/conversations.html --db life.db --format html5
ctk export blog/conversations/ --db life.db --format markdown
ctk export blog/conversations.txt --db life.db --format text
```

### 2. Physical Backups

The blog post "Long Echo" documents the full strategy:

- USB drives given to loved ones
- CDs/DVDs for optical redundancy
- Multiple cloud providers (GitHub Pages, Netlify, Vercel)
- Local NAS/backup systems

### 3. Format Resilience

**HTML5 + JavaScript**: CTK's HTML export includes:
- Interactive browsing interface
- Search functionality (works offline)
- Tree visualization for branching conversations
- No server dependencies—pure static files

**Markdown + YAML**: Hugo-compatible format:
- Browse naturally in any text editor
- Git-friendly for version control
- Easy to migrate to other static site generators

## MCP (Model Context Protocol) Integration

CTK supports MCP for tool calling during live chat:

```bash
# Start TUI with MCP server
ctk chat --db chats.db --mcp-config mcp.json
```

**MCP servers provide tools** that the LLM can call:
- File system operations
- Web search
- Database queries
- Custom functions

**Example MCP configuration**:

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed"],
      "env": {}
    }
  }
}
```

The LLM can now read files, search directories, and perform operations through MCP tools during the conversation.

## Design Philosophy

**🌳 Trees, Not Lists**: All conversations are trees, enabling natural representation of branching

**🔌 Pluggable by Default**: Auto-discovery of importers/exporters makes adding providers trivial

**🔒 Privacy First**: 100% local, no telemetry, optional sanitization

**📊 Rich by Default**: Beautiful terminal output with color-coded tables

**🤖 LLM-Powered**: Natural language queries, auto-tagging, smart search

**⚡ Fast**: SQLite with proper indexing for instant search

## Use Cases

### Personal Knowledge Management

Import all your AI conversations and search them like a second brain:

```bash
# Find that Python trick you discussed 3 months ago
ctk search "python context manager" --db personal.db

# Or use natural language
ctk say "show me that conversation about decorators" --db personal.db
```

### Fine-Tuning Datasets

Export curated conversations for training local models:

```bash
# Export starred conversations for fine-tuning
ctk filter --db all.db --output curated.db --starred
ctk export fine_tune.jsonl --db curated.db --format jsonl
```

### Conversation Archaeology

Analyze your interaction patterns with AI over time:

```python
with ConversationDB("life.db") as db:
    stats = db.get_statistics()

    # Most common topics
    from collections import Counter
    all_tags = []
    for conv in db.get_all_conversations():
        all_tags.extend(conv.metadata.tags)

    common = Counter(all_tags).most_common(10)
    print("Most discussed topics:", common)
```

### Backup and Portability

Never lose your conversations when switching providers:

```bash
# Export everything to multiple formats
ctk export backup_$(date +%Y%m%d).json --db life.db --format json
ctk export archive_$(date +%Y%m%d).html --db life.db --format html5
ctk export portable_$(date +%Y%m%d).md --db life.db --format markdown
```

## Development

```bash
# Run tests
make test

# Unit tests only
make test-unit

# Integration tests
make test-integration

# Coverage report
make coverage

# Format code (black + isort)
make format

# Lint (flake8 + mypy)
make lint

# Clean build artifacts
make clean
```

## Roadmap

### Completed ✅
- Terminal UI with conversation management
- Rich console output with tables
- Natural language commands via `say` (LLM tool calling)
- Star/pin/archive organization
- Multiple export formats (JSONL, JSON, Markdown, HTML5)
- MCP tool integration
- Auto-tagging with LLM
- Database merge/diff operations

### In Progress 🔨
- Embeddings and similarity search
- Unit and integration test coverage
- Performance optimization for large databases

### Planned 📋
- Web-based UI (complement to TUI)
- Conversation deduplication utilities
- LangChain/LlamaIndex integration
- Advanced analytics dashboard

## Resources

- **Repository**: [github.com/queelius/ctk](https://github.com/queelius/ctk)
- **Long Echo Blog Post**: [blog/long-echo](https://queelius.github.io/metafunctor/post/2025-01-long-echo/)
- **Documentation**: See `README.md` in repository
- **Examples**: See `examples/` directory

## License

MIT

---

*CTK: Because your conversations with AI are valuable knowledge that deserves to be preserved, searchable, and yours forever. Built for the Long Echo project—ensuring today's conversations remain accessible decades from now.*
