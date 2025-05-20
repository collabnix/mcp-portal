---
title: "MCP Examples"
date: 2025-05-20T10:00:00-07:00
draft: false
---

# MCP Example Projects

This page showcases examples of MCP servers and implementations to help you understand how to use the Model Context Protocol in real-world scenarios.

## Basic Examples

### Hello World MCP Server

The simplest possible MCP server that responds with a greeting.

```python
from mcp import BaseModel, Field, JSONSchema, Response, Server, Tool, router
from typing import Annotated

class GreetingRequest(BaseModel):
    name: Annotated[
        str,
        Field(description="The name of the person to greet"),
    ]

@router.tool(
    "greeting",
    "Says hello to the specified person",
    request_schema=JSONSchema.from_type(GreetingRequest),
)
async def greeting(request: GreetingRequest) -> Response:
    return Response({"greeting": f"Hello, {request.name}!"})

if __name__ == "__main__":
    server = Server(tools=[greeting])
    server.run(host="127.0.0.1", port=8000)
```

[View full code on GitHub](https://github.com/modelcontextprotocol/examples/hello-world)

### Calculator Tool

A simple calculator tool that performs basic arithmetic operations.

```python
from mcp import BaseModel, Field, JSONSchema, Response, Server, Tool, router
from typing import Annotated, Literal

class CalculationRequest(BaseModel):
    operation: Annotated[
        Literal["add", "subtract", "multiply", "divide"],
        Field(description="The arithmetic operation to perform"),
    ]
    x: Annotated[
        float,
        Field(description="The first number"),
    ]
    y: Annotated[
        float,
        Field(description="The second number"),
    ]

@router.tool(
    "calculator",
    "Performs basic arithmetic operations",
    request_schema=JSONSchema.from_type(CalculationRequest),
)
async def calculator(request: CalculationRequest) -> Response:
    operation = request.operation
    x, y = request.x, request.y
    
    result = None
    if operation == "add":
        result = x + y
    elif operation == "subtract":
        result = x - y
    elif operation == "multiply":
        result = x * y
    elif operation == "divide":
        if y == 0:
            return Response({"error": "Cannot divide by zero"}, status_code=400)
        result = x / y
    
    return Response({"result": result})

if __name__ == "__main__":
    server = Server(tools=[calculator])
    server.run(host="127.0.0.1", port=8000)
```

[View full code on GitHub](https://github.com/modelcontextprotocol/examples/calculator)

## Intermediate Examples

### Weather Service

An MCP server that connects to a weather API to provide weather forecasts.

**Key features:**
- API integration with OpenWeatherMap
- Error handling for API failures
- Support for different units (metric/imperial)

[View tutorial](/blog/building-first-mcp-server)

### File System Explorer

Enables AI assistants to explore and interact with the local file system.

**Key features:**
- List files and directories
- Read file contents
- Create and modify files
- Path validation and security measures

[View on GitHub](https://github.com/modelcontextprotocol/reference-implementations/filesystem)

## Advanced Examples

### GitHub Integration

Connect your AI assistant to GitHub for code review, issue management, and more.

**Key features:**
- Repository browsing
- Pull request review
- Issue management
- Code search

[View on GitHub](https://github.com/modelcontextprotocol/reference-implementations/github)

### PostgreSQL Database Tool

Enable AI assistants to query and analyze data from PostgreSQL databases.

**Key features:**
- Query execution
- Schema exploration
- Result formatting
- Query validation

[View on GitHub](https://github.com/modelcontextprotocol/reference-implementations/postgres)

### Slack Integration

Allow AI assistants to search and interact with Slack workspaces.

**Key features:**
- Channel listing
- Message history
- Search functionality
- Messaging capabilities

[View on GitHub](https://github.com/modelcontextprotocol/reference-implementations/slack)

## Community Examples

### PDF Analyzer

An MCP server that analyzes and extracts information from PDF documents.

**Created by:** @community-member

[View on GitHub](https://github.com/username/pdf-analyzer-mcp)

### MCP-powered ChatOps

Integrate AI assistants into your DevOps workflow with MCP.

**Created by:** @devops-team

[View on GitHub](https://github.com/username/mcp-chatops)

## Contributing Your Examples

Have you created an MCP server or tool? Share it with the community!

1. Fork the [examples repository](https://github.com/modelcontextprotocol/examples)
2. Add your example with clear documentation
3. Submit a pull request
4. After review, your example may be featured on this page

## Resources

- [Official MCP Examples](https://github.com/modelcontextprotocol/examples)
- [MCP Reference Implementations](https://github.com/modelcontextprotocol/reference-implementations)
- [MCP Python SDK Documentation](https://docs.mcp.anthropic.com/python)
