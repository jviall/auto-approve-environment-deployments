# Auto Approve Environment Deployments

![Marketplace](https://img.shields.io/badge/GitHub%20Marketplace-Auto%20Approve%20Environment%20Deployments-brightgreen)

Easily automate the approval of pending deployments in GitHub Actions when using environment protection rules with multiple dependent jobs.

> [!NOTE]
> This action was forked from [paulsony13/auto-approve-environment-deployment](https://github.com/paulsony13/auto-approve-environment-deployment) to solve a couple issues that reduced its usability for the general public.
> All credit goes to @pualsony13 who first paved this clever solution!

---

## 📋 Problem Statement

When using GitHub Actions with **environment protection rules**, each dependent job in your workflow requires manual approval to proceed. For example, if you have four dependent jobs and environment protection is enabled, GitHub requires manual approval **four times** (or more based on the number of jobs). This defeats the purpose of automation by introducing excessive manual intervention.

### Example Workflow Challenge

- **Job A** → Requires manual approval
- **Job B** → Depends on Job A and also requires manual approval
- **Job C** → Depends on Job B and so on...

If environment protection rules are applied, manual approvals are required at each step.

---

## 🚀 Solution

This GitHub Action solves the problem by automating the approval process for subsequent jobs once the initial approval is granted. It works by:

1. Retrieving the environment ID of the targeted environment.
2. Auto-approving any pending deployments for that environment, reducing user intervention.

With this Action, you can hook it into any stage of your workflow, and it will auto-approve pending jobs seamlessly.

---

## 📖 Features

- **Automates Deployment Approvals**: Eliminates the need for repeated manual approvals for protected environments.
- **Customizable Environment Targeting**: Specify the environment name for approvals.
- **Secure API Access**: Leverages a GitHub token to securely interact with the GitHub API.
- **Conditional Execution**: Allows condition-based auto-approvals, such as only for production environments.

---

## 🛠️ Usage

### Prerequisites

1. **Environment Protection Rules**: Ensure your repository has environments with protection rules configured.
2. **GitHub Personal Access Token**: Generate a token with `repo` scope to authenticate API calls. At a mimimum it requires read & write access to `actions`, and read access to `environments`. You cannot use the `GITHUB_TOKEN` as granting it `enviornment` permissions in Github Actions are not currently supported.

### Inputs

| Name          | Description                  | Required | Default |
| ------------- | ---------------------------- | -------- | ------- |
| `environment` | The name of the environment. | ✅       | N/A     |
| `token`       | GitHub token for API access. | ✅       | N/A     |

### Example Workflow

```yaml
name: Deployment Workflow

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v5

      - name: Auto Approve Environment Deployment
        uses: jviall/auto-approve-environment-deployments@v1.0.0
        with:
          environment: production
          token: ${{ secrets.YOUR_PERSONAL_ACCESS_TOKEN }}
```
