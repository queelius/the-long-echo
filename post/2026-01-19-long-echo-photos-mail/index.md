---
author:
  name: "Alex Towell"
  email: "queelius@gmail.com"
  url: "https://metafunctor.com"

title: "Long Echo: Photos and Mail"
date: 2026-01-19
draft: false
series: ["the-long-echo"]
series_weight: 30
tags:
  - long-term-thinking
  - data-preservation
  - cli
  - python
  - sqlite
  - personal-archival
  - photos
  - email
categories:
  - Projects
  - Philosophy
linked_project: [ptk, mtk, ctk, btk, ebk]
description: "Expanding the Long Echo ecosystem with photo and mail archival. Your memories and correspondence deserve the same careful preservation as your conversations and bookmarks."
---

The [Long Echo toolkit](/post/2025-12-16-long-echo-toolkit/) now covers conversations, bookmarks, and ebooks. But two of the most emotionally significant categories of personal data remain: **photos** and **mail**.

Both share a troubling pattern: they're scattered across devices and cloud services, organized by date rather than meaning, and vulnerable to platform disappearance. They deserve better.

## The Expanding Ecosystem

| Tool | Domain | Status |
|------|--------|--------|
| [ctk](https://github.com/queelius/ctk) | AI Conversations | stable |
| [btk](https://github.com/queelius/btk) | Bookmarks & Media | stable |
| [ebk](https://github.com/queelius/ebk) | eBooks | stable |
| [repoindex](https://github.com/queelius/repoindex) | Git Repositories | stable |
| **[ptk](https://github.com/queelius/ptk)** | **Photos** | **incubating** |
| **[mtk](https://github.com/queelius/mtk)** | **Mail** | **incubating** |

The orchestration layer, [longecho](https://github.com/queelius/longecho), ties these together into a unified personal archive.

## PTK: Photo Toolkit

Photos are the most emotionally valuable digital artifacts most people have. They're also among the worst-managed.

### The Problem

Your photo library is probably:

- **Scattered** — Phone, old phones, cloud services, camera imports, messaging app saves
- **Organized by date** — Not by who's in them, where they were taken, or what they mean
- **Cloud-dependent** — Google Photos, iCloud, Amazon Photos—what happens when you switch?
- **Unsearchable by content** — "Find photos of mom at the beach" isn't possible
- **Missing context** — Only you know why that blurry photo matters

### The Vision

ptk provides:

**Unified import** from any source:
```bash
ptk import ~/Pictures/
ptk import ~/phone-backup/DCIM/
ptk import google-takeout.zip --source google-photos
ptk import icloud-export/ --source icloud
```

**Intelligent organization** by multiple dimensions:
```bash
ptk shell
ptk:/$ cd /people/mom
ptk:/people/mom$ ls
2019/  2020/  2021/  2022/  2023/  2024/

ptk:/$ cd /locations/beach
ptk:/$ cd /events/christmas-2023
ptk:/$ cd /years/2020/months/march
```

**AI-powered features**:
```bash
# Face detection and clustering
ptk faces detect --all
ptk faces cluster
ptk faces label cluster-7 "Mom"
ptk faces find "Mom"

# Scene captioning
ptk caption --all --model ollama/llava
ptk search "sunset over water"

# Semantic search
ptk ask "photos from our trip to Colorado"
```

**Preservation guarantees**:
```bash
# Verify nothing is corrupted
ptk verify --checksums

# Export to durable formats
ptk export ~/archive/photos/ --format longecho
ptk export photos.html --format html-gallery

# Original files always preserved
ptk originals list
ptk originals verify
```

### Why SQLite?

Like the other Long Echo tools, ptk uses SQLite for metadata:

```bash
# Works even if ptk disappears
sqlite3 photos.db "
  SELECT path, caption, taken_at
  FROM photos
  WHERE caption LIKE '%birthday%'
  ORDER BY taken_at
"
```

The database stores metadata, face embeddings, captions, and organization. The actual photo files stay in place or are copied to a managed library—your choice.

### Graceful Degradation

1. **Full ptk** → Face search, semantic queries, smart albums
2. **SQLite queries** → Find any photo by metadata
3. **EXIF data** → Dates and locations in the files themselves
4. **Directory structure** → Browse by year/month even with nothing installed
5. **The photos themselves** → JPEG will always be viewable

## MTK: Mail Toolkit

Your email archive is one of the most complete records of your relationships and life decisions. Decades of correspondence, contracts, confirmations, conversations.

### The Problem

Your email is probably:

- **Trapped in cloud services** — Gmail, Outlook, corporate servers
- **Searchable only by keywords** — Not by meaning or relationship
- **A privacy minefield** — Contains everything, shared with everyone
- **Vulnerable to deletion** — One account closure and decades vanish

### The Vision

mtk builds on [notmuch](https://notmuchmail.org/), the excellent mail indexer, adding features for personal archival:

**Import from anywhere**:
```bash
mtk import ~/mail/  # Maildir
mtk import archive.mbox --format mbox
mtk import gmail-takeout.mbox --source gmail
mtk import --imap imap.example.com --user me@example.com
```

**Relationship mapping**:
```bash
mtk relationships
# Top correspondents:
#   alice@example.com    1,247 messages over 8 years
#   bob@work.com           892 messages over 3 years
#   ...

mtk thread-with alice@example.com --year 2020
mtk relationship-graph --output relationships.svg
```

**Enhanced search**:
```bash
# notmuch queries still work
mtk search "from:alice subject:project"

# Plus semantic search
mtk ask "emails about the server migration"
mtk similar 12345  # Find emails like this one
```

**Privacy controls**:
```bash
# Redact before export
mtk export --redact-addresses --redact-attachments

# Filter sensitive content
mtk filter --exclude "password|confidential|ssn"

# Selective export
mtk export --to alice@example.com --after 2020-01-01
```

**notmuch integration**:
```bash
# mtk enhances notmuch, doesn't replace it
mtk notmuch-sync  # Sync mtk metadata to notmuch tags
notmuch search tag:important  # Still works
```

### The Privacy Question

Email requires more careful handling than photos or bookmarks. mtk is designed with privacy as a core concern:

- **Local-only by default** — No cloud sync, no external APIs without explicit action
- **Selective export** — Export only what you choose, with redaction options
- **Encryption support** — Work with GPG-encrypted messages
- **Relationship-aware filtering** — Export only threads with specific people

The goal isn't to expose your email to AI—it's to make your own archive searchable and preservable while maintaining control.

## Shared Architecture

Both ptk and mtk follow the patterns established by ctk, btk, and ebk:

### SQLite-First
```bash
sqlite3 photos.db "SELECT * FROM photos WHERE faces LIKE '%mom%'"
sqlite3 mail.db "SELECT * FROM messages WHERE correspondent = 'alice@example.com'"
```

### Virtual Filesystem Shells
```bash
ptk shell
ptk:/$ cd /people/mom/2023
ptk:/people/mom/2023$ ls

mtk shell
mtk:/$ cd /from/alice@example.com
mtk:/from/alice@example.com$ ls recent/
```

### LLM Integration
```bash
ptk ask "find the photo from Sarah's wedding"
mtk ask "when did I discuss the budget with finance?"
```

### Multi-Format Export
```bash
ptk export --format html-gallery
ptk export --format longecho
mtk export --format mbox
mtk export --format markdown
```

## The Complete Picture

With ptk and mtk, Long Echo covers the major categories of personal digital life:

| Category | Tool | What It Preserves |
|----------|------|-------------------|
| Conversations | ctk | AI chats, coding sessions, research threads |
| Bookmarks | btk | Articles, videos, podcasts, references |
| Books | ebk | Your library, annotations, reading history |
| Code | repoindex | Repositories, commit history, project metadata |
| Photos | ptk | Memories, faces, places, moments |
| Mail | mtk | Correspondence, relationships, decisions |

Each tool is independent—use only what you need. But together, they form a comprehensive personal archive that you own, can query, and can preserve for decades.

## Status

ptk and mtk are in active development. The architecture is defined, core features are being implemented. Both follow the patterns proven by ctk, btk, and ebk.

If you're interested in early testing or have specific needs for photo or mail archival, the repositories are at:

- [github.com/queelius/ptk](https://github.com/queelius/ptk)
- [github.com/queelius/mtk](https://github.com/queelius/mtk)

---

*Your photos are memories. Your mail is relationships. They deserve the same careful preservation as everything else in your digital life.*
