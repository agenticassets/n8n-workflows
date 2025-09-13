# CLAUDE.md

**Claude Code Guide for n8n Automation Tools**

This repository combines two powerful n8n automation tools that work together seamlessly:

1. **n8n MCP** (Model Context Protocol) - Live node discovery, configuration, and validation
2. **FastAPI Workflow Browser** - Static analysis of 2,056 workflow files with advanced search

## Tool Integration Strategy

Use these tools in sequence for maximum effectiveness:

**Discovery → Configuration → Validation**

1. **FastAPI** - Find workflow examples and patterns
2. **n8n MCP** - Configure and validate specific nodes
3. **n8n MCP** - Deploy to live n8n instance

## FastAPI Workflow Browser

**Purpose**: Analyze and search through 2,056 static workflow files

### Quick Start
```bash
# Start the server (should already be running)
python run.py

# Access at: http://localhost:8000
```

### Key Capabilities
- **Search**: Full-text search across 29,522 workflow nodes
- **Browse**: 16 categories (AI Agent Development, Communication, E-commerce, etc.)
- **Analyze**: Trigger types, complexity levels, integration patterns
- **Download**: Individual workflow JSON files

### Essential API Usage
```bash
# Search workflows by integration
curl "http://localhost:8000/api/workflows?query=OpenAI&limit=5"

# Browse by category
curl "http://localhost:8000/api/workflows/category/AI Agent Development"

# Get specific workflow details
curl "http://localhost:8000/api/workflows/{filename}"

# Database statistics
curl "http://localhost:8000/api/stats"
```

## n8n MCP (Model Context Protocol)

**Purpose**: Live n8n node discovery, configuration, and validation

### Current Status
- **535 total nodes** available (269 AI tools, 108 triggers)
- **88% documentation coverage**
- **Connected and operational**

### Essential MCP Functions

#### Discovery & Search
```typescript
// Find nodes by keyword
mcp__n8n-mcp__search_nodes({query: "OpenAI", limit: 5})

// List by category
mcp__n8n-mcp__list_nodes({category: "AI", limit: 20})

// List all AI-capable tools
mcp__n8n-mcp__list_ai_tools()

// Browse task templates
mcp__n8n-mcp__list_tasks()
```

#### Configuration & Validation
```typescript
// Get node essentials (lightweight)
mcp__n8n-mcp__get_node_essentials("nodes-base.openAi")

// Get complete node schema (detailed)
mcp__n8n-mcp__get_node_info("nodes-base.openAi")

// Validate node configuration
mcp__n8n-mcp__validate_node_operation("nodes-base.openAi", config)

// Validate complete workflow
mcp__n8n-mcp__validate_workflow(workflow)
```

#### Templates & Tasks
```typescript
// Get pre-configured node for task
mcp__n8n-mcp__get_node_for_task("chat_with_ai")

// Search workflow templates
mcp__n8n-mcp__search_templates("AI chatbot")

// Get complete template
mcp__n8n-mcp__get_template(templateId)
```

## Integrated Workflow Patterns

### 1. Discovery Phase (FastAPI)
**Find existing solutions and patterns**

```bash
# Search for AI workflows
curl "http://localhost:8000/api/workflows?query=OpenAI+ChatGPT&limit=10"

# Browse specific categories
curl "http://localhost:8000/api/workflows/category/AI Agent Development"

# Analyze workflow complexity
curl "http://localhost:8000/api/stats"
```

**Use cases:**
- Find similar workflow implementations
- Understand integration patterns
- Analyze complexity and node usage
- Discover naming conventions

### 2. Configuration Phase (n8n MCP)
**Configure and understand specific nodes**

```typescript
// After finding OpenAI workflows, configure OpenAI node
mcp__n8n-mcp__search_nodes({query: "OpenAI"})
mcp__n8n-mcp__get_node_essentials("nodes-base.openAi")

// Get task-specific configuration
mcp__n8n-mcp__get_node_for_task("chat_with_ai")

// Understand dependencies
mcp__n8n-mcp__get_property_dependencies("nodes-base.openAi")
```

**Use cases:**
- Understand node properties and requirements
- Get pre-configured templates
- Validate node configurations
- Learn about property dependencies

### 3. Validation Phase (n8n MCP)
**Validate before deployment**

```typescript
// Validate individual nodes
mcp__n8n-mcp__validate_node_operation("nodes-base.openAi", config)

// Validate complete workflow
mcp__n8n-mcp__validate_workflow(workflowJson)

// Check connections and structure
mcp__n8n-mcp__validate_workflow_connections(workflow)
```

**Use cases:**
- Ensure configuration correctness
- Validate workflow structure
- Check for missing properties
- Verify connections and flow

## Quick Reference

### FastAPI Endpoints (Static Analysis)
```bash
GET /api/workflows              # Search workflows
GET /api/workflows/{filename}   # Get workflow details
GET /api/categories             # List categories
GET /api/stats                  # Database statistics
```

### n8n MCP Functions (Live Data)
```typescript
// Essential discovery functions
search_nodes({query, limit})           // Find nodes by keyword
list_nodes({category, limit})          // List nodes by category
list_ai_tools()                        // All AI-capable nodes

// Essential configuration functions
get_node_essentials(nodeType)          // Key properties only
validate_node_operation(nodeType, config) // Validate configuration

// Essential template functions
list_tasks()                           // Browse task templates
get_node_for_task(task)               // Pre-configured nodes
```

### Performance Tips
- **FastAPI**: Use specific queries and filters for faster results
- **n8n MCP**: Use `get_node_essentials` instead of `get_node_info` for speed
- **Integration**: Always validate with MCP after finding patterns in FastAPI

## Common Usage Examples

### Example 1: Building an AI Chatbot
```bash
# 1. Discovery - Find AI chatbot examples
curl "http://localhost:8000/api/workflows?query=chatbot+OpenAI&limit=5"

# 2. Configuration - Understand OpenAI node
mcp__n8n-mcp__get_node_for_task("chat_with_ai")
mcp__n8n-mcp__get_node_essentials("nodes-base.openAi")

# 3. Validation - Check configuration
mcp__n8n-mcp__validate_node_operation("nodes-base.openAi", yourConfig)
```

### Example 2: Webhook Integration
```bash
# 1. Discovery - Find webhook patterns
curl "http://localhost:8000/api/workflows?query=webhook&trigger=Webhook&limit=10"

# 2. Configuration - Setup webhook node
mcp__n8n-mcp__get_node_for_task("receive_webhook")
mcp__n8n-mcp__get_property_dependencies("nodes-base.webhook")

# 3. Validation - Verify webhook setup
mcp__n8n-mcp__validate_workflow_connections(workflowJson)
```

### Example 3: Database Operations
```bash
# 1. Discovery - Find database workflows
curl "http://localhost:8000/api/workflows/category/Data Processing & Analysis"

# 2. Configuration - Setup database node
mcp__n8n-mcp__get_node_for_task("query_postgres")
mcp__n8n-mcp__list_nodes({category: "database"})

# 3. Validation - Check database config
mcp__n8n-mcp__validate_node_minimal("nodes-base.postgres", config)
```

## Key Statistics

### FastAPI Database
- **2,056 workflows** analyzed
- **29,522 nodes** indexed
- **365 unique integrations**
- **16 categories** available

### n8n MCP
- **535 nodes** available
- **269 AI tools** ready
- **108 trigger nodes**
- **88% documentation** coverage

Use both tools together for comprehensive n8n workflow development and analysis.