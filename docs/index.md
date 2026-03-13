---
associations:
  featured:
  - slug: longecho
    type: project
  links:
  - name: GitHub Organization
    url: https://github.com/long-echo
  papers:
  - dagshell
  - dreamlog-paper
  projects:
  - ctk
  - btk
  - ebk
  - mtk
  - ptk
  - longshade
  - jot
  - repoindex
  - crier
  - src2md
  writing: []
description: Digital preservation tools and philosophy. Graceful degradation, plain-text
  archival, and building systems that outlast their creators.
title: The Long Echo
---

*Not resurrection. Not immortality. Just love that still responds.*

I have stage 4 cancer. That is part of why I build these tools.

I build tools for preserving digital artifacts: conversations, bookmarks, photos, email, ebooks. The tools share a philosophy: your data should be readable in fifty years without the original software.

## Graceful Degradation

Every tool in this ecosystem exports to formats that work at multiple levels:

```
Level 1: Full app        → Semantic search, rich UI
Level 2: Database        → SQLite queries (app gone, database remains)
Level 3: File search     → grep through JSONL (just text tools)
Level 4: Human reading   → Markdown, HTML (no tools needed)
Level 5: Fallback        → Plain text in any editor
```

If the software disappears, the data does not. If the database corrupts, the JSONL files remain. If everything fails, the plain text survives.

## The Toolkit

**Archive tools**, each handling one domain:

- **CTK**: AI conversations across all platforms
- **BTK**: Bookmarks with hierarchical tags and semantic search
- **EBK**: Ebook libraries
- **MTK**: Email archives with person resolution
- **PTK**: Photo libraries with content-hash identification

**Orchestration**:

- **Long Echo**: The layer connecting all tools, shared export format (ECHO)
- **Jot**: CLI notes and journal
- **Longshade**: Persona synthesis from personal archives. Spec only, not yet implemented. The philosophical destination of the whole project.

## What This Series Covers

The posts in this series range from tool announcements and technical walkthroughs to philosophical pieces about digital legacy, identity preservation, and what it means to build an echo of yourself from your own data.

Some of the fiction I write connects here too. The novels explore what happens when AI systems carry patterns of people who are gone, which is the same question longshade asks, just dramatized.
