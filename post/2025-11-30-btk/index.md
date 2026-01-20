---
title: "BTK: Modern Database-First Bookmark Manager"
date: 2025-11-30
draft: false
series: ["the-long-echo"]
series_weight: 24
tags: ['Python', 'bookmarks', 'SQLite', 'NLP', 'CLI', 'productivity']
categories: ['software-development', 'productivity']
description: "A modern, database-first bookmark manager with powerful features for organizing, searching, and analyzing your bookmarks."
---

**[BTK](https://pypi.org/project/bookmark-tk/)** (Bookmark Toolkit) is a modern bookmark manager that treats bookmarks as structured data rather than flat lists. Built on SQLite with NLP-powered auto-tagging, it brings database-level power to personal bookmark management.

## Why Another Bookmark Manager?

Browser bookmark managers are:
- **Flat**: No rich metadata, limited organization
- **Siloed**: Each browser has its own format
- **Ephemeral**: Pages go offline, links break

BTK fixes this:
- **Structured**: SQLite database with full-text search
- **Universal**: Import from Chrome, Firefox, Safari, HTML, JSON
- **Persistent**: Caches content for offline access

Your bookmarks represent years of intellectual curation—articles that shaped your thinking, tutorials that taught you skills, references you return to again and again. They deserve better than ephemeral browser sync. BTK is part of the [Long Echo](/post/2025-01-long-echo/) toolkit: tools for preserving your digital intellectual life in formats you control.

## Quick Start

```bash
pip install bookmark-tk

# Start the interactive shell (recommended)
btk shell

# Or use direct CLI commands
btk bookmark add https://example.com --title "Example" --tags tutorial,web
btk bookmark list
btk bookmark search "python"

# Import and export
btk import html bookmarks.html
btk export bookmarks.html html --hierarchical
```

## Interactive Shell

BTK includes a powerful shell with a virtual filesystem interface:

```
$ btk shell

btk:/$ ls
bookmarks  tags  starred  archived  recent  domains

btk:/$ cd tags
btk:/tags$ ls
programming/  research/  tutorial/  web/

btk:/tags$ cd programming/python
btk:/tags/programming/python$ ls
3298  4095  5124  5789  (bookmark IDs with this tag)

btk:/tags/programming/python$ cat 4095/title
Advanced Python Techniques

btk:/tags/programming/python$ star 4095
★ Starred bookmark #4095

btk:/tags/programming/python$ cd /bookmarks/4095
btk:/bookmarks/4095$ tag data-science machine-learning
✓ Added tags to bookmark #4095
```

Shell features:
- **Virtual filesystem** - Navigate bookmarks like files and directories
- **Hierarchical tags** - Tags like `programming/python/django` create navigable folders
- **Context-aware commands** - Commands adapt based on your current location
- **Unix-like interface** - Familiar `cd`, `ls`, `pwd`, `mv`, `cp` commands

## Hierarchical Tags

Organize with nested tags:

```bash
# Add with hierarchical tags
btk bookmark add https://docs.python.org --tags programming/python/docs
btk bookmark add https://flask.palletsprojects.com --tags programming/python/web

# Query at any level
btk tag filter programming         # All programming bookmarks
btk tag filter programming/python  # Python subset

# Tag management
btk tag list
btk tag tree                       # Show hierarchy
btk tag rename old-tag new-tag
```

## Auto-Tagging with NLP

BTK automatically suggests tags based on content:

```bash
# Preview suggested tags for a bookmark
btk content auto-tag --id 42

# Apply suggested tags
btk content auto-tag --id 42 --apply

# Bulk auto-tag with parallel workers
btk content auto-tag --all --workers 100
```

## Content Caching

Store page content for offline access and full-text search:

```bash
# Content is cached automatically when adding bookmarks
btk bookmark add https://example.com

# Manually refresh content
btk content refresh --id 42            # Specific bookmark
btk content refresh --all              # All bookmarks
btk content refresh --all --workers 50 # Parallel refresh

# View cached content
btk content view 42                    # View markdown in terminal
btk content view 42 --html             # Open HTML in browser

# Search cached content
btk bookmark search "specific phrase" --in-content
```

## PDF Support

Extract and index text from PDF bookmarks:

```bash
# Add PDF bookmark (auto-extracts text)
btk bookmark add https://arxiv.org/pdf/2301.00001.pdf --tags research,ml

# Search within PDF text
btk bookmark search "neural network" --in-content

# View extracted text
btk content view 42
```

## Browser Integration

Import bookmarks from browsers:

```bash
# Import bookmarks
btk import chrome
btk import firefox --profile default
btk import html bookmarks.html
btk import json bookmarks.json
btk import csv bookmarks.csv
```

## Database Operations

```bash
# Use specific database
btk --db ~/bookmarks.db bookmark list

# Set default database
btk config set database.path ~/bookmarks.db

# Database management
btk db info              # Show statistics
btk db vacuum            # Optimize database

# Deduplication
btk db dedupe --strategy merge       # Merge duplicate metadata
btk db dedupe --strategy keep_first  # Keep oldest
btk db dedupe --preview              # Preview changes
```

## Export Formats

```bash
# Export to various formats
btk export output.html html --hierarchical  # HTML with folder structure
btk export output.json json                 # JSON format
btk export output.csv csv                   # CSV format
btk export output.md markdown               # Markdown with sections
```

## Plugin System

Extend BTK with custom plugins:

```python
from btk.plugins import Plugin, PluginMetadata, PluginPriority

class MyPlugin(Plugin):
    def get_metadata(self) -> PluginMetadata:
        return PluginMetadata(
            name="my-plugin",
            version="1.0.0",
            description="Custom functionality",
            priority=PluginPriority.NORMAL
        )

    def on_bookmark_added(self, bookmark):
        # Custom logic when bookmark is added
        pass
```

## Architecture

- **Database**: SQLAlchemy ORM with SQLite backend
- **Testing**: 515 tests, >80% coverage on core modules
- **Content**: HTML/Markdown conversion, zlib compression, PDF extraction

## Installation

```bash
pip install bookmark-tk
```

## Resources

- **PyPI**: [pypi.org/project/bookmark-tk/](https://pypi.org/project/bookmark-tk/)
- **GitHub**: [github.com/queelius/bookmark-tk](https://github.com/queelius/bookmark-tk)

---

*BTK: Your bookmarks deserve better than a flat list in a browser sidebar.*
