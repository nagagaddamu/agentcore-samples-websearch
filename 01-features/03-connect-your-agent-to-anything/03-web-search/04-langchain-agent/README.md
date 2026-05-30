# Web Search with a LangChain Agent

## Overview

This demo shows the same Web Search Tool integration using LangChain and LangGraph instead of Strands. It uses `langchain-mcp-adapters` to connect to the AgentCore Gateway and `create_react_agent` from LangGraph for the agent loop.

```
┌────────────┐  "Latest AWS announcements?"   ┌──────────────────────────┐
│   User     │ ─────────────────────────────▶ │   LangChain Agent        │
│            │◀───────────────────────────── │   (ChatBedrockConverse)  │
│            │  "Here's what I found: [...]"  │                          │
└────────────┘                                 │  tools from MCP client   │
                                               │       │                  │
                                               └───────┼──────────────────┘
                                                       │ MultiServerMCPClient
                                                       ▼
                                               ┌──────────────────────────┐
                                               │  AgentCore Gateway       │
                                               │  → Web Search Connector  │
                                               └──────────────────────────┘
```

## How It Works

### MCP Client Setup

`langchain-mcp-adapters` provides `MultiServerMCPClient` which connects to one or more MCP servers and converts their tools into LangChain-compatible tool objects:

```python
from langchain_mcp_adapters.client import MultiServerMCPClient

async with MultiServerMCPClient({
    "web-search": {
        "transport": "streamable_http",
        "url": gateway_url,
        "headers": {"Authorization": f"Bearer {token}"},
    }
}) as client:
    tools = client.get_tools()
    # tools is a list of LangChain Tool objects
```

### Agent Creation

The agent uses LangGraph's `create_react_agent` with `ChatBedrockConverse` as the LLM:

```python
from langchain_aws import ChatBedrockConverse
from langgraph.prebuilt import create_react_agent

model = ChatBedrockConverse(
    model="us.anthropic.claude-sonnet-4-20250514-v1:0",
    region_name="us-east-1",
)
agent = create_react_agent(model, tools=tools)
result = await agent.ainvoke({"messages": [{"role": "user", "content": query}]})
```

### Async Execution

LangChain's MCP adapter uses async I/O. The demo wraps the agent call in `asyncio.run()` for CLI usage.

## Prerequisites

```bash
pip install -r ../requirements.txt
```

Run `01-setup-gateway/setup_gateway.py` first and export the environment variables it prints.

Requires access to Claude Sonnet 4 (`us.anthropic.claude-sonnet-4-20250514-v1:0`) in us-east-1.

## Usage

```bash
# Default query
python web_search_langchain.py

# Custom query
python web_search_langchain.py --query "Latest AWS announcements"
python web_search_langchain.py --query "Python 3.13 new features"
```

## IAM Permissions

```json
{
  "Effect": "Allow",
  "Action": "bedrock:InvokeModel",
  "Resource": "arn:aws:bedrock:us-east-1::foundation-model/us.anthropic.claude-sonnet-4-20250514-v1:0"
}
```

## Files

| File | Description |
|:-----|:------------|
| `web_search_langchain.py` | Main demo script — LangChain agent with web search |
| `../utils/gateway_auth.py` | OAuth token retrieval (shared with other demos) |
