# GitHub Actions Shared Workflows Repository

This repository is designed to host shared and reusable GitHub Actions workflows for the organization. By centralizing common automation logic here, you can:

- Reduce duplication across repositories
- Standardize CI/CD and notification processes
- Easily maintain and update workflows in one place

Workflows in this repo can be referenced from any other repository in the organization using the `workflow_call` feature, making it simple to share automation logic.

---

## RingCentral PR Review Notification Workflow

One example workflow in this repository sends notifications to RingCentral when a review is requested from specific GitHub teams on a pull request.

### Features

- Supports multiple teams, each mapped to a different RingCentral webhook
- Sends a formatted message to the appropriate RingCentral channel when a team is requested for review
- Easily reusable in other repositories via `workflow_call`

---

### How to Use in Other Repositories

1. Reference the workflow in your repo's `.github/workflows/` directory:
   ```yaml
   name: RingCentral PR Review Notification
   on:
     pull_request:
       types: [review_requested]
   jobs:
     notify:
       uses: vector-remote-care/github-actions/.github/workflows/ringcentral-reviewer-notify.yml@main
   ```
2. Make sure your teams and webhook URLs are configured in the shared workflow.

---

### Local Testing with act

You can test the workflow locally using [act](https://github.com/nektos/act):

1. Create a test event file, e.g. `.github/workflows/pull_request_review_requested.json`:
   ```json
   {
     "pull_request": {
       "html_url": "https://github.com/vector-remote-care/github-actions/pull/1"
     },
     "requested_reviewer": {
       "type": "Team",
       "login": "da"
     }
   }
   ```
2. Run the workflow locally:
   ```sh
   act -e .github/workflows/pull_request_review_requested.json -j notify
   ```

---

### Customization

- To add more teams or change webhook URLs, edit the mapping logic in `.github/workflows/ringcentral-reviewer-notify.yml`.
- For questions or improvements, open an issue or pull request.
