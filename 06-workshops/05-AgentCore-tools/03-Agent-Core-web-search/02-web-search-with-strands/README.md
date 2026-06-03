# Web Search Tool with Strands Agent

## Overview

In this tutorial you will learn how to:

1. Connect a Strands agent to an AgentCore Gateway via MCP Streamable HTTP
2. Discover the Web Search tool dynamically at runtime
3. Ask real-time questions — the agent invokes WebSearch automatically
4. Receive grounded responses with cited source URLs

### Tutorial Details

| Information | Details |
|:------------|:--------|
| Tutorial type | Interactive (Jupyter Notebook) |
| AgentCore components | AgentCore Gateway |
| Agentic framework | Strands Agents |
| LLM model | Anthropic Claude Sonnet 4 (`us.anthropic.claude-sonnet-4-20250514-v1:0`) |
| Tutorial vertical | Cross-vertical |
| Example complexity | Easy |
| SDK used | boto3, strands-agents |

### Tutorial Architecture

```
┌────────────┐  "What's the latest AI news?"  ┌──────────────────────────┐
│   User     │ ─────────────────────────────▶ │   Strands Agent          │
│            │◀───────────────────────────── │   Claude Sonnet 4        │
│            │  Grounded answer + citations   │                          │
└────────────┘                                 │  tools: [WebSearch]      │
                                               │       │ MCP tools/call   │
                                               └───────┼──────────────────┘
                                                       ▼
                                               ┌──────────────────────────┐
                                               │  AgentCore Gateway       │
                                               │  Web Search Connector    │
                                               └──────────────────────────┘
```

## Notebook

→ [`02-web-search-strands-agent.ipynb`](02-web-search-strands-agent.ipynb)

## Prerequisites

Complete tutorial `01-gateway-setup-and-raw-mcp` first and export the environment variables it produces.

```bash
pip install -r requirements.txt
```
