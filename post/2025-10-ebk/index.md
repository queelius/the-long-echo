---
title: "EBK: Managing an eBook Library with SQL, AI, and Set Theory"
date: 2025-10-13
draft: false
series: ["the-long-echo"]
series_weight: 25
tags: ['Python', 'ebook-manager', 'SQLite', 'SQLAlchemy', 'full-text-search', 'knowledge-graphs', 'semantic-search', 'MCP', 'AI', 'cli', 'data-management']
categories: ['software-development', 'data-science', 'artificial-intelligence', 'tools']
linked_project: [ebk]
description: "An eBook metadata management tool with a SQLite backend, knowledge graphs, semantic search, and MCP server integration. Part of the Long Echo project."
---

**[EBK](https://github.com/queelius/ebk)** is a tool I built because my ebook collection outgrew every existing manager I tried. Different formats, inconsistent metadata, duplicate files, and no way to ask "find all books about X published after Y that I haven't read" without writing custom scripts every time.

EBK treats your library as a queryable database with AI-enhanced discovery. It's part of the [Long Echo](/post/2025-01-long-echo/) toolkit: tools for preserving your digital intellectual life in formats you control.

## What it does

At its core, EBK is a SQLAlchemy + SQLite database with FTS5 full-text search, proper indexing, and transaction safety. When you import a file, it extracts text from PDFs (PyMuPDF with pypdf fallback) and EPUBs (ebooklib), generates overlapping chunks for semantic search, computes SHA256 hashes for deduplication, extracts covers, creates thumbnails, and indexes everything for instant search.

```bash
# Import with auto-extraction
ebk db-import my-book.pdf ~/my-library
```

Deduplication is hash-based:

```
Same file (same hash)          -> Skip (already imported)
Same book, different format    -> Add as additional format
Different book                 -> Import as new entry
```

Books are stored in hash-prefixed directories (`ab/cd/ef/abcdef123456...pdf`) so you don't end up with massive flat directories.

## The fluent API

```python
from ebk import Library

lib = Library.open("~/ebooks")

results = (lib.query()
    .where("language", "en")
    .where("date", "2020", ">=")
    .where("subjects", "Python", "contains")
    .order_by("title")
    .take(10)
    .execute())

# Filter and operate
(lib.filter(lambda e: e.get("rating", 0) >= 4)
    .tag_all("recommended")
    .export_to_hugo("/path/to/site", organize_by="subject"))
```

Complex queries without writing SQL. Filter, transform, and export subsets to different formats.

## AI features

These are optional but useful for larger collections.

### Knowledge graphs

Using NetworkX, EBK extracts concept relationships across your library:

```python
graph = lib.build_knowledge_graph(
    extract_entities=True,
    min_connection_strength=0.3
)
graph.visualize(output="library_knowledge.html")
```

This reveals connections you might not notice: "These books about functional programming also discuss category theory."

### Semantic search

Beyond keyword matching, find books by meaning:

```python
results = lib.semantic_search(
    "explaining complex mathematical concepts simply",
    threshold=0.7
)
```

Uses vector embeddings with TF-IDF fallback for offline use.

### MCP server

```bash
pip install ebk[mcp]
```

This lets AI assistants query your library directly, retrieve relevant passages during conversations, and suggest books based on discussion context. You can ask your AI "What books do I have about transformer architectures?" and get accurate results from your own library.

## Set-theoretic library operations

This is one of my favorite features. Merge multiple libraries with set operations:

```bash
# Union: all unique books from all libraries
ebk merge union ~/merged ~/lib1 ~/lib2 ~/lib3

# Intersection: only books present in ALL libraries
ebk merge intersect ~/common ~/lib1 ~/lib2

# Difference: books in lib1 NOT in lib2
ebk merge diff ~/lib1-only ~/lib1 ~/lib2

# Symmetric difference: books in exactly ONE library
ebk merge symdiff ~/unique ~/lib1 ~/lib2
```

Good for consolidating backups, finding duplicates, and identifying unique collections.

## Import from anywhere

```bash
# Calibre library (reads metadata.opf files)
ebk db-import-calibre ~/Calibre/Library ~/my-library

# Individual files with auto-extraction
ebk db-import book.pdf ~/my-library

# Batch import
ebk db-import ~/Downloads/*.epub ~/my-library

# ZIP archives
ebk import-zip library-backup.zip --output-dir ~/my-library
```

## Export

### Hugo static site

```bash
ebk export hugo ~/library ~/hugo-site \
    --jinja \
    --organize-by subject \
    --include-covers \
    --include-files
```

Creates a browsable website with subject-based organization, covers, full-text content, and download links.

### Symlink DAG

```bash
ebk export-dag ~/library ~/output
```

Creates a directory structure where tags become folders and books appear via symlinks. Multiple paths lead to the same book. File managers can browse it naturally.

## CLI

```bash
ebk db-init ~/my-library          # Initialize
ebk db-import book.pdf ~/my-library  # Import
ebk db-search "quantum computing" ~/my-library  # Search
ebk db-stats ~/my-library          # Statistics
```

Rich tables, progress bars, colorized output, JSON mode for scripting.

## Architecture

```
Integrations Layer    <- Streamlit, MCP, Viz
CLI Layer             <- Typer commands
Core Library Layer    <- Fluent API
Import/Export Layer   <- Format handlers
Database Layer        <- SQLAlchemy + SQLite
```

Core is lightweight with minimal dependencies. Extensions are optional (`pip install ebk[streamlit]`, `pip install ebk[viz]`). Each layer is independently testable.

## Optional integrations

**Streamlit dashboard** (`pip install ebk[streamlit]`): web interface for visual browsing, advanced search, statistics, and batch operations.

**Visualization tools** (`pip install ebk[viz]`): network graphs showing book relationships, tag clouds, timeline views, author collaboration networks.

## Getting started

```bash
pip install ebk[all]
ebk db-init ~/my-library
ebk db-import ~/Documents/book.pdf ~/my-library
ebk db-search "topic" ~/my-library
```

## Links

- [GitHub Repository](https://github.com/queelius/ebk)
- [Project Page](/projects/ebk/)

MIT licensed.

---

*Part of the Long Echo project. Your books represent decades of accumulated knowledge. They deserve better than scattered files on a hard drive.*
