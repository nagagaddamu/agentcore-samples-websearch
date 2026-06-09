# Amazon Bedrock AgentCore Tools

## Overview
Amazon Bedrock AgentCore Tools provide enterprise-grade capabilities that enhance AI agents' ability to perform complex 
tasks securely and efficiently. This suite includes three tools: 

- Amazon Bedrock AgentCore Code Interpreter
- Amazon Bedrock AgentCore Browser Tool
- Amazon Bedrock AgentCore Web Search Tool

## Amazon Bedrock AgentCore Code Interpreter

### Key Features

1. **Secure Code Execution**: Execute code in isolated sandbox environments, ensuring security while accessing internal data sources.

2. **Fully Managed AWS-native Solution**: Seamlessly integrates with frameworks like Strands Agents, LangGraph, and CrewAI.

3. **Advanced Configuration Support**: Includes large file support for both input and output, and internet access.

4. **Multiple Language Support**: Pre-built runtime modes for various programming languages including JavaScript, TypeScript, and Python.

### Benefits

- **Enhanced Agent Accuracy**: Enables agents to perform complex calculations and data processing.
- **Enterprise-Grade Security**: Meets stringent security requirements with isolated environments.
- **Efficient Data Processing**: Capable of handling gigabyte-scale data by referencing files in Amazon S3.

## Amazon Bedrock AgentCore Browser Tool

### Key Features

1. **Model Agnostic Flexibility**: Supports various command syntaxes from different AI models, including Anthropic's Claude, OpenAI's models, and Amazon's Nova models.

2. **Enterprise-Grade Security**: Provides VM-level isolation, VPC connectivity, and integration with enterprise SSO systems.

3. **Comprehensive Audit Capabilities**: Includes CloudTrail logging of all browser commands and session recording features.

### Benefits

- **End-to-End Automation**: Enables AI agents to automate complex web workflows previously requiring manual intervention.
- **Enhanced Security**: Meets enterprise requirements with extensive security features and audit capabilities.
- **Real-Time Monitoring**: Offers Live View for immediate intervention and Session Replay for debugging and auditing.

## Use Cases

- Complex data analysis and visualization in secure environments
- Automated web interactions for form filling, data extraction, and multi-step processes
- Large-scale data processing and monitoring
- Secure code execution for AI agents in enterprise settings

## Amazon Bedrock AgentCore Web Search Tool

### Key Features

1. **Real-time Information Access**: Retrieve current web results with titles, URLs, snippets, and publication dates — no frozen training data.

2. **Zero Infrastructure Management**: No search APIs to provision, no scaling to configure. Expose web search through AgentCore Gateway as a fully managed MCP connector.

3. **Framework Agnostic**: Works with Strands Agents, LangChain, LangGraph, CrewAI, or any MCP-compatible client.

### Benefits

- **Grounded Responses**: Agents cite live sources rather than hallucinating outdated facts.
- **Critical for Time-Sensitive Use Cases**: CVE scanning, earnings research, regulatory monitoring — all require data that doesn't exist in training sets.
- **MCP-Native Discovery**: Agents discover and invoke the tool via standard `tools/list` and `tools/call` — no custom integration code.

## Use Cases

- Complex data analysis and visualization in secure environments
- Automated web interactions for form filling, data extraction, and multi-step processes
- Large-scale data processing and monitoring
- Secure code execution for AI agents in enterprise settings
- Real-time web grounding for agents that need current information

## Tutorials Overview

1. [Amazon Bedrock AgentCore Code Interpreter](01-Agent-Core-code-interpreter)
2. [Amazon Bedrock AgentCore Browser Tool](02-Agent-Core-browser-tool)
3. [Amazon Bedrock AgentCore Web Search Tool](03-Agent-Core-web-search)
