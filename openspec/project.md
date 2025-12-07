# Project Context

## Purpose
This project provides a production-ready Docker Compose setup for deploying Open WebUI with external Ollama integration. It serves as a containerized deployment solution that enables users to run a web-based interface for interacting with large language models through Ollama, with features like RAG (Retrieval-Augmented Generation), authentication options, and comprehensive configuration management.

## Tech Stack
- **Docker & Docker Compose**: Container orchestration and deployment
- **Open WebUI**: Web-based interface for LLM interactions (ghcr.io/open-webui/open-webui:latest)
- **Ollama**: External LLM inference server integration
- **Shell Scripts**: Health monitoring and maintenance automation
- **Task**: Task runner for common operations (Taskfile.yml)
- **Environment Configuration**: Comprehensive .env-based configuration system

## Project Conventions

### Code Style
- **YAML Formatting**: 2-space indentation for docker-compose.yml and Taskfile.yml
- **Environment Variables**: UPPERCASE_WITH_UNDERSCORES naming convention
- **Shell Scripts**: Bash-compatible scripts with proper error handling
- **Documentation**: Comprehensive README with markdown formatting and clear sections

### Architecture Patterns
- **Containerized Deployment**: Single-service architecture with Open WebUI container
- **External Service Integration**: Ollama runs externally (host machine or network) for better resource management
- **Data Separation**: Persistent data in bind mounts (`./data`), volatile data in Docker volumes
- **Configuration Management**: Environment-based configuration with sensible defaults
- **Health Monitoring**: Built-in health checks with curl-based endpoint testing
- **Logging Strategy**: Structured JSON logging with rotation (max-size: 10m, max-file: 3)

### Testing Strategy
- **Health Checks**: Container-level health monitoring via HTTP endpoint (`/health`)
- **Connectivity Testing**: Manual curl commands for Ollama connectivity verification
- **Port Availability**: Manual checks for port conflicts before deployment
- **Log Monitoring**: Real-time log streaming for troubleshooting

### Git Workflow
- **Main Branch**: Single main branch for simplicity
- **Environment Files**: `.env` is gitignored, `.env.example` serves as template
- **Data Directory**: `data/` directory with `.gitkeep` for persistence
- **Documentation**: README-first approach with comprehensive setup instructions

## Domain Context
Open WebUI is a web-based interface that provides:
- Chat interface for multiple LLM models
- RAG capabilities for document-based question answering
- User management and authentication (optional)
- Model management and configuration
- Community features and sharing capabilities
- Image generation integration (optional)

The deployment integrates with Ollama, which serves as the LLM inference backend, allowing users to run local models like Llama 2, Mistral, CodeLlama, and others.

## Important Constraints
- **Network Security**: HTTP-only setup by default (no HTTPS certificates)
- **Authentication**: Disabled by default (`WEBUI_AUTH=false`) for local development
- **Port Requirements**: Default port 8080 must be available
- **Ollama Dependency**: Requires external Ollama instance running
- **Docker Requirements**: Docker Engine 20.10+ and Docker Compose 2.0+
- **Resource Usage**: 512MB-2GB memory, 1-2 CPU cores recommended
- **Data Persistence**: SQLite database stored in `./data` directory

## External Dependencies
- **Ollama API**: External LLM inference server (typically on port 11434)
- **Docker Engine**: Container runtime (version 20.10+)
- **Docker Compose**: Container orchestration (version 2.0+)
- **Task Runner**: Optional task automation (go-task.com)
- **Open WebUI Image**: ghcr.io/open-webui/open-webui:latest from GitHub Container Registry
- **Host System**: Linux/macOS/Windows with Docker support