# OpenAPI Navigator

An MCP (Model Context Protocol) server that provides tools for navigating and querying OpenAPI specifications. This server makes it easy for AI agents to explore, search, and understand OpenAPI specs without having to manually parse complex JSON/YAML files.

## Features

- **Load OpenAPI specs** from local files or URLs
- **Navigate endpoints** with filtering by tags
- **Search endpoints** using fuzzy matching across paths, summaries, and operation IDs
- **Explore schemas** and their definitions
- **Multiple spec support** - load and manage multiple OpenAPI specifications simultaneously
- **Smart indexing** for fast lookups and searches
- **Reference preservation** - maintains `$ref` structures for agents to decide when to resolve

## Installation

This project uses `uv` for dependency management. Install dependencies with:

```bash
uvx sync
```

## Usage

### Running the Server

The server can be run in several ways:

#### 1. Using the package command (recommended)
```bash
uvx openapi-navigator
```

#### 2. Using FastMCP CLI (local development)
```bash
uv run fastmcp run src/openapi_mcp/server.py --transport stdio
```

#### 3. Using FastMCP CLI with HTTP transport (local development)
```bash
uv run fastmcp run src/openapi_mcp/server.py --transport http --host 127.0.0.1 --port 8000
```

## Available Tools

The OpenAPI Navigator provides the following tools:

### Core Operations
- **`load_spec`** - Load an OpenAPI specification from a local file (requires absolute path)
- **`load_spec_from_url`** - Load an OpenAPI specification from a URL
- **`list_loaded_specs`** - List all currently loaded specifications
- **`unload_spec`** - Remove a specification from memory

### Endpoint Operations
- **`search_endpoints`** - Search endpoints using fuzzy matching. Use `""` or `"a"` as the query to get all endpoints
- **`get_endpoint`** - Get detailed information for a specific endpoint by path and method

### Schema Operations
- **`search_schemas`** - Search schema names using fuzzy matching. Use `""` or `"a"` as the query to get all schemas
- **`get_schema`** - Get detailed information for a specific schema by name

## Example Workflow

1. **Load a specification:**
   ```
   load_spec("/absolute/path/to/schema.yaml", "my-api")
   ```

2. **Get all endpoints:**
   ```
   search_endpoints("my-api", "")
   ```

3. **Get all schemas:**
   ```
   search_schemas("my-api", "")
   ```

4. **Search for specific endpoints:**
   ```
   search_endpoints("my-api", "virtual machine")
   ```

5. **Get endpoint details:**
   ```
   get_endpoint("my-api", "/api/virtualization/virtual-machines/", "GET")
   ```

6. **Get schema details:**
   ```
   get_schema("my-api", "VirtualMachine")
   ```

## Development

### Testing

Run the integration tests:
```bash
uv run python test_integration.py
```

### Inspecting the Server

Use FastMCP CLI to inspect the server:
```bash
uv run fastmcp inspect src/openapi_mcp/server.py
```

This will generate a `server-info.json` file with detailed information about all available tools.

## Architecture

The OpenAPI Navigator consists of two main components:

1. **SpecManager**: Handles loading, validation, and indexing of OpenAPI specifications
2. **MCP Server**: Exposes tools through the Model Context Protocol

### SpecManager Features

- **Multi-format support**: Handles both YAML and JSON OpenAPI specs
- **Version detection**: Automatically detects OpenAPI 3.x vs Swagger 2.x
- **Smart indexing**: Builds indexes for fast endpoint and schema lookups
- **Fuzzy search**: Provides intelligent search across endpoint metadata
- **Reference handling**: Preserves `$ref` structures without automatic resolution

### Error Handling

- **Validation warnings**: Warns on validation issues but continues if possible
- **Graceful degradation**: Only rejects specs that prevent core functionality
- **Helpful error messages**: Provides clear feedback on what went wrong

## Security Considerations

- **Absolute paths only**: Local file loading requires absolute paths for security
- **No automatic execution**: The server only reads and parses specs, never executes code
- **Input validation**: All inputs are validated before processing

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.