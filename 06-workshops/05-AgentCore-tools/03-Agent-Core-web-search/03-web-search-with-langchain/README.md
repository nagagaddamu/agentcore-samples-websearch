# Web Search Tool with LangChain Agent

## Overview

In this tutorial you will learn how to:

1. Connect a LangChain agent to an AgentCore Gateway using `langchain-mcp-adapters`
2. Discover Web Search tools via the MCP protocol
3. Build a ReAct agent with `ChatBedrockConverse` and LangGraph
4. Run web-grounded Q&A with the same Gateway set up in tutorial 01

### Tutorial Details

| Information | Details |
|:------------|:--------|
| Tutorial type | Interactive (Jupyter Notebook) |
| AgentCore components | AgentCore Gateway |
| Agentic framework | LangChain + LangGraph |
| LLM model | Anthropic Claude Sonnet 4 (`us.anthropic.claude-sonnet-4-20250514-v1:0`) |
| Tutorial vertical | Cross-vertical |
| Example complexity | Easy |
| SDK used | boto3, langchain-aws, langchain-mcp-adapters |

### Tutorial Architecture

```
┌────────────┐   question   ┌─────────────────────────────────────────┐
│   User     │ ───────────▶ │  LangGraph ReAct Agent                  │
│            │◀─────────── │  ChatBedrockConverse (Claude Sonnet 4)   │
│            │   answer     │                                         │
└────────────┘              │  MultiServerMCPClient                   │
                            │       │ streamable_http                 │
                            └───────┼─────────────────────────────────┘
                                    ▼
                            ┌──────────────────────┐
                            │  AgentCore Gateway   │
                            │  Web Search          │
                            └──────────────────────┘
```

## Notebook

→ [`03-web-search-langchain-agent.ipynb`](03-web-search-langchain-agent.ipynb)

## Prerequisites

Complete tutorial `01-gateway-setup-and-raw-mcp` first and export the environment variables it produces.

```bash
pip install -r requirements.txt
```
