---
title: "BTK: Bookmark Toolkit"
date: 2025-11-30
draft: false
series: ["the-long-echo"]
series_weight: 24
tags: ['Python', 'bookmarks', 'SQLite', 'NLP', 'CLI', 'productivity']
categories: ['software-development', 'productivity']
description: "A database-first bookmark manager with hierarchical tags, content caching, and NLP auto-tagging. Part of the Long Echo toolkit."
---

Your bookmarks represent years of intellectual curation. Articles that shaped your thinking, tutorials that taught you skills, references you return to again and again. They deserve better than a flat list in a browser sidebar, siloed per browser, with no metadata and no way to search content.

[BTK](https://github.com/queelius/btk) treats bookmarks as structured data. SQLite database, full-text search, hierarchical tags, content caching, NLP-powered auto-tagging. Part of the [Long Echo](/post/2025-01-long-echo/) toolkit.

## Why Another Bookmark Manager?

Browser bookmark managers are flat (no rich metadata), siloed (each browser has its own format), and ephemeral (pages go offline, links break, and you lose the content along with the link).

BTK fixes all three: structured SQLite database you own, import from Chrome/Firefox/Safari/HTML/JSON, and content caching that stores page text locally for offline access and full-text search.

## The Shell

BTK includes a virtual filesystem interface. Navigate your bookmarks like a Unix filesystem:

```bash
$ btk shell

btk:/$ ls
bookmarks  tags  starred  archived  recent  domains

btk:/$ cd tags/programming/python
btk:/tags/programming/python$ ls
3298  4095  5124  5789

btk:/tags/programming/python$ cat 4095/title
Advanced Python Techniques

btk:/tags/programming/python$ star 4095
Starred bookmark #4095
```

Familiar `cd`, `ls`, `pwd`, `mv`, `cp` commands. Hierarchical tags create navigable folder trees. Smart collections (`/unread`, `/popular`, `/broken`, `/untagged`, `/pdfs`) are auto-updating views.

## Key Features

**Hierarchical tags**: Tags like `programming/python/django` create a navigable tree. Query at any level.

```bash
btk tag filter programming         # All programming bookmarks
btk tag filter programming/python  # Python subset
btk tag tree                       # Show full hierarchy
```

**Content caching**: Stores local copies of pages as compressed Markdown. Search within cached content, view offline, survive link rot.

```bash
btk content refresh --all --workers 50  # Cache all bookmarks in parallel
btk bookmark search "specific phrase" --in-content
btk content view 42 --html              # View cached content
```

**PDF support**: Extracts and indexes text from PDF bookmarks.

**NLP auto-tagging**: Analyzes content and suggests tags based on TF-IDF.

```bash
btk content auto-tag --all --workers 100  # Bulk auto-tag
```

**Browser integration**: Import from Chrome, Firefox, Safari, or any HTML/JSON/CSV bookmark export.

**Plugin system**: Extend with custom hooks for bookmark lifecycle events.

## Database Operations

```bash
btk db info               # Statistics
btk db vacuum             # Optimize
btk db dedupe --preview   # Find and merge duplicates
```

## Export

```bash
btk export bookmarks.html html --hierarchical  # Browser-importable
btk export bookmarks.json json
btk export bookmarks.md markdown
```

The HTML export with `--hierarchical` creates a folder structure matching your tag tree. Importable back into any browser.

## Numbers

515 tests, >80% coverage on core modules. Published on PyPI as `bookmark-tk`.

```bash
pip install bookmark-tk
```

## Graceful Degradation

1. **Full BTK**: Shell, auto-tagging, content caching, web server
2. **SQLite queries**: `sqlite3 bookmarks.db "SELECT url, title FROM bookmarks WHERE stars = 1"`
3. **HTML export**: Browse in any browser, import into any browser
4. **JSON export**: Parse with jq or any tool
5. **The URLs themselves**: Still just links

## Resources

- **PyPI**: [pypi.org/project/bookmark-tk](https://pypi.org/project/bookmark-tk/)
- **Repository**: [github.com/queelius/btk](https://github.com/queelius/btk)
- **Long Echo Philosophy**: [Designing for Digital Resilience](/post/2025-01-long-echo/)

---

*Your bookmarks are intellectual breadcrumbs. BTK makes sure the trail survives.*
