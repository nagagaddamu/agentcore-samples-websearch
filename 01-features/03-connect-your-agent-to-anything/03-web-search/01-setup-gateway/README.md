# Set Up AgentCore Gateway with Web Search Tool

## Overview

This script creates all the infrastructure needed to expose the Web Search Tool through an AgentCore Gateway. After running it, you'll have a fully functional MCP endpoint that any agent framework can connect to.

```
┌──────────────────┐         ┌─────────────────────────────────────┐
│  setup_gateway   │────────▶│  AWS Resources Created:             │
│                  │         │                                     │
│  1. IAM Role     │         │  • IAM Role (InvokeWebSearch)       │
│  2. Cognito      │         │  • Cognito User Pool + M2M Client   │
│  3. Gateway      │         │  • AgentCore Gateway (MCP)          │
│  4. Target       │         │  • Web Search Connector Target      │
└──────────────────┘         └─────────────────────────────────────┘
```

## How It Works

### Gateway Service Role

The Gateway needs an IAM role that allows the AgentCore service to perform actions on your behalf. For the Web Search Tool, the role needs two permissions:

- `bedrock-agentcore:InvokeGateway` — to invoke the Gateway itself
- `bedrock-agentcore:InvokeWebSearch` — to authorize web search invocations against `arn:aws:bedrock-agentcore:<region>:aws:tool/web-search.v1`

### Cognito Authentication

The script creates a Cognito User Pool with:
- A resource server defining an `invoke` scope
- A machine-to-machine (M2M) client using the `client_credentials` OAuth flow

This provides JWT-based authentication for incoming requests to the Gateway.

### Gateway Creation

The Gateway is created with:
- **Protocol**: MCP (Model Context Protocol)
- **MCP version**: 2025-03-26
- **Search type**: SEMANTIC (enables tool discovery across multiple targets)
- **Authorizer**: Custom JWT backed by the Cognito User Pool

### Web Search Connector Target

The Web Search Tool uses the `connector` target type — a fully AWS-managed integration. You specify `connectorId: "web-search"` and the Gateway handles schema management, endpoint resolution, and service authentication automatically.

Outbound authentication uses `GATEWAY_IAM_ROLE` — the Gateway's own service role authenticates to the Web Search backend. No additional credentials needed.

## Prerequisites

```bash
pip install -r ../requirements.txt
```

Your AWS credentials must have permissions to create IAM roles, Cognito pools, and AgentCore Gateways. Your account must be allowlisted for the Web Search Tool private beta.

> **Region**: The Web Search Tool connector is currently available in **us-east-1** only.

## Usage

```bash
# Default setup
python setup_gateway.py

# Custom gateway name
python setup_gateway.py --gateway-name my-web-search-gw

# Explicit region
python setup_gateway.py --region us-east-1
```

After completion, the script prints environment variables to export:

```bash
export AGENTCORE_GATEWAY_URL="https://..."
export COGNITO_DOMAIN="..."
export COGNITO_CLIENT_ID="..."
export COGNITO_CLIENT_SECRET="..."
export COGNITO_SCOPE="agentcore-websearch/invoke"
export AWS_DEFAULT_REGION="us-east-1"
```

```bash
#export AGENTCORE_GATEWAY_URL="https://naga-web-search-gw-jpeyqm6nni.gateway.bedrock-agentcore.us-east-1.amazonaws.com/mcp"
#export COGNITO_DOMAIN="us-east-16ay8pbcw2"
#export COGNITO_CLIENT_ID="n0gmrlojshfmc059dck71583f"
#export COGNITO_CLIENT_SECRET="s9bt641phqs1t7bie4lvg80sah9st50f6c3ooeksf0ri6d7f5po"
#export COGNITO_SCOPE="agentcore-websearch/invoke"
#export AWS_DEFAULT_REGION="us-east-1"
```


Export these before running the other demos.

## IAM Permissions

### Caller (you)

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
    "cognito-idp:ListUserPools",
    "cognito-idp:ListUserPoolClients",
    "cognito-idp:DescribeUserPoolClient",
    "cognito-idp:DescribeResourceServer",
    "bedrock-agentcore:CreateGateway",
    "bedrock-agentcore:GetGateway",
    "bedrock-agentcore:CreateGatewayTarget",
    "bedrock-agentcore:ListGatewayTargets"
  ],
  "Resource": "*"
}
```

### Gateway Service Role (created by this script)

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

## Files

| File | Description |
|:-----|:------------|
| `setup_gateway.py` | Main setup script — creates all resources |
