# Earnings Brief Agent — Chained Multi-turn Research

## Overview

In this example you will build a financial research agent that, given a company ticker, automatically searches for analyst estimates, recent competitor results, and sector news — then generates a structured pre-earnings call brief.

**Why live search is critical here**: Analyst consensus, guidance revisions, and competitor earnings are filed days before the call. This information does not exist in any model's training data.

### Tutorial Details

| Information | Details |
|:------------|:--------|
| Tutorial type | Interactive (Jupyter Notebook) |
| AgentCore components | AgentCore Gateway |
| Agentic framework | Strands Agents |
| LLM model | Anthropic Claude Sonnet 4 |
| Tutorial vertical | Finance |
| Example complexity | Intermediate |
| SDK used | boto3, strands-agents |

### What the Agent Does

```
Input: Ticker symbol (e.g., "AMZN") + earnings date
         │
         ▼
┌─────────────────────────────────────────────┐
│  Search 1: Analyst EPS and revenue estimates │
│  Search 2: Guidance revisions since last Q   │
│  Search 3: Top 2 competitors' recent results │
│  Search 4: Macro / sector news this quarter  │
└─────────────────────────────────────────────┘
         │
         ▼
Output: Structured brief
  ## Pre-Earnings Brief: AMZN — Q2 2026
  ### Analyst Consensus
  ### Key Risks and Tailwinds
  ### Competitor Context
  ### What to Watch
```

### Chained Search Pattern

Each search is informed by the results of the previous one:
1. Analyst consensus → identifies what the market expects
2. Guidance revision search → narrows to surprises vs. prior guidance
3. Competitor search → adds comparative context
4. Macro/sector search → explains external factors the model might miss

The agent reflects after each search and decides what to look for next — this is the chained multi-turn pattern.

## Notebook

→ [`02-earnings-brief.ipynb`](02-earnings-brief.ipynb)

## Prerequisites

Complete `01-gateway-setup-and-raw-mcp` and export the environment variables.

```bash
pip install -r requirements.txt
```
