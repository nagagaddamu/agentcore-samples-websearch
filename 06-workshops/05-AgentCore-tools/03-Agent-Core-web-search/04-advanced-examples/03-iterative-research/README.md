# Iterative Research Agent — Search → Reflect → Search Loop

## Overview

In this example you will build a research agent that answers complex questions by planning its research, executing searches, reflecting on what it still doesn't know, and issuing follow-up queries — repeating until it can answer with confidence.

This is a technique demonstration. The CVE Scanner and Earnings Brief examples both use this underlying pattern; this notebook makes it explicit and reusable.

### Tutorial Details

| Information | Details |
|:------------|:--------|
| Tutorial type | Interactive (Jupyter Notebook) |
| AgentCore components | AgentCore Gateway |
| Agentic framework | Strands Agents |
| LLM model | Anthropic Claude Sonnet 4 |
| Tutorial vertical | Cross-vertical |
| Example complexity | Advanced |
| SDK used | boto3, strands-agents |

### The Research Loop

```
Question
   │
   ▼
┌──────────────────────────────────────────┐
│  Plan: What do I need to know?           │
│  → Break question into sub-questions     │
└──────────────┬───────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────┐
│  Search: Execute highest-priority query  │◀──┐
└──────────────┬───────────────────────────┘   │
               │                               │
               ▼                               │
┌──────────────────────────────────────────┐   │
│  Reflect: What gaps remain?              │   │
│  → If gaps exist: refine query, repeat ──┼───┘
│  → If confident: synthesize             │
└──────────────┬───────────────────────────┘
               │
               ▼
         Final answer with citations
```

### Why This Pattern Matters

Single-shot search works for simple factual queries. For questions that require:
- Comparing multiple sources
- Reconciling conflicting information
- Drilling into details from a high-level result

...you need the reflect-and-refine loop. This notebook teaches that loop explicitly with configurable max iterations and a confidence threshold.

### Example Questions

- "What are the key differences between Claude 4 and GPT-4.5 for code generation?"
- "What regulatory changes in 2026 affect EU-based AI startups?"
- "What happened to Silicon Valley Bank and what changed in US banking regulation after?"

## Notebook

→ [`03-iterative-research.ipynb`](03-iterative-research.ipynb)

## Prerequisites

Complete `01-gateway-setup-and-raw-mcp` and export the environment variables.

```bash
pip install -r requirements.txt
```
