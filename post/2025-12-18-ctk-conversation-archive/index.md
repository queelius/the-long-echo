+++
title = 'CTK: 1,956 OpenAI Conversations as a Long Echo Archive'
date = 2025-12-18T11:11:23-06:00
draft = true
series = ["the-long-echo"]
series_weight = 23
tags = ['ctk', 'long-echo', 'data-preservation', 'personal-archival', 'ai-conversation', 'python', 'cli']
categories = ['tools', 'personal']
description = 'A live demonstration of CTK (Conversation Toolkit) exporting nearly 2,000 OpenAI conversations to a self-contained, searchable HTML archive.'
+++

Following the [Long Echo philosophy](/post/2025-01-long-echo/) and the [Long Echo Toolkit](/post/2025-12-16-long-echo-toolkit/), I've built **CTK (Conversation Toolkit)** - a tool for exporting and preserving AI conversations from platforms like OpenAI's ChatGPT.

## The Archive

**[View Live Archive](/conversations/index.html)** - 1,956 conversations spanning my use of ChatGPT, fully searchable and browsable.

### Stats

- **1,956 conversations** (sanitized from 2,219 - removed 263 containing sensitive data)
- **396 media files** (images generated or shared during conversations)
- **Self-contained HTML** with embedded search
- **Dark mode support**
- **Zero external dependencies**

## Why Archive Conversations?

AI conversations represent a unique form of intellectual dialogue. Unlike traditional documents, they capture:

- The iterative process of thought refinement
- Questions that reveal curiosity and gaps in understanding
- Problem-solving approaches and debugging sessions
- Creative collaborations and brainstorming

These conversations are ephemeral by default. Platforms may change, accounts may be lost, services may shut down. Long Echo ensures they persist.

## CTK Features

CTK follows the same principles as [BTK (Bookmark Toolkit)](/post/2025-11-30-btk/):

- **Export to HTML** - Self-contained, works offline, opens in any browser
- **Automatic sanitization** - Removes conversations containing API keys, passwords, personal identifiers
- **Media preservation** - Downloads and embeds images from conversations
- **Search** - Full-text search across all conversations
- **Multiple views** - Conversation list, timeline, search results

### Sanitization

Before export, CTK automatically filters out conversations containing:
- API keys (`sk-`, `ghp_`, `AKIA`)
- Passwords and secrets
- Personal identifiers

This makes the archive safe to share publicly while preserving the intellectual content.

## Long Echo in Practice

This archive demonstrates the core Long Echo principle: **graceful degradation**.

The HTML export requires:
- No server
- No JavaScript frameworks
- No external APIs
- No internet connection (after download)

If you download this file today, it will still work in 20 years. The format is simple enough that future tools can easily parse and transform it.

## Try CTK

CTK is available as part of my toolkit collection. Export your own conversations:

```bash
ctk export --db conversations.db --format html output.html --media-dir media
```

The tool handles the complexity of:
- Parsing conversation JSON
- Downloading media assets
- Building searchable indexes
- Generating responsive HTML

---

*The conversation archive linked above is a personal artifact - a record of thousands of hours of thinking, learning, and creating with AI assistance. Long Echo ensures it survives.*
