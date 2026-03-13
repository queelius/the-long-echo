+++
title = 'CTK: 1,956 OpenAI Conversations as a Long Echo Archive'
date = 2025-12-18T11:11:23-06:00
draft = true
series = ["the-long-echo"]
series_weight = 23
tags = ['ctk', 'long-echo', 'data-preservation', 'personal-archival', 'ai-conversation', 'python', 'cli']
categories = ['tools', 'personal']
description = 'Exporting nearly 2,000 OpenAI conversations to a self-contained, searchable HTML archive using CTK.'
+++

Following the [Long Echo philosophy](/post/2025-01-long-echo/) and the [Long Echo Toolkit](/post/2025-12-16-long-echo-toolkit/), I used CTK (Conversation Toolkit) to export and preserve my ChatGPT conversations.

## The Archive

**[View Live Archive](/conversations/index.html)** -- 1,956 conversations, fully searchable and browsable.

### Stats

- **1,956 conversations** (sanitized from 2,219, removed 263 containing sensitive data)
- **396 media files** (images generated or shared during conversations)
- **Self-contained HTML** with embedded search
- **Dark mode support**
- **Zero external dependencies**

## Why Archive Conversations?

AI conversations capture something different from normal documents:

- The iterative process of refining a thought
- Questions that reveal where your understanding has gaps
- Problem-solving and debugging sessions in real time
- Brainstorming that you'd otherwise forget

These conversations are ephemeral by default. Platforms change, accounts get lost, services shut down. Long Echo is about making sure the content outlasts the platform.

## CTK Features

CTK follows the same principles as [BTK (Bookmark Toolkit)](/post/2025-11-30-btk/):

- **Export to HTML** -- Self-contained, works offline, opens in any browser
- **Automatic sanitization** -- Removes conversations containing API keys, passwords, personal identifiers
- **Media preservation** -- Downloads and embeds images from conversations
- **Search** -- Full-text search across all conversations
- **Multiple views** -- Conversation list, timeline, search results

### Sanitization

Before export, CTK filters out conversations containing:
- API keys (`sk-`, `ghp_`, `AKIA`)
- Passwords and secrets
- Personal identifiers

This makes the archive safe to share publicly while keeping the intellectual content.

## Long Echo in Practice

This archive demonstrates graceful degradation concretely.

The HTML export requires:
- No server
- No JavaScript frameworks
- No external APIs
- No internet connection (after download)

Download this file today, it still works in 20 years. The format is simple enough that future tools can parse and transform it without trouble.

## Try CTK

Export your own conversations:

```bash
ctk export --db conversations.db --format html output.html --media-dir media
```

The tool handles parsing conversation JSON, downloading media assets, building searchable indexes, and generating responsive HTML.

---

The archive linked above is a record of thousands of hours of thinking and building with AI assistance. Long Echo is about making sure that record survives the platform it was created on.
