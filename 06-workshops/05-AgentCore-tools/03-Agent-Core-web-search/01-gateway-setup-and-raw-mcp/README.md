# Web Search Tool — Gateway Setup and Raw MCP Calls

## Overview

In this tutorial you will learn how to:

1. Create the IAM service role for the Gateway
2. Configure Amazon Cognito for inbound authentication
3. Create an AgentCore Gateway with MCP protocol
4. Add a Web Search Tool connector target
5. Verify the setup with direct MCP `tools/list` and `tools/call` invocations

### Tutorial Details

| Information | Details |
|:------------|:--------|
| Tutorial type | Interactive (Jupyter Notebook) |
| AgentCore components | AgentCore Gateway |
| Gateway target type | Connector (`web-search`) |
| Tutorial vertical | Cross-vertical |
| Example complexity | Easy |
| SDK used | boto3 |

### Tutorial Architecture

<div style="text-align:left">
    <img src="images/tutorial-architecture.png" width="80%"/>
</div>

### Inbound and Outbound Authentication

<div style="text-align:left">
    <img src="images/inbound-and-outbound-auth.png" width="80%"/>
</div>

The Gateway uses two authentication layers:
- **Inbound**: Amazon Cognito validates the OAuth token passed by your agent
- **Outbound**: The Gateway's IAM role authenticates automatically to the Web Search backend — no additional credentials needed

## Notebook

→ [`01-web-search-gateway-setup.ipynb`](01-web-search-gateway-setup.ipynb)

## Prerequisites

- AWS account allowlisted for the Web Search Tool private beta
- Python 3.10+ and Jupyter
- AWS credentials with IAM, Cognito, and AgentCore Gateway permissions

```bash
pip install -r requirements.txt
```

> **Region**: The Web Search Tool connector is currently available in **us-east-1** only.

## What You'll Build

After completing this tutorial you'll have:
- An AgentCore Gateway with MCP protocol
- A Web Search connector target in READY state
- Environment variables ready for tutorials 02 and 03
