# Context Portal MCP Server Report

## Overview
Context Portal MCP (ConPort) is a database-backed server implementing the Model Context Protocol (MCP). It acts as a project "memory bank", enabling AI assistants and developer tools to store and retrieve structured context about a software project.

The server is built with **FastAPI** and **FastMCP**, exposing a collection of MCP tools for managing product context, active context, decisions, progress, system patterns, and custom data. Each workspace uses its own SQLite database, enabling isolated storage per project.

## Architecture
- **Entry Point:** `src/context_portal_mcp/main.py` initializes a FastAPI application, mounts the FastMCP server, and defines CLI options for HTTP or STDIO modes.
- **Database Layer:** Functions in `src/context_portal_mcp/db/database.py` manage SQLite connections, migrations, and CRUD operations for all context entities.
- **Handlers:** Business logic resides in `src/context_portal_mcp/handlers/mcp_handlers.py`, which orchestrates validation and database interaction.
- **Core Components:** Custom exceptions and configuration utilities live under `src/context_portal_mcp/core`.

## Key Features
- Per-workspace SQLite databases with automatic provisioning and Alembic migrations.
- Rich MCP toolset for CRUD operations on project context elements.
- Semantic search support via vector embeddings and ChromaDB integration.
- Logging and CLI options for HTTP or STDIO operation modes.

## MCP Tools
The server exposes numerous MCP tools, including but not limited to:
- `get_product_context` and `update_product_context`
- `get_active_context` and `update_active_context`
- `log_decision`, `get_decisions`, and `search_decisions_fts`
- `log_progress`, `get_progress`, and `update_progress`
- `log_system_pattern` and `get_system_patterns`
- `log_custom_data` and `get_custom_data`
- `link_conport_items` and `get_linked_items`
- `get_item_history` and `get_recent_activity_summary`

These tools allow AI assistants to build and query a project knowledge graph, enabling retrieval-augmented generation workflows.

## Operation Modes
- **HTTP Mode:** Runs a FastAPI server with MCP endpoints available over HTTP.
- **STDIO Mode:** Provides MCP access over standard I/O for tight IDE integration.

## Data Flow
1. Client invokes an MCP tool with a `workspace_id`.
2. Handlers validate input via Pydantic models in `db/models.py`.
3. Database functions execute CRUD operations on the workspace's SQLite database.
4. Results return to the client, potentially augmented with vector search results when semantic retrieval is requested.

## Conclusion
Context Portal MCP offers a robust backend for storing and retrieving structured project context. Its modular design, rich toolset, and per-workspace isolation make it suitable as a memory bank for AI-assisted development environments.

