# Resource Cleanup

## Overview

Deletes all AWS resources created by the setup script: Gateway targets, Gateway, Cognito User Pool, and IAM role.

## Usage

Use the resource IDs printed by `01-setup-gateway/setup_gateway.py`:

```bash
python cleanup.py \
  --gateway-id gw-abc123def456 \
  --user-pool-id us-east-1_AbCdEfGh \
  --role-name agentcore-web-search-gateway-role
```

## Files

| File | Description |
|:-----|:------------|
| `cleanup.py` | Deletes Gateway, Cognito, and IAM resources |
