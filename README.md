# Open WebUI Docker Compose Setup

A production-ready Docker Compose setup for deploying [Open WebUI](https://github.com/open-webui/open-webui) with external Ollama integration.

## 🚀 Features

- ✅ Complete Docker Compose configuration
- ✅ All settings configurable via `.env` file
- ✅ Persistent data storage in `data/` directory
- ✅ Volatile data (cache, vector store) in Docker volumes
- ✅ External Ollama API integration
- ✅ HTTP-only setup (no HTTPS certificates required)
- ✅ Built-in health monitoring
- ✅ Automatic container restart
- ✅ Structured logging

## 📋 Prerequisites

- Docker Engine 20.10+
- Docker Compose 2.0+
- External Ollama instance running (see [Ollama Setup](#ollama-setup))

## 🏗️ Project Structure

```
.
├── docker-compose.yml      # Docker Compose configuration
├── .env                    # Environment variables (gitignored)
├── .env.example           # Environment template
├── healthcheck.sh         # Health monitoring script
├── data/                  # Persistent data directory
│   └── .gitkeep          # Git placeholder
└── README.md             # This file
```

## ⚙️ Quick Start

### 1. Clone/Download the Project

```bash
cd /path/to/openwebui
```

### 2. Configure Environment Variables

Copy the example environment file and customize it:

```bash
cp .env.example .env
```

Edit `.env` and configure the following key settings:

```bash
# External Ollama API endpoint
OLLAMA_BASE_URL=http://host.docker.internal:11434

# Port for Open WebUI
OPENWEBUI_PORT=8080

# Generate a secure secret key
WEBUI_SECRET_KEY=$(openssl rand -hex 32)
```

### 3. Start the Service

```bash
docker-compose up -d
```

### 4. Access Open WebUI

Open your browser and navigate to:

```
http://localhost:8080
```

Since authentication is disabled by default, you'll have direct access to the interface.

## 🔧 Configuration

### Environment Variables

All configuration is managed through the `.env` file. Key variables include:

#### Network Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `OPENWEBUI_PORT` | Host port for web access | `8080` |
| `OLLAMA_BASE_URL` | External Ollama API endpoint | `http://host.docker.internal:11434` |

#### Authentication & Security

| Variable | Description | Default |
|----------|-------------|---------|
| `WEBUI_AUTH` | Enable authentication | `false` |
| `ENABLE_SIGNUP` | Allow new user signups | `false` |
| `WEBUI_SECRET_KEY` | Session encryption key | `change-this-to-a-secure-random-string` |
| `DEFAULT_USER_ROLE` | Default role for users | `user` |

#### Model Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `DEFAULT_MODELS` | Comma-separated model list | (empty - shows all) |

#### RAG Settings

| Variable | Description | Default |
|----------|-------------|---------|
| `ENABLE_RAG_WEB_SEARCH` | Enable web search | `false` |
| `RAG_EMBEDDING_MODEL` | Embedding model | `sentence-transformers/all-MiniLM-L6-v2` |
| `CHUNK_SIZE` | Document chunk size | `1500` |
| `CHUNK_OVERLAP` | Chunk overlap | `100` |

#### Logging

| Variable | Description | Default |
|----------|-------------|---------|
| `WEBUI_LOG_LEVEL` | Log level | `INFO` |

For a complete list of available variables, see [`.env.example`](.env.example).

## 💾 Data Persistence

### Persistent Data (`data/` directory)

The following data is stored in the `./data` directory and persists across container restarts:

- User profiles and settings
- Chat history and conversations
- Custom prompts and templates
- Model configurations
- Database files (SQLite)

### Volatile Data (Docker Volumes)

The following data is stored in Docker volumes and recreated on container restart:

- `openwebui_cache` - Application cache
- `openwebui_vector_db` - Vector embeddings for RAG

## 🔌 Ollama Setup

### Option 1: Ollama on Host Machine (Recommended)

If Ollama is running on your host machine:

```bash
# In .env file
OLLAMA_BASE_URL=http://host.docker.internal:11434
```

### Option 2: Ollama on Network

If Ollama is running on another machine in your network:

```bash
# In .env file
OLLAMA_BASE_URL=http://192.168.1.XXX:11434
```

### Verify Ollama Connection

```bash
# Test from your host
curl http://localhost:11434/api/tags

# Test from container
docker exec openwebui curl http://host.docker.internal:11434/api/tags
```

## 🏥 Health Monitoring

### Check Container Health

Use the included health check script:

```bash
./healthcheck.sh
```

This will display:
- Docker status
- Container status
- Health check status
- Port mappings
- Recent logs
- HTTP endpoint test

### Manual Health Check

```bash
# Check container status
docker-compose ps

# View logs
docker-compose logs -f

# Check health endpoint
curl http://localhost:8080/health
```

## 🛠️ Common Operations

### Start Services

```bash
docker-compose up -d
```

### Stop Services

```bash
docker-compose down
```

### Restart Services

```bash
docker-compose restart
```

### View Logs

```bash
# Follow all logs
docker-compose logs -f

# View last 100 lines
docker-compose logs --tail 100

# View specific service logs
docker-compose logs -f openwebui
```

### Update Open WebUI

```bash
# Pull latest image
docker-compose pull

# Restart with new image
docker-compose up -d
```

### Clean Volatile Data

To reset cache and vector database:

```bash
# Stop and remove volumes
docker-compose down -v

# Start fresh
docker-compose up -d
```

## 🔒 Security Considerations

⚠️ **Important Security Notes:**

1. **No Authentication**: This setup has authentication disabled (`WEBUI_AUTH=false`)
   - Only use on trusted networks
   - Enable authentication for production use

2. **Secret Key**: Change the default `WEBUI_SECRET_KEY` to a secure random string:
   ```bash
   openssl rand -hex 32
   ```

3. **Network Security**: This setup uses HTTP only
   - Use a reverse proxy (nginx, Traefik) for HTTPS in production
   - Keep Open WebUI behind a firewall for local use

## 📊 Resource Usage

Typical resource requirements:

- **Memory**: 512MB - 2GB (depending on usage)
- **CPU**: 1-2 cores
- **Disk**: 
  - Base image: ~500MB
  - Data: Varies (chat history, embeddings)
  - Volumes: ~100MB - 1GB

## 🧹 Maintenance

### Backup Data

```bash
# Backup persistent data
tar -czf openwebui-backup-$(date +%Y%m%d).tar.gz data/

# Backup environment
cp .env .env.backup
```

### Restore Data

```bash
# Stop container
docker-compose down

# Restore data
tar -xzf openwebui-backup-YYYYMMDD.tar.gz

# Start container
docker-compose up -d
```

### Clean Up Unused Resources

```bash
# Remove unused images
docker image prune -a

# Remove unused volumes
docker volume prune
```

## 🐛 Troubleshooting

### Container Won't Start

```bash
# Check logs
docker-compose logs openwebui

# Verify env file
cat .env

# Check Docker resources
docker system df
```

### Can't Connect to Ollama

```bash
# Test Ollama from host
curl http://localhost:11434/api/tags

# Test from container
docker exec openwebui curl http://host.docker.internal:11434/api/tags

# Check Ollama is running
ps aux | grep ollama
```

### Port Already in Use

```bash
# Check what's using port 8080
sudo lsof -i :8080

# Change port in .env
OPENWEBUI_PORT=3000
docker-compose up -d
```

### Permission Issues

```bash
# Fix data directory permissions
sudo chown -R $(id -u):$(id -g) data/
```

## 📚 Additional Resources

- [Open WebUI Documentation](https://docs.openwebui.com/)
- [Ollama Documentation](https://ollama.ai/docs)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 📝 License

This configuration is provided as-is for use with Open WebUI. See the [Open WebUI repository](https://github.com/open-webui/open-webui) for license information.

## 🤝 Contributing

Feel free to submit issues and enhancement requests!