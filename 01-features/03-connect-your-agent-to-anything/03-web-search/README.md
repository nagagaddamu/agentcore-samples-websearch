# AgentCore Web Search Tool

## Overview

[Amazon Bedrock AgentCore Web Search Tool](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway.html) exposes web search as a fully managed, MCP-compliant tool through Amazon Bedrock AgentCore Gateway. Your agents discover and invoke it using the standard Model Context Protocol — no custom integrations, no infrastructure to manage.

> 🔒 **Search Privacy**: The Web Search Tool queries an AWS-maintained search index. Queries do not route to any third-party search engines or external providers.

![Web Search Tool Architecture](images/tutorial-architecture.png)

## How the Web Search Tool Works

### Gateway + Connector Architecture

The Web Search Tool uses the **connector** target type — a fully AWS-managed integration that requires no schema, no endpoint configuration, and no outbound credential setup. You specify `connectorId: "web-search"` and the Gateway handles everything else.

![Web search with Amazon Bedrock AgentCore Gateway](images/agentcore-web-search-architecture.png)

### Key Capabilities

- **Real-time information access** — Retrieve current web results with titles, URLs, snippets, and publication dates
- **Zero infrastructure management** — No search APIs to provision or scaling to configure
- **Framework agnostic** — Works with Strands Agents, LangChain, LangGraph, CrewAI, or any MCP-compatible client
- **Structured results** — Results returned in both MCP `content` (text) and `structuredContent` (typed JSON) formats

### How It Works

1. **Gateway setup** — Create an AgentCore Gateway and add a Web Search Tool target using `connectorId: "web-search"`
2. **Tool discovery** — Your agent calls `tools/list` on the Gateway endpoint and discovers `WebSearch` with its input schema
3. **Search invocation** — Your agent calls `tools/call` with a natural language query (up to 200 characters)
4. **Structured results** — The tool returns results with text snippets, URLs, titles, and publication dates
5. **Grounded response** — Your agent uses the results to compose a response with cited sources

### Response Format

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

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `text` | string | Yes | Text content or snippet of the search result |
| `url` | string | No | URL of the source webpage |
| `title` | string | No | Title of the source webpage |
| `publishedDate` | string | No | Publication date of the result |

> **Note:** Queries longer than 200 characters may not return results. Keep queries concise.

### Authentication

- **Inbound**: Amazon Cognito with `client_credentials` OAuth flow (can use other OAuth providers)
- **Outbound**: Automatic — the Gateway uses its own IAM role to authenticate to the Web Search backend

## Demos

| Folder | Framework | What You'll Learn |
|:-------|:----------|:------------------|
| [01-setup-gateway/](01-setup-gateway/) | boto3 (SDK) | Create IAM role, Cognito, Gateway, and Web Search target |
| [02-raw-mcp/](02-raw-mcp/) | MCP protocol | Direct tool discovery and invocation without an agent |
| [03-strands-agent/](03-strands-agent/) | Strands Agents | Full agent loop with automatic tool selection and cited responses |
| [04-langchain-agent/](04-langchain-agent/) | LangChain + LangGraph |  Full agent loop integration using LangChain's MCP adapter |
| [05-cleanup/](05-cleanup/) | boto3 (SDK) | Delete all resources created by the setup |

## Prerequisites

Before running the agent demos (steps 3–4 in Quick Start), your AWS account must have model access enabled for the Bedrock model you intend to use.

**Default model**: `us.anthropic.claude-sonnet-4-5-20250514-v1:0` (cross-region inference profile)

To use a different model, export `BEDROCK_MODEL_ID` before running any agent demo:

```bash
export BEDROCK_MODEL_ID="us.anthropic.claude-3-5-sonnet-20241022-v2:0"
```

> **Note:** On-demand model IDs (e.g. `anthropic.claude-sonnet-4-5-20250929-v1:0`) are not supported directly — you must use a cross-region inference profile ID (prefixed with `us.`, `eu.`, or `ap.`) or an inference profile ARN. See [Supported models and Regions for cross-Region inference](https://docs.aws.amazon.com/bedrock/latest/userguide/inference-profiles-support.html) for the full list.

To enable model access in your account:
1. Open the [Amazon Bedrock console](https://console.aws.amazon.com/bedrock/)
2. Go to **Model access** in the left navigation
3. Request access for the model you want to use

## Quick Start

```bash
pip install -r requirements.txt

# Step 1: Create Gateway and Web Search target
python 01-setup-gateway/setup_gateway.py

# Step 2: Load the credentials written by setup
source .env.web-search

# Optional: override the default Bedrock model
export BEDROCK_MODEL_ID="us.anthropic.claude-3-5-sonnet-20241022-v2:0"

# Step 3: Verify with raw MCP calls
python 02-raw-mcp/raw_mcp_call.py

# Step 4: Run with Strands agent
python 03-strands-agent/web_search_strands.py

# Step 5: Run with LangChain agent
python 04-langchain-agent/web_search_langchain.py

# Cleanup when done
python 05-cleanup/cleanup.py --gateway-id <id> --user-pool-id <id> --role-name <name>
```

## Shared Utilities

Demos 02–04 share utilities in `utils/`:

```python
from utils.gateway_auth import get_oauth_token, create_streamable_http_transport
from utils.web_search_agent import create_agent, create_mcp_client
```

- `gateway_auth.py` — OAuth token retrieval from Cognito and MCP transport factory
- `web_search_agent.py` — Strands agent factory with Web Search tools

## IAM Permissions

### Caller (setup script)

```json
{
  "Effect": "Allow",
  "Action": [
    "iam:CreateRole",
    "iam:PutRolePolicy",
    "iam:GetRole",
    "cognito-idp:CreateUserPool",
    "cognito-idp:CreateUserPoolDomain",
    "cognito-idp:CreateResourceServer",
    "cognito-idp:CreateUserPoolClient",
    "bedrock-agentcore:CreateGateway",
    "bedrock-agentcore:GetGateway",
    "bedrock-agentcore:CreateGatewayTarget",
    "bedrock-agentcore:ListGatewayTargets"
  ],
  "Resource": "*"
}
```

### Caller (agent demos)

```json
{
  "Effect": "Allow",
  "Action": "bedrock:InvokeModel",
  "Resource": "*"
}
```

### Gateway Service Role (created by setup)

```json
{
  "Effect": "Allow",
  "Action": [
    "bedrock-agentcore:InvokeGateway",
    "bedrock-agentcore:InvokeWebSearch"
  ],
  "Resource": [
    "arn:aws:bedrock-agentcore:us-east-1:<account>:gateway/*",
    "arn:aws:bedrock-agentcore:us-east-1:aws:tool/web-search.v1"
  ]
}
```

## Region Availability

The Web Search Tool connector is currently available in **us-east-1** only.

## Files

| File | Description |
|:-----|:------------|
| `requirements.txt` | Python dependencies for all sub-demos |
| `utils/gateway_auth.py` | Shared OAuth and MCP transport utilities |
| `utils/web_search_agent.py` | Shared Strands agent factory |
| `01-setup-gateway/` | Gateway and target creation script |
| `02-raw-mcp/` | Direct MCP protocol demo |
| `03-strands-agent/` | Strands agent demo |
| `04-langchain-agent/` | LangChain agent demo |
| `05-cleanup/` | Resource deletion script |
