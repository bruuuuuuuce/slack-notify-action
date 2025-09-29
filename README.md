# Slack Deploy Notify Action

Send a Slack notification on deployment success or failure using a bot token, with color coding and full visibility.

---

## Features

- Automatically sets **success (green)** or **failure (red)** colors.  
- Includes a clickable **“build logs”** link if deployment fails.  
- Shows **repo, commit, and actor** in Slack context block.  
- Works as a reusable **composite GitHub Action**.

---

## Usage

### Example Workflow

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "🚀 Deploying..." # your deployment commands

  notify:
    needs: deploy
    if: always()
    runs-on: ubuntu-latest
    steps:
      - name: Send Slack notification
        uses: bruuuuuuuce/slack-deploy-notify-action@v1
        with:
          environment: "production"
          is-success: ${{ needs.deploy.result == 'success' }}
          channel-id: "C08H67G6D3N"
          slack-bot-token: ${{ secrets.SLACK_BOT_TOKEN }}
```

## Inputs

| Name              | Required | Description                                                     |
| ----------------- | -------- | --------------------------------------------------------------- |
| `environment`     | Yes      | Deployment environment (e.g., `production`, `staging`)          |
| `is-success`      | Yes      | Whether the deployment succeeded (`true` or `false`)            |
| `channel-id`      | Yes      | Slack channel ID to post the notification (e.g., `C08H67G6D3N`) |
| `slack-bot-token` | Yes      | Slack Bot Token (xoxb-...)                                      |
