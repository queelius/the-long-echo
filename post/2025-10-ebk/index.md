---
title: "EBK: A Modern eBook Management System with AI-Powered Features"
date: 2025-10-13
draft: false
series: ["the-long-echo"]
series_weight: 25
tags: ['Python', 'ebook-manager', 'SQLite', 'SQLAlchemy', 'full-text-search', 'knowledge-graphs', 'semantic-search', 'MCP', 'AI', 'cli', 'data-management']
categories: ['software-development', 'data-science', 'artificial-intelligence', 'tools']
linked_project: [ebk]
description: "EBK is a comprehensive eBook metadata management tool that combines a robust SQLite backend with AI-powered features including knowledge graphs, semantic search, and MCP server integration for AI assistants."
---

Managing a large eBook collection can quickly become overwhelming. Different formats, inconsistent metadata, duplicate files, and the challenge of actually *finding* what you need when you need it—these problems compound as your library grows.

**[EBK](https://github.com/queelius/ebk)** addresses these challenges with a modern, Python-based solution that treats your eBook library as a queryable, searchable, AI-enhanced knowledge base.

Your books represent decades of accumulated knowledge—technical references, formative texts, research that shaped your thinking. They deserve better than scattered files on a hard drive. EBK is part of the [Long Echo](/post/2025-01-long-echo/) toolkit: tools for preserving your digital intellectual life in formats you control, queryable long after the cloud services have changed their APIs.

## The Core Value Proposition

EBK isn't just another file organizer. It's built on several key principles:

### 1. Database-First Architecture

At its heart, EBK uses **SQLAlchemy + SQLite** with a normalized schema and proper relationships. This means:

- **Fast queries** through indexed fields and relationships
- **FTS5 full-text search** across titles, descriptions, and extracted content
- **Transaction safety** ensuring your metadata is never corrupted
- **SQL power** when you need complex queries

### 2. Automatic Intelligence

The system automatically extracts and processes information:

```python
# Simple import with auto-extraction
ebk db-import my-book.pdf ~/my-library
```

Behind the scenes, EBK:
- **Extracts text** from PDFs (PyMuPDF + pypdf fallback) and EPUBs (ebooklib)
- **Generates chunks** (500-word overlapping segments) for semantic search
- **Computes SHA256 hashes** for deduplication
- **Extracts covers** (first page for PDFs, metadata for EPUBs)
- **Creates thumbnails** for display
- **Indexes everything** in FTS5 for instant search

### 3. Hash-Based Deduplication

EBK implements intelligent file handling:

```
Same file (same hash)          → Skip (already imported)
Same book, different format    → Add as additional format
Different book                 → Import as new entry
```

Books are stored in hash-prefixed directories (`ab/cd/ef/abcdef123456...pdf`) for scalability—no more massive flat directories.

## The Fluent Python API

EBK provides a powerful, chainable API for programmatic access:

```python
from ebk import Library

# Open library and build complex queries
lib = Library.open("~/ebooks")

results = (lib.query()
    .where("language", "en")
    .where("date", "2020", ">=")
    .where("subjects", "Python", "contains")
    .order_by("title")
    .take(10)
    .execute())

# Method chaining for operations
(lib.filter(lambda e: e.get("rating", 0) >= 4)
    .tag_all("recommended")
    .export_to_hugo("/path/to/site", organize_by="subject"))
```

This API makes it trivial to:
- Build complex queries without writing SQL
- Filter and transform collections
- Export subsets to different formats
- Analyze reading patterns and statistics

## AI-Powered Features

EBK's optional AI features transform it from a management tool into a knowledge assistant:

### Knowledge Graphs

Using **NetworkX**, EBK can extract and visualize concept relationships:

```python
# Extract knowledge graph from library
graph = lib.build_knowledge_graph(
    extract_entities=True,
    min_connection_strength=0.3
)

# Visualize relationships between topics
graph.visualize(output="library_knowledge.html")
```

This reveals hidden connections: "These books about functional programming also discuss category theory."

### Semantic Search

Beyond keyword matching, EBK can find books by *meaning*:

```python
# Find books semantically similar to a query
results = lib.semantic_search(
    "explaining complex mathematical concepts simply",
    threshold=0.7
)
```

The system uses vector embeddings (with TF-IDF fallback for offline use) to understand intent, not just keywords.

### Reading Companion

Track your reading journey with timestamps and context:

```python
session = lib.start_reading_session(book_id)
session.add_note("Chapter 3: Key insight about...", page=42)
session.complete()

# Later: "What was I reading about X-rays last month?"
lib.search_reading_history("X-rays", date_range="last_month")
```

## MCP Server Integration

One of EBK's most powerful features is its **Model Context Protocol (MCP) server**:

```bash
pip install ebk[mcp]
# Configure your AI assistant (Claude, etc.) to use the MCP server
```

This allows AI assistants to:
- Query your library directly
- Retrieve relevant passages during conversations
- Suggest books based on discussion context
- Answer questions using your personal knowledge base

Imagine asking your AI: "What books do I have about transformer architectures?" and getting instant, accurate results from *your own library*.

## Rich CLI Experience

EBK uses **Typer + Rich** for a beautiful command-line experience:

```bash
# Initialize new library
ebk db-init ~/my-library

# Import from Calibre
ebk db-import-calibre ~/Calibre/Library ~/my-library

# Full-text search with colorized output
ebk db-search "quantum computing" ~/my-library

# Statistics with rich tables
ebk db-stats ~/my-library
```

The CLI features:
- **Progress bars** for long operations
- **Colorized output** for readability
- **Clickable file links** (terminal permitting)
- **Rich tables** for results
- **JSON output** when scripting

## Flexible Export Options

EBK can transform your library into various formats:

### Hugo Static Site

```bash
ebk export hugo ~/library ~/hugo-site \
    --jinja \
    --organize-by subject \
    --include-covers \
    --include-files
```

Creates a browsable website with:
- Subject-based organization
- Cover image display
- Full-text content
- Download links

### Symlink DAG

```bash
ebk export-dag ~/library ~/output
```

Creates a navigable directory structure where:
- Tags become folders
- Books appear via symlinks
- Multiple paths lead to the same book
- File managers can browse it naturally

## Import from Anywhere

EBK handles multiple import sources:

```bash
# From Calibre library
ebk db-import-calibre ~/Calibre/Library ~/my-library

# Individual files with auto-extraction
ebk db-import book.pdf ~/my-library

# Batch import with progress tracking
ebk db-import ~/Downloads/*.epub ~/my-library

# From ZIP archives
ebk import-zip library-backup.zip --output-dir ~/my-library
```

Each import source is handled appropriately:
- **Calibre**: Reads `metadata.opf` files
- **Raw files**: Auto-extracts metadata from PDF/EPUB
- **ZIP archives**: Preserves existing EBK structure

## Set-Theoretic Operations

Merge multiple libraries with mathematical precision:

```bash
# Union: All unique books from all libraries
ebk merge union ~/merged ~/lib1 ~/lib2 ~/lib3

# Intersection: Only books present in ALL libraries
ebk merge intersect ~/common ~/lib1 ~/lib2

# Difference: Books in lib1 NOT in lib2
ebk merge diff ~/lib1-only ~/lib1 ~/lib2

# Symmetric difference: Books in exactly ONE library
ebk merge symdiff ~/unique ~/lib1 ~/lib2
```

Perfect for:
- Consolidating backups
- Finding duplicates across libraries
- Identifying unique collections
- Deduplicating merged sources

## Architecture: Modular and Extensible

EBK follows a clean, layered design:

```
┌─────────────────────────────────────┐
│        Integrations Layer           │  ← Streamlit, MCP, Viz
├─────────────────────────────────────┤
│           CLI Layer                 │  ← Typer commands
├─────────────────────────────────────┤
│        Core Library Layer           │  ← Fluent API
├─────────────────────────────────────┤
│      Import/Export Layer            │  ← Format handlers
├─────────────────────────────────────┤
│        Database Layer               │  ← SQLAlchemy + SQLite
└─────────────────────────────────────┘
```

This design ensures:
- **Core remains lightweight** (minimal dependencies)
- **Extensions are optional** (`pip install ebk[streamlit]`)
- **APIs are stable** (database changes don't affect CLI)
- **Testing is isolated** (each layer independently testable)

## Optional Integrations

### Streamlit Dashboard

```bash
pip install ebk[streamlit]
streamlit run -m ebk.integrations.streamlit.app -- ~/library
```

Provides a web interface for:
- Visual browsing with cover thumbnails
- Advanced search and filtering
- Statistics and visualizations
- Batch operations
- Export management

### Visualization Tools

```bash
pip install ebk[viz]
```

Enables:
- **Network graphs** showing book relationships
- **Tag clouds** for subject distribution
- **Timeline views** of publication dates
- **Author collaboration networks**

## Real-World Use Cases

### Academic Research

```python
lib = Library.open("~/research-library")

# Find papers on a specific topic with high citations
papers = (lib.query()
    .where("subjects", "machine-learning", "contains")
    .where("custom.citations", 100, ">=")
    .order_by("date", desc=True)
    .execute())

# Export to Hugo for lab website
lib.export_to_hugo("/lab-website/content/papers")
```

### Personal Knowledge Base

```python
# Build knowledge graph of your interests
graph = lib.build_knowledge_graph()

# Find books that bridge two topics
bridges = graph.find_bridges("programming", "philosophy")

# Get reading recommendations
recommended = lib.recommend(
    based_on=["favorite_book_1", "favorite_book_2"],
    diversity=0.6  # Balance similarity with variety
)
```

### Curation and Sharing

```python
# Create curated collections
lib.filter(lambda b: "beginner-friendly" in b.get("tags", []))
   .tag_all("recommended-for-beginners")
   .export_to_zip("beginner-collection.zip")

# Share with Calibre users
lib.export_to_calibre("/shared/calibre-library")
```

## Development Philosophy

EBK embraces modern Python best practices:

- **Type hints throughout** for IDE support
- **Comprehensive testing** with pytest
- **Documentation** via docstrings and Markdown
- **Makefile automation** for common tasks
- **Modular design** for easy extension

```bash
# Development workflow
make venv      # Create virtual environment
make setup     # Install in development mode
make test      # Run test suite
make coverage  # Check coverage
```

## Future Directions

EBK is actively developed with several exciting features planned:

- **Enhanced AI features**: Question generation for active recall
- **Mobile app**: React Native app using SQLite directly
- **Cloud sync**: Optional encrypted cloud backup
- **Plugin system**: Community-contributed importers/exporters
- **Advanced analytics**: Reading time tracking, difficulty estimation

## Getting Started

```bash
# Install with all features
pip install ebk[all]

# Initialize a library
ebk db-init ~/my-library

# Import your first book
ebk db-import ~/Documents/book.pdf ~/my-library

# Search and explore
ebk db-search "topic" ~/my-library
ebk db-stats ~/my-library
```

## Why EBK?

The eBook ecosystem is fragmented. Files are scattered, metadata is inconsistent, and powerful operations (like "find all books about X published after Y that I haven't read") are impossible without custom scripts.

EBK provides:
- **A single source of truth** for your eBook metadata
- **Powerful querying** without writing SQL
- **AI-enhanced discovery** for better insights
- **Format independence** (import/export to many formats)
- **Future-proof storage** (open SQLite + documented schema)

Your books represent significant investment—both financial and intellectual. They deserve better than a pile of files in a directory.

EBK treats your library as what it truly is: a personal knowledge base worth curating, querying, and exploring.

---

**Resources:**
- [GitHub Repository](https://github.com/queelius/ebk)
- [Project Page](/projects/ebk/)
- [Author's Website](https://metafunctor.com)

**Questions or feedback?** Open an issue on GitHub or reach out at lex@metafunctor.com.

Happy reading! 📚✨
