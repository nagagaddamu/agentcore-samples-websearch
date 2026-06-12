# Raw MCP Tool Discovery and Invocation

## Overview

This demo calls the AgentCore Gateway directly over the MCP protocol — no agent framework involved. It's the simplest way to verify your Gateway and Web Search Tool target infrastructure are working correctly.

> 🔒 **Search Privacy**: The Web Search Tool queries an AWS-maintained search index. Queries do not route to any third-party search engines or external providers.

```
┌──────────────────┐   tools/list    ┌─────────────────────────────────┐
│  raw_mcp_call.py │ ──────────────▶ │  AgentCore Gateway              │
│                  │                  │                                 │
│  MCPClient       │   tools/call    │  Target: web-search (connector) │
│  (Streamable     │ ──────────────▶ │                                 │
│   HTTP)          │◀────────────── │  → WebSearch results             │
└──────────────────┘   results       └─────────────────────────────────┘
```

## How It Works

### Tool Discovery (`tools/list`)

The MCP `tools/list` call returns all tools available on the Gateway. For a Gateway with only the Web Search connector target, you'll see one tool:

```json
{
  "name": "WebSearch",
  "description": "Search the web for current information",
  "inputSchema": {
    "type": "object",
    "properties": {
      "query": {
        "type": "string",
        "description": "Search query (max 200 characters)"
      }
    },
    "required": ["query"]
  }
}
```

### Tool Invocation (`tools/call`)

Calling the tool with a query returns structured results:

```json
{
  "results": [
    {
      "text": "Snippet from the web page...",
      "url": "https://example.com/article",
      "title": "Article Title",
      "publishedDate": "2026-05-28"
    }
  ]
}
```

### Authentication Flow

1. The script obtains an OAuth token from Cognito (`client_credentials` flow)
2. The token is passed as a `Bearer` header on the MCP Streamable HTTP transport
3. The Gateway validates the JWT and authorizes the request

## Prerequisites

```bash
pip install -r ../requirements.txt
```

Run `01-setup-gateway/setup_gateway.py` first, then load credentials: `source .env.web-search`

## Usage

```bash
# Default query
python raw_mcp_call.py

# Custom query
python raw_mcp_call.py --query "Latest Python release"
python raw_mcp_call.py --query "AWS re:Invent 2026 announcements"
```

## IAM Permissions

No direct IAM permissions needed — authentication is handled via the Cognito OAuth token. The Gateway's service role provides the `bedrock-agentcore:InvokeWebSearch` permission.

## Files

| File | Description |
|:-----|:------------|
| `raw_mcp_call.py` | Main demo script — tool discovery and invocation |
