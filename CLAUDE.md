# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a comprehensive collection of **2,053 n8n workflow automation files** with an advanced documentation and search system. The repository features both Python (FastAPI) and Node.js (Express) implementations for browsing, searching, and analyzing n8n workflows.

## Development Commands

### Python-based System (Primary/Recommended)
```bash
# Install Python dependencies
pip install -r requirements.txt

# Start the FastAPI documentation server (recommended)
python run.py

# Development mode with auto-reload
python run.py --dev

# Custom host/port
python run.py --host 0.0.0.0 --port 3000

# Force database reindexing
python run.py --reindex

# Create workflow categorization data
python create_categories.py

# Import workflows into n8n instance
python import_workflows.py
```

### Node.js-based System (Alternative)
```bash
# Install Node.js dependencies
npm install

# Initialize database
npm run init

# Index workflows
npm run index

# Start Node.js server
npm start

# Development mode with auto-reload
npm run dev
```

### Database Operations
```bash
# Direct database operations (Python)
python workflow_db.py --index --force    # Force reindex all workflows
python workflow_db.py --stats            # Show database statistics
```

## Repository Architecture

### Core Structure
- **`workflows/`** - 2,053 n8n workflow JSON files organized by service/integration (e.g., `Telegram/`, `Gmail/`, `Shopify/`)
- **`context/`** - Category definitions and search data for workflow classification
- **`database/`** - SQLite database with FTS5 full-text search (auto-created)
- **`static/`** - Static web assets for the documentation interface

### Dual Implementation Architecture

**Python System (Primary)**:
- **`run.py`** - Main launcher with command-line interface
- **`api_server.py`** - FastAPI server with RESTful API endpoints
- **`workflow_db.py`** - Database operations using SQLite with FTS5 search
- **`create_categories.py`** - Workflow categorization system
- **`import_workflows.py`** - Bulk workflow import utility

**Node.js System (Alternative)**:
- **`src/server.js`** - Express.js server implementation
- **`src/database.js`** - SQLite database operations
- **`src/init-db.js`** - Database initialization
- **`src/index-workflows.js`** - Workflow indexing

### Workflow File Structure
Each workflow JSON contains:
- **name**: Human-readable workflow identifier
- **nodes**: Array of n8n node definitions with types, parameters, and connections
- **connections**: Graph structure defining data flow between nodes
- **active**: Boolean indicating if workflow is enabled
- **settings**: Workflow-level configuration options

## Key Features & Systems

### Advanced Search System
- **SQLite FTS5**: Full-text search across 29,445 workflow nodes
- **Sub-100ms responses**: Optimized database queries with proper indexing
- **Smart categorization**: 365 unique integrations across 15 service categories
- **Filter capabilities**: By trigger type (Webhook, Manual, Scheduled, Complex), complexity, and integration

### Workflow Categories
The system automatically categorizes workflows into these areas:
- **AI Agent Development** - OpenAI, Anthropic, LangChain, vector stores
- **Communication & Messaging** - Telegram, Discord, Slack, WhatsApp, email
- **Data Processing & Analysis** - Databases, ETL, analytics platforms
- **CRM & Sales** - Salesforce, HubSpot, Pipedrive
- **E-commerce & Retail** - Shopify, Stripe, PayPal
- **Marketing & Advertising** - Email marketing, campaign management
- **Project Management** - Jira, Trello, Asana, Monday.com
- **Social Media Management** - LinkedIn, Twitter, Facebook
- **Creative Content** - Design automation, video processing
- **Cloud Storage** - Google Drive, Dropbox, AWS S3
- **Technical Infrastructure** - DevOps, monitoring, CI/CD
- **Financial & Accounting** - QuickBooks, Xero, payment processing

### Performance Optimizations
- **Change detection**: MD5 hashing prevents unnecessary re-indexing
- **Background processing**: Non-blocking workflow analysis
- **Compressed responses**: Gzip middleware for optimal transfer speeds
- **Mobile optimization**: Responsive design with touch-friendly interface

## API Endpoints

### Core Search & Browse
- `GET /` - Main workflow browser interface
- `GET /api/workflows` - Search with query, filters, and pagination
- `GET /api/workflows/{filename}` - Individual workflow details
- `GET /api/workflows/{filename}/download` - Download workflow JSON
- `GET /api/stats` - Database statistics and metrics

### Advanced Features
- `GET /api/workflows/category/{category}` - Browse by service category
- `GET /api/categories` - List all available categories
- `GET /api/integrations` - Integration usage statistics
- `POST /api/reindex` - Trigger background database reindexing

## Workflow Naming Convention

The repository uses an intelligent naming system that converts technical filenames to readable titles:

**Pattern**: `[ID]_[Service1]_[Service2]_[Purpose]_[Trigger].json`

**Examples**:
- `2051_Telegram_Webhook_Automation_Webhook.json` → "Telegram Webhook Automation"
- `0250_HTTP_Discord_Import_Scheduled.json` → "HTTP Discord Import Scheduled"
- `0966_OpenAI_Data_Processing_Manual.json` → "OpenAI Data Processing Manual"

## Database Schema

### Workflows Table
```sql
CREATE TABLE workflows (
    id INTEGER PRIMARY KEY,
    filename TEXT UNIQUE,
    name TEXT,
    active BOOLEAN,
    trigger_type TEXT,
    complexity TEXT,
    node_count INTEGER,
    integrations TEXT,    -- JSON array of services used
    description TEXT,
    file_hash TEXT,       -- MD5 for change detection
    analyzed_at TIMESTAMP
);

-- Full-text search index
CREATE VIRTUAL TABLE workflows_fts USING fts5(
    filename, name, description, integrations, tags,
    content='workflows', content_rowid='id'
);
```

## Working with Workflows

### Security Considerations
- **Remove credentials**: All workflow files should have credentials/API keys removed
- **Webhook URLs**: Replace personal webhook URLs with placeholders
- **Test environment**: Always verify workflows in development before production use
- **Permissions**: Ensure proper access rights for all external integrations

### Import Process
1. Export workflow as JSON from n8n interface
2. Follow naming convention: `[ID]_[Service]_[Purpose]_[Trigger].json`
3. Remove sensitive data (credentials, personal URLs, API keys)
4. Place in appropriate service directory under `workflows/`
5. Run reindexing to update search database

### Integration Categories
When adding new workflows, ensure services are mapped in `context/def_categories.json` for proper categorization. The system will automatically detect services from filenames and assign appropriate categories.

## Common Tasks

### Adding New Workflow Categories
1. Edit `context/def_categories.json` to add service-to-category mappings
2. Run `python create_categories.py` to regenerate search categories
3. Restart the server to load new categorization data

### Debugging Search Issues
1. Check database stats: `python workflow_db.py --stats`
2. Force reindex if needed: `python run.py --reindex`
3. Verify workflow JSON format is valid
4. Check integration detection in filename parsing

### Performance Monitoring
- Monitor response times via `/api/stats` endpoint
- Database size should remain under 100MB for optimal performance
- FTS5 search handles up to 50k+ workflows efficiently

## Docker Deployment

### Quick Start with Docker
```bash
# Build and run with docker-compose (recommended)
docker-compose up -d

# Access the application at http://localhost:8000
# Database will be persisted in ./database/ directory
```

### Docker Commands
```bash
# Build and start the container
docker-compose up -d

# View logs
docker-compose logs -f n8n-workflows

# Stop the container
docker-compose down

# Rebuild after code changes
docker-compose up -d --build

# Use pre-built image (no build step)
docker-compose --profile prebuilt up -d n8n-workflows-prebuilt

# Force database reindexing in container
docker-compose exec n8n-workflows python run.py --reindex
```

### Docker Architecture
- **Base image**: `python:3.11-slim` for optimal size and security
- **Security**: Runs as non-root user (`appuser`)
- **Persistence**: Database volume mounted at `./database/`
- **Health checks**: Built-in health monitoring via `/api/stats` endpoint
- **Auto-restart**: Container restarts automatically unless manually stopped

### Volume Mounts
- `./database:/app/database` - Persists SQLite database between restarts
- `./workflows:/app/workflows:ro` - Read-only access to workflow files
- `./context:/app/context:ro` - Read-only access to category definitions

### Environment Variables
- `PYTHONUNBUFFERED=1` - Ensures real-time log output
- `WORKFLOW_DB_PATH=/app/database/workflows.db` - Database location

## Development Notes

- **Preferred system**: Use Python-based system (`run.py`) for primary development
- **Database**: SQLite with FTS5 provides excellent performance for this use case
- **Search optimization**: Index updates are incremental based on file modification times
- **Mobile support**: Interface is fully responsive and touch-optimized
- **Error handling**: Graceful degradation when workflows have parsing issues
- **Version compatibility**: Designed for n8n 1.0+ but includes backward compatibility
- **Docker support**: Fully containerized with persistence and health monitoring