# Web Search with a Strands AI Agent

## Overview

This demo shows the complete agent integration: a Strands agent automatically discovers and invokes the Web Search Tool to answer real-time questions with cited sources.

> 🔒 **Search Privacy**: The Web Search Tool queries an AWS-maintained index built from open-source and licensed content providers. No queries are routed to third-party search engines or external providers.

```
┌────────────┐  "What's the latest AI news?"  ┌──────────────────────────┐
│   User     │ ─────────────────────────────▶ │   Strands Agent          │
│            │◀───────────────────────────── │   (Claude Sonnet 4)      │
│            │  "Here's what I found: [...]"  │                          │
└────────────┘                                 │  tools: [WebSearch]      │
                                               │       │                  │
                                               └───────┼──────────────────┘
                                                       │ MCP tools/call
                                                       ▼
                                               ┌──────────────────────────┐
                                               │  AgentCore Gateway       │
                                               │  → Web Search Connector  │
                                               │  → Structured results    │
                                               └──────────────────────────┘
```

## How It Works

### Tool Discovery

The agent connects to the Gateway via MCP Streamable HTTP and calls `tools/list`. The Gateway returns the `WebSearch` tool with its input schema. Strands automatically registers it as an available tool for the LLM.

### Agent Loop

When you ask a question:

1. Strands sends the query + tool schema to Claude Sonnet 4
2. Claude decides to call `WebSearch` with a concise search query
3. Strands invokes the tool via MCP `tools/call` on the Gateway
4. The Gateway routes to the Web Search connector and returns results
5. Results are fed back to Claude as a tool result
6. Claude synthesizes a final answer with cited sources

### Shared Agent (`utils/web_search_agent.py`)

The agent factory handles MCP client setup and tool discovery:

```python
from utils.web_search_agent import create_agent, create_mcp_client

mcp_client = create_mcp_client()

with mcp_client:
    agent = create_agent(mcp_client)
    result = agent("What is the latest version of Python?")
    print(result.message)
```

The `create_mcp_client()` function handles OAuth token retrieval and transport creation. The `create_agent()` function discovers tools and configures the Strands agent with an appropriate system prompt.

## Prerequisites

```bash
pip install -r ../requirements.txt
```

Run `01-setup-gateway/setup_gateway.py` first, then load credentials: `source .env.web-search`

Requires access to Claude Sonnet 4 (`us.anthropic.claude-sonnet-4-6`) in us-east-1.

## Usage

```bash
# Run default demo queries
python web_search_strands.py

# Custom query
python web_search_strands.py --query "What are the latest AI announcements?"
python web_search_strands.py --query "Current price of Bitcoin"
```

## IAM Permissions

```json
{
  "Effect": "Allow",
  "Action": "bedrock:InvokeModel",
  "Resource": "arn:aws:bedrock:us-east-1::foundation-model/us.anthropic.claude-sonnet-4-6"
}
```

Gateway invocation is authorized via the Cognito OAuth token — no additional IAM permissions needed for the caller.

## Files

| File | Description |
|:-----|:------------|
| `web_search_strands.py` | Main demo script — Strands agent with web search |
| `../utils/web_search_agent.py` | Shared agent factory with MCP client setup |
| `../utils/gateway_auth.py` | OAuth token retrieval and transport creation |
