# Change: Add SearXNG Web Search Integration

## Why
OpenWebUI currently supports RAG web search functionality but lacks integration with SearXNG, a privacy-focused metasearch engine. Adding SearXNG integration will provide users with a self-hosted, privacy-respecting web search option for RAG capabilities, enhancing data sovereignty and reducing dependency on commercial search APIs.

## What Changes
- Add SearXNG service to docker-compose.yml with proper configuration
- Add SearXNG-specific environment variables for web search configuration
- Update RAG web search settings to support SearXNG as a search provider
- Add SearXNG configuration documentation to README
- **BREAKING**: New required environment variables for SearXNG configuration

## Impact
- Affected specs: web-search, rag-configuration
- Affected code: docker-compose.yml, .env.example, README.md
- New dependency: SearXNG container service
- Additional memory usage: ~256MB for SearXNG container
- New configuration options for users wanting privacy-focused search