---
layout: page
title: Examples
permalink: /examples/
---

# Paperlyte Examples

Real-world examples and tutorials to help you make the most of Paperlyte.

## Basic Usage Examples

### Personal Knowledge Base

Create a personal wiki for notes, ideas, and research:

```markdown
# My Knowledge Base

## Daily Notes
- [2025-01-15](daily/2025-01-15.md) - Project kickoff meeting
- [2025-01-14](daily/2025-01-14.md) - Research findings

## Project Ideas
- [App Concept](ideas/mobile-app.md)
- [Book Outline](ideas/technical-book.md)

## Learning Resources
- [JavaScript Notes](learning/javascript.md)
- [Design Patterns](learning/design-patterns.md)
```

### Team Documentation

Set up documentation for a development team:

```yaml
# _config.yml
title: "Team Alpha Documentation"
description: "Internal documentation for Team Alpha"

sections:
  - name: "Onboarding"
    path: "/onboarding/"
  - name: "Development"
    path: "/dev/"
  - name: "Deployment"
    path: "/deploy/"
```

## Advanced Examples

### API Documentation Site

Create comprehensive API documentation:

```markdown
# Payment API

## Authentication
All requests require an API key in the header:
```http
Authorization: Bearer sk_live_123...
```

## Create Payment
```http
POST /v1/payments
Content-Type: application/json

{
  "amount": 2000,
  "currency": "usd",
  "payment_method": "card_123"
}
```

**Response:**
```json
{
  "id": "pay_123",
  "status": "succeeded",
  "amount": 2000
}
```
```

### Technical Blog

Transform Paperlyte into a technical blog:

```markdown
---
layout: post
title: "Building Scalable APIs"
date: 2025-01-15
author: "Jane Developer"
tags: [api, scaling, architecture]
---

# Building Scalable APIs

In this post, we'll explore patterns for building APIs that can handle growth...

## Key Principles

1. **Stateless Design**
2. **Horizontal Scaling**
3. **Caching Strategies**

## Implementation Example

```python
from flask import Flask, jsonify
from flask_caching import Cache

app = Flask(__name__)
cache = Cache(app, config={'CACHE_TYPE': 'redis'})

@app.route('/api/data')
@cache.cached(timeout=300)
def get_data():
    return jsonify(expensive_computation())
```
```

## Integration Examples

### GitHub Actions Workflow

Automate documentation updates with GitHub Actions:

```yaml
# .github/workflows/docs.yml
name: Update Documentation

on:
  push:
    branches: [main]
    paths: ['docs/**']

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Setup Paperlyte
        run: |
          curl -L https://github.com/paperlyte/paperlyte/releases/latest/download/paperlyte-linux.tar.gz | tar xz
          ./paperlyte configure --site-url ${{ secrets.SITE_URL }}
      
      - name: Build and Deploy
        run: |
          ./paperlyte build
          ./paperlyte deploy --target s3 --bucket ${{ secrets.S3_BUCKET }}
```

### Docker Setup

Run Paperlyte in a container:

```dockerfile
# Dockerfile
FROM node:16-alpine

WORKDIR /app

# Install Paperlyte
RUN npm install -g @paperlyte/cli

# Copy documentation
COPY docs/ ./docs/
COPY _config.yml ./

# Build the site
RUN paperlyte build

EXPOSE 3000

CMD ["paperlyte", "serve", "--host", "0.0.0.0", "--port", "3000"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  paperlyte:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ./docs:/app/docs
      - ./_config.yml:/app/_config.yml
    environment:
      - NODE_ENV=production
```

## Plugin Examples

### Custom Theme

Create a custom theme for your documentation:

```css
/* themes/custom/style.css */
:root {
  --primary-color: #2563eb;
  --secondary-color: #64748b;
  --background-color: #f8fafc;
  --text-color: #1e293b;
}

.header {
  background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
  color: white;
  padding: 2rem 0;
}

.sidebar {
  background: var(--background-color);
  border-right: 1px solid #e2e8f0;
}

.content {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

code {
  background: #f1f5f9;
  padding: 0.125rem 0.25rem;
  border-radius: 0.25rem;
  font-family: 'Monaco', 'Menlo', monospace;
}
```

### Search Plugin

Add custom search functionality:

```javascript
// plugins/enhanced-search.js
class EnhancedSearch {
  constructor(options) {
    this.options = options;
    this.index = [];
  }

  async initialize() {
    // Build search index
    const documents = await this.loadDocuments();
    this.index = this.buildIndex(documents);
  }

  search(query) {
    const results = this.index
      .filter(doc => doc.content.toLowerCase().includes(query.toLowerCase()))
      .map(doc => ({
        ...doc,
        score: this.calculateScore(doc, query),
        excerpt: this.generateExcerpt(doc.content, query)
      }))
      .sort((a, b) => b.score - a.score);

    return results;
  }

  calculateScore(doc, query) {
    const titleMatch = doc.title.toLowerCase().includes(query.toLowerCase()) ? 2 : 0;
    const contentMatch = (doc.content.toLowerCase().match(new RegExp(query.toLowerCase(), 'g')) || []).length;
    return titleMatch + contentMatch;
  }
}

// Register plugin
paperlyte.plugins.register('enhanced-search', EnhancedSearch);
```

## Migration Examples

### From GitBook

Migrate content from GitBook to Paperlyte:

```bash
#!/bin/bash
# migrate-from-gitbook.sh

# Convert GitBook SUMMARY.md to Paperlyte navigation
python3 << 'EOF'
import re

with open('SUMMARY.md', 'r') as f:
    content = f.read()

# Extract navigation structure
nav_items = []
for line in content.split('\n'):
    if '* [' in line:
        match = re.search(r'\* \[(.*?)\]\((.*?)\)', line)
        if match:
            title, path = match.groups()
            nav_items.append(f'  - {path}')

# Generate _config.yml navigation
config = f"""
title: "Migrated Documentation"
description: "Documentation migrated from GitBook"

header_pages:
{chr(10).join(nav_items)}
"""

with open('_config.yml', 'w') as f:
    f.write(config)

print("Migration complete! Review _config.yml and adjust as needed.")
EOF
```

### From Confluence

Export and convert Confluence pages:

```python
# confluence_to_paperlyte.py
import requests
import html2text
from pathlib import Path

class ConfluenceMigrator:
    def __init__(self, base_url, username, api_token):
        self.base_url = base_url
        self.auth = (username, api_token)
        self.h2t = html2text.HTML2Text()
        self.h2t.ignore_links = False
        
    def export_space(self, space_key, output_dir):
        pages = self.get_space_pages(space_key)
        
        for page in pages:
            content = self.get_page_content(page['id'])
            markdown = self.h2t.handle(content['body']['view']['value'])
            
            # Create file path
            file_path = Path(output_dir) / f"{page['title'].replace(' ', '-').lower()}.md"
            
            # Write markdown file
            with open(file_path, 'w', encoding='utf-8') as f:
                f.write(f"---\ntitle: {page['title']}\n---\n\n")
                f.write(markdown)
                
        print(f"Exported {len(pages)} pages to {output_dir}")

# Usage
migrator = ConfluenceMigrator(
    'https://yourcompany.atlassian.net/wiki',
    'your-email@company.com',
    'your-api-token'
)

migrator.export_space('PROJ', './docs')
```

## Best Practices Examples

### Content Organization

Structure your documentation for maximum usability:

```
docs/
├── index.md                 # Homepage
├── getting-started/
│   ├── installation.md
│   ├── quick-start.md
│   └── first-project.md
├── guides/
│   ├── user-guide/
│   ├── admin-guide/
│   └── developer-guide/
├── api/
│   ├── overview.md
│   ├── authentication.md
│   └── endpoints/
├── examples/
│   ├── basic-usage.md
│   ├── advanced-features.md
│   └── integrations.md
└── reference/
    ├── configuration.md
    ├── troubleshooting.md
    └── faq.md
```

### Content Templates

Create templates for consistent documentation:

```markdown
<!-- template-api-endpoint.md -->
---
title: "{{ endpoint_name }}"
category: "API Reference"
---

# {{ endpoint_name }}

{{ brief_description }}

## Request

```http
{{ http_method }} {{ endpoint_path }}
Content-Type: application/json
Authorization: Bearer {{ auth_token }}

{{ request_body }}
```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| {{ param_name }} | {{ param_type }} | {{ required }} | {{ param_description }} |

## Response

```json
{{ response_example }}
```

## Error Codes

| Code | Description |
|------|-------------|
| {{ error_code }} | {{ error_description }} |

## Examples

### Success Response
{{ success_example }}

### Error Response
{{ error_example }}
```

---

*These examples should give you a solid foundation for using Paperlyte effectively. Need more specific help? Check our [Getting Started](getting-started.html) guide or [API Reference](api-reference.html).*