## ADDED Requirements

### Requirement: SearXNG Service Integration
The system SHALL provide a SearXNG metasearch engine service for privacy-focused web search capabilities.

#### Scenario: SearXNG container startup
- **WHEN** docker-compose is started with SearXNG enabled
- **THEN** SearXNG container SHALL start successfully and respond to health checks

#### Scenario: SearXNG configuration
- **WHEN** SearXNG environment variables are properly configured
- **THEN** SearXNG SHALL use the specified settings for search behavior

### Requirement: SearXNG Environment Configuration
The system SHALL support configuration of SearXNG through environment variables for instance name, bind address, secret key, and search parameters.

#### Scenario: Instance name configuration
- **WHEN** SEARXNG_INSTANCE_NAME is set
- **THEN** SearXNG SHALL display the configured instance name

#### Scenario: Bind address configuration
- **WHEN** SEARXNG_BIND_ADDRESS is set
- **THEN** SearXNG SHALL listen on the specified address and port

#### Scenario: Secret key configuration
- **WHEN** SEARXNG_SECRET_KEY is provided
- **THEN** SearXNG SHALL use the key for session encryption

### Requirement: OpenWebUI SearXNG Integration
The system SHALL allow OpenWebUI to use SearXNG as a web search provider for RAG functionality.

#### Scenario: RAG web search with SearXNG
- **WHEN** ENABLE_RAG_WEB_SEARCH is true and SearXNG is configured
- **THEN** OpenWebUI SHALL be able to perform web searches through SearXNG

#### Scenario: Search result format
- **WHEN** OpenWebUI requests search results from SearXNG
- **THEN** SearXNG SHALL return results in JSON format suitable for RAG processing

### Requirement: SearXNG Privacy Settings
The system SHALL provide privacy-focused search configuration options including safe search, rate limiting, and search engine selection.

#### Scenario: Safe search configuration
- **WHEN** SEARXNG_SAFE_SEARCH is configured
- **THEN** SearXNG SHALL apply the specified safe search filtering level

#### Scenario: Search language configuration
- **WHEN** SEARXNG_SEARCH_LANGUAGE is set
- **THEN** SearXNG SHALL prioritize results in the specified language

## MODIFIED Requirements

### Requirement: RAG Web Search Configuration
The system SHALL support multiple web search providers including SearXNG, with configurable search engine selection.

#### Scenario: Search provider selection
- **WHEN** RAG_WEB_SEARCH_ENGINE is set to 'searxng'
- **THEN** OpenWebUI SHALL use SearXNG for web search operations

#### Scenario: SearXNG URL configuration
- **WHEN** SEARXNG_URL is provided
- **THEN** OpenWebUI SHALL connect to the specified SearXNG instance