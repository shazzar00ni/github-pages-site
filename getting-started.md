---
layout: page
title: Getting Started
permalink: /getting-started/
---

# Getting Started with Paperlyte

Welcome to Paperlyte! This guide will help you get up and running quickly with your new documentation solution.

## Installation

### Prerequisites

Before installing Paperlyte, ensure you have:

- A modern web browser
- Text editor (VS Code, Sublime Text, or any preferred editor)
- Basic familiarity with Markdown syntax

### Quick Installation

1. **Download Paperlyte**
   ```bash
   # Clone or download the latest version
   git clone https://github.com/paperlyte/paperlyte.git
   cd paperlyte
   ```

2. **Setup Configuration**
   ```bash
   # Copy the default configuration
   cp config.example.yml config.yml
   ```

3. **Start Using Paperlyte**
   ```bash
   # Launch the application
   ./paperlyte serve
   ```

## First Document {#first-document}

Let's create your first document:

1. **Create a new file**: `my-first-doc.md`

2. **Add content**:
   ```markdown
   # My First Document
   
   This is my first document with Paperlyte!
   
   ## Features I love:
   - Simple markdown syntax
   - Fast rendering
   - Easy organization
   ```

3. **View your document**: Open your browser to see the rendered result

## Basic Configuration

### Project Settings

Edit your `config.yml` file:

```yaml
title: "My Documentation"
description: "Project documentation made simple"
author: "Your Name"
theme: "default"
```

### Organizing Content

Paperlyte uses a simple folder structure:

```
docs/
├── getting-started.md
├── features/
│   ├── basic-usage.md
│   └── advanced-features.md
└── api/
    ├── overview.md
    └── reference.md
```

## Next Steps

Now that you have Paperlyte running:

1. **[Explore Features](features.html)** - Learn about all available features
2. **[Check Examples](examples.html)** - See real-world usage examples
3. **[API Reference](api-reference.html)** - Dive into technical details

## Need Help?

- Check our [FAQ section](features.html#faq)
- Browse [examples](examples.html) for inspiration
- Report issues on our GitHub repository

---

*Ready to dive deeper? Continue to the [Features](features.html) section.*