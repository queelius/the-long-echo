---
title: "EBK: Ebook Toolkit"
date: 2025-10-13
draft: false
series: ["the-long-echo"]
series_weight: 25
tags: ['Python', 'ebook-manager', 'SQLite', 'SQLAlchemy', 'full-text-search', 'knowledge-graphs', 'semantic-search', 'MCP', 'AI', 'cli', 'data-management']
categories: ['software-development', 'data-science', 'artificial-intelligence', 'tools']
linked_project: [ebk]
description: "EBK is a comprehensive eBook metadata management tool with AI-powered enrichment, semantic search, and knowledge graphs. Part of the Long Echo toolkit."
---

Your books represent decades of accumulated knowledge. Technical references, formative texts, research that shaped your thinking. They deserve better than scattered files on a hard drive with inconsistent metadata and no way to search across them.

[EBK](https://github.com/queelius/ebk) treats your ebook library as a queryable, searchable knowledge base. It's part of the [Long Echo](/post/2025-01-long-echo/) toolkit: tools for preserving your digital intellectual life in formats you control.

## The Core Abstraction

At its heart, EBK is a SQLAlchemy + SQLite database with a normalized schema. Everything else (CLI, AI features, exports) is layered on top. This means your library metadata is always queryable with standard tools, even if EBK itself disappears.

```bash
# Works even without EBK installed
sqlite3 library.db "SELECT title, author FROM books WHERE favorite = 1"
```

## What It Does

```bash
# Initialize and import
ebk db-init ~/my-library
ebk db-import ~/Documents/book.pdf ~/my-library
ebk db-import-calibre ~/Calibre/Library ~/my-library

# Search with FTS5 full-text search
ebk db-search "quantum computing" ~/my-library

# Field-specific queries
ebk db-search "title:Python author:Knuth tag:programming" ~/my-library
```

Behind a simple import, EBK automatically extracts text from PDFs (PyMuPDF with pypdf fallback) and EPUBs, generates text chunks for semantic search, computes SHA256 hashes for deduplication, extracts covers, and indexes everything in FTS5.

### Deduplication

Same file (same hash) gets skipped. Same book in a different format gets added as an additional format. Different book gets imported as new. Books are stored in hash-prefixed directories for scalability.

### AI Enrichment

EBK can use LLMs to auto-generate tags, categories, and descriptions for books with sparse metadata:

```bash
ebk enrich 42  # Enhance metadata with LLM
```

Semantic search finds books by meaning, not just keywords:

```python
results = lib.semantic_search(
    "explaining complex mathematical concepts simply",
    threshold=0.7
)
```

Uses vector embeddings when available, TF-IDF fallback for offline use.

### Knowledge Graphs

Using NetworkX, EBK can extract concept relationships across your library:

```python
graph = lib.build_knowledge_graph(extract_entities=True)
graph.visualize(output="library_knowledge.html")
```

This reveals connections you didn't know existed. "These books about functional programming also discuss category theory."

### Fluent Python API

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
```

### Export

Multiple formats for different needs:

```bash
ebk export hugo ~/library ~/hugo-site --organize-by subject --include-covers
ebk export-dag ~/library ~/output  # Navigable symlink directory structure
```

The Hugo export creates a browsable website. The DAG export creates a tag-based directory structure where books appear via symlinks under multiple categories. Both work without EBK installed.

### Set-Theoretic Operations

Merge libraries with mathematical precision:

```bash
ebk merge union ~/merged ~/lib1 ~/lib2    # All unique books
ebk merge intersect ~/common ~/lib1 ~/lib2 # Books in ALL libraries
ebk merge diff ~/unique ~/lib1 ~/lib2      # Books in lib1 NOT in lib2
```

### MCP Server

EBK includes an MCP server that lets AI assistants query your library directly during conversations. "What books do I have about transformer architectures?" gets an answer from your actual collection.

## Architecture

```
Integrations (Streamlit, MCP, Viz)
CLI (Typer commands)
Core Library (Fluent API)
Import/Export (Format handlers)
Database (SQLAlchemy + SQLite)
```

Each layer is independently testable. Extensions are optional (`pip install ebk[streamlit]`, `pip install ebk[mcp]`).

## Graceful Degradation

1. **Full EBK**: Rich CLI, semantic search, AI enrichment, knowledge graphs
2. **SQLite queries**: Direct database access with standard tools
3. **HTML export**: Browse catalog in any browser
4. **File system**: The books themselves, always readable

The database stores metadata and search indexes. Your actual files stay wherever you put them.

## Resources

- **Repository**: [github.com/queelius/ebk](https://github.com/queelius/ebk)
- **Long Echo Philosophy**: [Designing for Digital Resilience](/post/2025-01-long-echo/)

---

*Your books represent significant intellectual investment. They deserve better than a pile of files in a directory.*
