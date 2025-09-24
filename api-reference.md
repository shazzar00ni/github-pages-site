---
layout: page
title: API Reference
permalink: /api-reference/
---

# Paperlyte API Reference

Comprehensive technical documentation for developers integrating with Paperlyte.

## Overview

The Paperlyte API provides programmatic access to all core functionality, enabling:
- Document creation and management
- Content rendering and export
- Configuration management
- Plugin development

## Authentication

All API requests require authentication using API keys:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.paperlyte.dev/v1/documents
```

### Getting an API Key

1. Log into your Paperlyte dashboard
2. Navigate to Settings → API Keys
3. Generate a new key with appropriate permissions

## Core Endpoints

### Documents API

#### List Documents
```http
GET /v1/documents
```

**Parameters:**
- `page` (optional): Page number for pagination
- `limit` (optional): Number of results per page (max 100)
- `folder` (optional): Filter by folder path

**Response:**
```json
{
  "documents": [
    {
      "id": "doc_123",
      "title": "Getting Started",
      "path": "/getting-started.md",
      "created_at": "2025-01-01T00:00:00Z",
      "updated_at": "2025-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "total_pages": 5,
    "total_count": 47
  }
}
```

#### Create Document
```http
POST /v1/documents
```

**Request Body:**
```json
{
  "title": "New Document",
  "content": "# Hello World\n\nThis is my new document.",
  "path": "/docs/new-document.md",
  "tags": ["tutorial", "beginner"]
}
```

#### Get Document
```http
GET /v1/documents/{id}
```

**Response:**
```json
{
  "id": "doc_123",
  "title": "Getting Started",
  "content": "# Getting Started\n\nWelcome to...",
  "html": "<h1>Getting Started</h1><p>Welcome to...</p>",
  "path": "/getting-started.md",
  "tags": ["guide", "introduction"],
  "created_at": "2025-01-01T00:00:00Z",
  "updated_at": "2025-01-15T10:30:00Z"
}
```

#### Update Document
```http
PUT /v1/documents/{id}
```

#### Delete Document
```http
DELETE /v1/documents/{id}
```

### Rendering API

#### Render Markdown
```http
POST /v1/render
```

**Request Body:**
```json
{
  "content": "# Hello\n\nThis is **bold** text.",
  "format": "html"
}
```

**Response:**
```json
{
  "html": "<h1>Hello</h1><p>This is <strong>bold</strong> text.</p>",
  "toc": [
    {
      "level": 1,
      "title": "Hello",
      "anchor": "hello"
    }
  ]
}
```

### Export API

#### Export Document
```http
POST /v1/documents/{id}/export
```

**Request Body:**
```json
{
  "format": "pdf",
  "options": {
    "theme": "default",
    "include_toc": true,
    "page_size": "A4"
  }
}
```

### Search API

#### Search Documents
```http
GET /v1/search?q={query}
```

**Parameters:**
- `q`: Search query
- `type`: Filter by content type (optional)
- `tags`: Filter by tags (optional)

**Response:**
```json
{
  "results": [
    {
      "id": "doc_123",
      "title": "Getting Started",
      "excerpt": "...quick start guide for...",
      "score": 0.95,
      "matches": [
        {
          "field": "content",
          "snippet": "...highlighted search terms..."
        }
      ]
    }
  ],
  "total": 15,
  "query_time": 0.023
}
```

## Configuration API

### Get Configuration
```http
GET /v1/config
```

### Update Configuration
```http
PUT /v1/config
```

**Request Body:**
```json
{
  "site": {
    "title": "My Documentation",
    "description": "Project documentation",
    "theme": "default"
  },
  "features": {
    "search_enabled": true,
    "comments_enabled": false,
    "export_enabled": true
  }
}
```

## Webhooks

Paperlyte can send webhook notifications for various events:

### Available Events
- `document.created`
- `document.updated`
- `document.deleted`
- `export.completed`

### Webhook Payload
```json
{
  "event": "document.updated",
  "timestamp": "2025-01-15T10:30:00Z",
  "data": {
    "document": {
      "id": "doc_123",
      "title": "Updated Document",
      "path": "/docs/updated.md"
    },
    "changes": ["title", "content"]
  }
}
```

## SDKs and Libraries

### Official SDKs
- **JavaScript/TypeScript**: `npm install @paperlyte/sdk`
- **Python**: `pip install paperlyte-sdk`
- **Go**: `go get github.com/paperlyte/go-sdk`

### JavaScript Example
```javascript
import { PaperlyteClient } from '@paperlyte/sdk';

const client = new PaperlyteClient({
  apiKey: 'your-api-key',
  baseUrl: 'https://api.paperlyte.dev'
});

// Create a document
const doc = await client.documents.create({
  title: 'New Document',
  content: '# Hello World',
  path: '/docs/hello.md'
});

console.log('Created document:', doc.id);
```

## Rate Limits

API requests are rate limited to prevent abuse:

- **Free tier**: 1,000 requests per hour
- **Pro tier**: 10,000 requests per hour
- **Enterprise**: Custom limits

Rate limit headers are included in all responses:
```http
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 999
X-RateLimit-Reset: 1640995200
```

## Error Handling

The API uses standard HTTP status codes and returns detailed error information:

```json
{
  "error": {
    "code": "validation_failed",
    "message": "The request body contains invalid data",
    "details": [
      {
        "field": "title",
        "message": "Title is required"
      }
    ]
  }
}
```

### Common Error Codes
- `400` - Bad Request (invalid parameters)
- `401` - Unauthorized (invalid API key)
- `403` - Forbidden (insufficient permissions)
- `404` - Not Found (resource doesn't exist)
- `429` - Too Many Requests (rate limit exceeded)
- `500` - Internal Server Error

---

*Need help with integration? Check our [Examples](examples.html) for practical implementation guides.*