---
title: "CTK: Conversation Toolkit"
date: 2025-10-09
draft: false
series: ["the-long-echo"]
series_weight: 22
tags: ['Python', 'AI', 'LLM', 'conversation-management', 'ChatGPT', 'Claude', 'Copilot', 'TUI', 'SQLite', 'plugin-architecture', 'privacy', 'MCP']
categories: ['artificial-intelligence', 'tools', 'software-design']
linked_project: [ctk]
description: "A plugin-based toolkit for managing AI conversations from multiple providers. Import, store, search, and export conversations in a unified tree format. Built for the Long Echo project."
---

[CTK](https://github.com/queelius/ctk) manages AI conversations across platforms. Import from ChatGPT, Claude, Copilot, Gemini. Store locally in SQLite. Search, tag, export. Keep everything.

## The Problem

If you use multiple AI assistants, your conversations are scattered across incompatible platforms, unsearchable, and dependent on companies that may not exist in 20 years. ChatGPT lives in OpenAI's web app. Claude is siloed in Anthropic's interface. Copilot chat history is buried in VS Code storage.

You can't search across them. You can't back them up in a unified format. You can't own them.

## The Key Insight: Conversations Are Trees

Most tools treat conversations as linear sequences. They're not. ChatGPT's "regenerate" feature creates branches. Claude supports conversation forking. Even a simple "let me try that again" is a tree operation.

```
User: "Write a poem"
  ├── Assistant (v1): "Roses are red..."
  └── Assistant (v2): "In fields of gold..."  [regenerated]
      └── User: "Make it longer"
          └── Assistant: "In fields of gold, where sunshine..."
```

CTK stores all conversations as trees. Linear chats are single-path trees. Branching conversations preserve every path. This means you never lose a regeneration, and you can export any path you want.

## What It Does

```bash
# Import from any platform
ctk import chatgpt_export.json --db my_chats.db
ctk import claude_export.json --db my_chats.db --format anthropic
ctk import ~/.vscode/workspaceStorage --db my_chats.db --format copilot

# Search across everything
ctk search "python async" --db my_chats.db

# Natural language queries via LLM tool calling
ctk say "find conversations about distributed systems" --db my_chats.db

# Interactive TUI for browsing and chatting
ctk chat --db my_chats.db

# Export for fine-tuning, archival, or publishing
ctk export training.jsonl --db my_chats.db --format jsonl
ctk export archive.html --db my_chats.db --format html5
ctk export archive/ --db my_chats.db --format markdown
```

### Plugin Architecture

Adding a new provider is one file. Implement `ImporterPlugin`, drop it in the integrations folder, done. Auto-discovered at runtime. No registry, no config.

Currently supported: OpenAI/ChatGPT (full tree), Anthropic/Claude (full tree), GitHub Copilot, Google Gemini, generic JSONL, coding agents (Cursor, Windsurf).

### Privacy

100% local. No telemetry. Optional sanitization strips API keys, passwords, and personal identifiers before export.

```bash
ctk export clean_export.jsonl --db chats.db --format jsonl --sanitize
```

### HTML5 Export

The HTML5 exporter produces a self-contained file with embedded search, tree visualization, and dark mode. No server, no internet, no dependencies. The file works offline in any browser, including continuing conversations with a local LLM directly in the exported HTML.

This is the Long Echo principle made concrete: if CTK disappears tomorrow, the HTML file still works.

### Organization

Star, pin, archive conversations. Auto-tag with LLM analysis. Create filtered databases for specific purposes.

```bash
ctk star abc123 --db chats.db
ctk filter --db all_chats.db --output work.db --tags "work"
ctk merge source1.db source2.db --output merged.db
```

### MCP Integration

CTK supports Model Context Protocol for tool calling during live chat. Connect file systems, databases, or custom functions. The LLM can use these tools while you're having a conversation.

## The Long Echo Connection

CTK was the first tool I built for Long Echo, and for a while I thought it *was* Long Echo. The hard problems of conversation parsing, unified representation, search, and storage were all solved here.

What Long Echo added was the philosophical framework: graceful degradation, multi-format export, the USB-drive-in-2074 thought experiment. CTK implements that framework for conversations.

```
Level 1: Full CTK       Semantic search, TUI, RAG
Level 2: SQLite         Direct queries (CTK gone, database remains)
Level 3: JSONL          grep through files
Level 4: HTML           Open in any browser
Level 5: Markdown       Read in any text editor
```

Each level works even when all higher levels fail.

## Resources

- **Repository**: [github.com/queelius/ctk](https://github.com/queelius/ctk)
- **Long Echo Philosophy**: [Designing for Digital Resilience](/post/2025-01-long-echo/)

---

*Your conversations with AI are valuable knowledge. They deserve better than "hope the company doesn't shut down."*
