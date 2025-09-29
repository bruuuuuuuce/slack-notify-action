# Slack Notify Action

[![GitHub release](https://img.shields.io/github/v/release/org/slack-notify-action?logo=github&style=flat-square)](https://github.com/org/slack-notify-action/releases)
[![Workflow Test](https://img.shields.io/github/actions/workflow/status/org/slack-notify-action/test-action.yml?label=tests&logo=github&style=flat-square)](https://github.com/org/slack-notify-action/actions)
[![License](https://img.shields.io/github/license/org/slack-notify-action?style=flat-square)](LICENSE)

A reusable GitHub Action that sends a Slack message when a deployment succeeds or fails.  
The message includes repo, commit, and actor details, and shows **green for success** or **red for failure**.

---

## 🚀 Usage

Add this step to your workflow:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "🚀 Deploying..." # your deployment command

  notify:
    needs: deploy
    if: always()
    runs-on: ubuntu-latest
    steps:
      - name: Send Slack notification
        uses: org/slack-notify-action@v1
        with:
          environment: "production"
          message: ${{ needs.deploy.result == 'success' && 'Deployment finished successfully 🎉' || 'Deployment failed 🚨' }}
          isSuccess: ${{ needs.deploy.result == 'success' }}
          slack_webhook_url: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## Inputs

| Name                | Required | Type    | Description                                                           |
| ------------------- | -------- | ------- | --------------------------------------------------------------------- |
| `environment`       | ✅        | string  | The environment being deployed (e.g. `staging`, `production`).        |
| `message`           | ❌        | string  | Custom message to include. Defaults to `"Deployment finished"`.       |
| `isSuccess`         | ✅        | boolean | Whether the deployment succeeded (`true` = green ✅, `false` = red ❌). |
| `slack_webhook_url` | ✅        | string  | Slack Incoming Webhook URL (set via GitHub secret).                   |
