---
title: "Long Echo in Practice: 5,874 Bookmarks in a Single File"
date: 2025-12-18
draft: false
series: ["the-long-echo"]
series_weight: 21
tags:
  - long-term-thinking
  - data-preservation
  - btk
  - python
  - bookmarks
  - personal-archival
categories:
  - Projects
  - Philosophy
linked_project: [btk]
description: "A concrete demonstration of graceful degradation: exporting years of bookmarks to a self-contained HTML app that works offline, forever."
---

I wrote about [Long Echo](/post/2025-01-long-echo/) and the [Long Echo Toolkit](/post/2025-12-16-long-echo-toolkit/) earlier. Today I want to show what it actually looks like in practice.

**[View the live demo: 5,874 bookmarks in a single file](/bookmarks.html)**

## The Export

```bash
btk --db bookmarks.db export bookmarks.html \
    --format html-app \
    --query "(reachable != 0 OR reachable IS NULL)"
```

**Result**: 5,874 bookmarks in a single 4MB HTML file.

## What You Get

Open it in any browser. No server. No internet. No dependencies. Just a file.

The `html-app` export includes:

- **Search**: Full-text filtering across titles, URLs, descriptions, tags
- **Multiple views**: Grid, list, table layouts
- **Tag sidebar**: Navigate by hierarchical tags
- **Dark mode**: Toggle with a button
- **Keyboard shortcuts**: Navigate without a mouse
- **Sorting**: By date, title, visits, stars
- **Filtering**: By starred, archived, has-content

Everything is embedded: CSS, JavaScript, all 5,874 bookmark records as JSON. One file.

## Why This Matters

This is graceful degradation made concrete:

| Level | What Works | Requirements |
|-------|-----------|--------------|
| **1. BTK CLI** | Full features, auto-tagging, content caching | Python, btk installed |
| **2. SQLite** | Direct queries, scripting | sqlite3 binary |
| **3. HTML App** | Visual browsing, search, filtering | Any browser |
| **4. View source** | Raw JSON data, greppable | Text editor |

The HTML app is level 3—it works even when BTK is gone, even when Python is gone. Someone in 2074 can double-click the file and browse my bookmarks.

## The Data Inside

If you view source, you'll find:

```javascript
const BOOKMARKS = [
    {
        "id": 1,
        "url": "https://example.com/article",
        "title": "Interesting Article",
        "description": "Notes about the article...",
        "tags": ["programming", "python"],
        "stars": 1,
        "created_at": "2023-05-12T14:32:00Z",
        "visited_count": 42
    },
    // ... 5,873 more
];
```

Plain JSON. No encoding tricks. Grep it, parse it with jq, import it into another tool. The data survives the interface.

## Try It

Install BTK:

```bash
pip install bookmark-tk
```

Export your bookmarks:

```bash
# From browser exports
btk import bookmarks.html --format html

# To self-contained app
btk export archive.html --format html-app
```

You now have a permanent, searchable copy of your bookmarks that will outlive every cloud service.

## Links

- **Live Demo**: [My Bookmarks Archive](/bookmarks.html) (5,874 bookmarks, 4MB)
- **BTK**: [github.com/queelius/btk](https://github.com/queelius/btk)
- **Long Echo Philosophy**: [Long Echo: Designing for Digital Resilience](/post/2025-01-long-echo/)
- **Full Toolkit**: [The Long Echo Toolkit](/post/2025-12-16-long-echo-toolkit/)

---

*Your bookmarks are worth more than "hope Chrome sync doesn't break." Export them to something permanent.*
