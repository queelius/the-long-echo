---
title: "The Long Echo Toolkit: Preserving Your Digital Life"
date: 2025-12-16T18:00:00
draft: false
series: ["the-long-echo"]
series_weight: 20
tags:
  - long-term-thinking
  - data-preservation
  - cli
  - python
  - sqlite
  - personal-archival
categories:
  - Projects
  - Philosophy
linked_project: [btk, ctk, ebk]
description: "Three tools for preserving your digital intellectual life: conversations, bookmarks, and books. Built on the same resilient architecture with reading queues, semantic search, and LLM integration."
canonical_url: https://metafunctor.com/post/2025-12-16-long-echo-toolkit/
---

Earlier this year I wrote about [Long Echo](/post/2025-01-long-echo/)—a philosophy for preserving AI conversations in ways that remain accessible across decades. The core insight was **graceful degradation**: designing systems that fail progressively, not catastrophically.

Since then, I've built out a complete toolkit for preserving all personal digital content—not just conversations, but bookmarks (including videos, podcasts, articles) and books. These three tools form a coherent system for managing your intellectual life.

## The Toolkit

| Tool | Domain | Install |
|------|--------|---------|
| **[CTK](https://github.com/queelius/ctk)** | AI Conversations | `pip install conversation-tk` |
| **[BTK](https://github.com/queelius/btk)** | Bookmarks & Media | `pip install bookmark-tk` |
| **[EBK](https://github.com/queelius/ebk)** | eBooks & Documents | `pip install ebk` |

All three share a common architecture and philosophy, but each is deeply specialized for its domain.

## Shared Architecture

### SQLite-First Storage
Every tool uses local SQLite databases you own. No cloud dependency. Queryable with standard tools even if the CLI disappears:

```bash
# Works even if the tools are gone
sqlite3 conversations.db "SELECT title FROM conversations WHERE title LIKE '%python%'"
sqlite3 bookmarks.db "SELECT url, title FROM bookmarks WHERE stars = 1"
sqlite3 library.db "SELECT title, author FROM books WHERE favorite = 1"
```

### Interactive Shells with Virtual Filesystems
Navigate your data like a Unix filesystem:

```bash
$ btk shell
btk:/$ cd tags/programming/python
btk:/tags/programming/python$ ls
3298  4095  5124  (bookmark IDs)
btk:/tags/programming/python$ cat 4095/title
Advanced Python Techniques

$ ebk shell
ebk:/$ cd authors/Knuth
ebk:/authors/Knuth$ ls
The Art of Computer Programming Vol 1
The Art of Computer Programming Vol 2
```

### Reading Queues
Track what you're reading, watching, or working through:

```bash
# Bookmarks
btk queue add 42 --priority high
btk queue next
btk queue progress 42 --percent 75
btk queue estimate-times  # Auto-estimate from content length

# Books
ebk queue add "Gödel, Escher, Bach"
ebk queue next
ebk queue list
```

### LLM Integration
All three tools integrate with LLMs for intelligent features:

```bash
# Auto-tag using content analysis
btk content auto-tag --all
ctk auto-tag --model ollama/llama3
ebk enrich 42  # Enhance metadata with LLM

# Natural language queries
ctk say "summarize my conversations about Rust"
btk ask "find articles about distributed systems"
ebk similar "Gödel, Escher, Bach"  # Semantic similarity
```

### Network Analysis
Discover relationships in your data:

```bash
# CTK: Conversation networks
ctk net embeddings --all
ctk net similar 42
ctk net clusters
ctk net central  # Most connected conversations
ctk net outliers  # Isolated conversations

# BTK: Bookmark graphs
btk graph build
btk graph analyze
```

### Web Servers
Browse your archives in a web UI:

```bash
btk serve  # REST API + web interface
ebk serve  # Library browser
```

## BTK: More Than Bookmarks

BTK evolved from a simple bookmark manager into a comprehensive media archive:

### Media Detection & Import
Automatically identifies and handles different content types:

```bash
btk media detect  # Scan bookmarks for media URLs
btk media list --type youtube
btk media import-playlist "https://youtube.com/playlist?list=..."
btk media import-channel "@3blue1brown"
btk media import-podcast "https://feed.example.com/rss"
btk media stats
```

### Content Caching
Stores local copies for offline access and full-text search:

```bash
btk content refresh --all --workers 50  # Cache all bookmarks
btk content view 42 --html  # View cached content
btk search "specific phrase" --in-content  # Search cached text
```

Supports HTML pages, PDFs (with text extraction), and converts everything to searchable markdown.

### Smart Collections
Auto-updating views of your bookmarks:

```bash
btk:/$ cd /unread     # Items in queue not yet started
btk:/$ cd /popular    # Most visited
btk:/$ cd /broken     # Dead links
btk:/$ cd /untagged   # Need organization
btk:/$ cd /pdfs       # PDF documents
btk:/$ cd /recent/added/week  # Added this week
```

## CTK: Conversation Intelligence

CTK manages AI conversations from any platform with sophisticated analysis:

### Universal Import
```bash
ctk import chatgpt_export.json --format openai
ctk import claude_export.json --format anthropic
ctk import ~/.vscode/workspaceStorage --format copilot
```

### Tree Structure Preservation
Conversations aren't linear—they branch. CTK preserves the full tree:

```bash
ctk tree 42  # Visualize conversation structure
ctk paths 42  # List all paths through the tree
ctk duplicate 42  # Fork a conversation
```

### Network Operations
Find patterns across your conversation history:

```bash
ctk net embeddings --all  # Generate embeddings
ctk net similar 42  # Find related conversations
ctk net clusters  # Discover topic clusters
ctk net path 42 87  # Find connection between conversations
```

## EBK: Library Management

EBK handles ebooks with features for serious readers:

### Automatic Processing
```bash
ebk import ~/Calibre\ Library/  # Import from Calibre
ebk import ~/Downloads/*.pdf  # Batch import PDFs
```

Automatically:
- Extracts text from PDFs and EPUBs
- Generates cover thumbnails
- Deduplicates by content hash
- Chunks text for semantic search

### Virtual Libraries
Create collection views without moving files:

```bash
ebk vlib add "Machine Learning" 42 43 44
ebk vlib list "Machine Learning"
```

### Annotations
```bash
ebk note add 42 "Chapter 3 insight: ..."
ebk note list 42
```

### Semantic Search
```bash
ebk search "neural network architectures"
ebk similar 42  # Find books like this one
```

## Multi-Format Export

The key to long-term preservation—export in formats that will always be readable:

```bash
# CTK exports
ctk export conversations.jsonl --format jsonl
ctk export conversations/ --format markdown
ctk export index.html --format html5

# BTK exports
btk export bookmarks.html html --hierarchical  # Browser-compatible
btk export bookmarks.json json
btk export bookmarks.md markdown

# EBK exports
ebk export catalog.html --format html
ebk export library.json --format json
```

Each export level provides graceful degradation:
1. **Full tool** → Rich interactive features
2. **SQLite queries** → Direct database access
3. **JSON/JSONL** → Machine-readable, greppable
4. **HTML** → Browseable in any browser
5. **Markdown/Text** → Readable in notepad

## A Complete Personal Archive

```
archive/
├── START_HERE.txt
├── conversations/
│   ├── conversations.db      # CTK database
│   ├── conversations.jsonl   # Greppable
│   ├── conversations/        # Markdown files
│   └── index.html            # Browseable
├── bookmarks/
│   ├── bookmarks.db          # BTK database
│   ├── bookmarks.html        # Browser-importable
│   ├── cached/               # Offline content
│   └── media/                # Downloaded videos/podcasts
├── books/
│   ├── library.db            # EBK database
│   ├── catalog.html          # Browseable catalog
│   ├── texts/                # Extracted text
│   └── covers/               # Thumbnails
└── RECOVERY.md
```

## Why This Matters

Your intellectual life is worth preserving:
- The debugging session that saved a project
- The research trail that led to a breakthrough
- The books that shaped your thinking
- The videos that taught you a skill

These deserve better than "hope the cloud doesn't disappear."

## Links

- **CTK**: [github.com/queelius/ctk](https://github.com/queelius/ctk)
- **BTK**: [github.com/queelius/btk](https://github.com/queelius/btk)
- **EBK**: [github.com/queelius/ebk](https://github.com/queelius/ebk)
- **Philosophy**: [Long Echo: Designing for Digital Resilience](/post/2025-01-long-echo/)

---

*Your digital intellectual life is worth preserving. Not in fragile cloud services, but in formats that will still be readable when your grandchildren find that USB drive.*
