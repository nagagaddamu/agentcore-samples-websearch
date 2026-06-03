# Advanced Web Search Examples

## Overview

These examples go beyond single-shot search to demonstrate patterns where live web data is genuinely critical — not optional. Each example shows a realistic use case where stale training data would break the outcome.

## Examples

| Folder | Use Case | Key Pattern |
|:-------|:---------|:------------|
| [`01-cve-scanner/`](01-cve-scanner/) | Security vulnerability scan across a dependency list | Multi-turn: one search per package, chained reasoning |
| [`02-earnings-brief/`](02-earnings-brief/) | Pre-earnings call research brief | Multi-turn: analyst estimates → competitors → sector news → synthesis |
| [`03-iterative-research/`](03-iterative-research/) | Deep Q&A via search → reflect → search loop | Technique demo: explicit multi-turn research pattern |

## When to Use Each Pattern

### CVE Scanner — "search for each item in a list"
The agent receives a list of dependencies, searches for CVEs for each one, and produces a prioritized remediation report. The loop is driven by the input list, and each search result is independent.

### Earnings Brief — "chained research where each search informs the next"
The agent starts with analyst consensus, finds gaps, searches competitors, then searches for macro context. The queries are generated dynamically — each result shapes what to search for next.

### Iterative Research — "search until confident"
The agent explicitly plans its research, executes searches, reflects on gaps, and issues follow-up queries until it can answer with confidence. This is the foundational pattern the other two use cases build on.

## Prerequisites

All examples require a running AgentCore Gateway with Web Search connector target.
Complete `01-gateway-setup-and-raw-mcp` first and export the environment variables it produces.
