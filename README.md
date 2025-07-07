Sure! Here's a cool, simple, and friendly README file you can add to your GitHub repo for this Ansible playbook:

---

# GCP Unhealthy Load Balancer Checker & Slack Notifier

👋 **Welcome!**
This Ansible playbook helps you automatically check the health of your **Google Cloud Platform (GCP) global backend services (load balancers)** and sends a summary of any **unhealthy ones directly to your Slack channel**. This way, you catch issues early and can reduce unnecessary costs caused by unhealthy load balancers.

---

## Why use this playbook?

* 🚦 **Automated health check:** No more manual checking of your load balancers.
* 🛎️ **Slack notifications:** Get instant alerts in Slack if any backend services are unhealthy.
* 💰 **Cost saver:** Quickly spot and fix unhealthy load balancers to avoid wasting GCP resources.
* 😎 **Easy to use:** Just provide your GCP service account and Slack token — and you’re good to go!

---

## How it works

1. Lists all global backend services (load balancers) in your GCP project.
2. Checks the health status of each backend service.
3. Skips serverless backends (since they behave differently).
4. Collects all unhealthy load balancers.
5. Sends a neat table of unhealthy services to a Slack channel using your Slack bot token.

---

## Prerequisites

* Ansible installed on your local machine or automation server.
* `gcloud` CLI installed and configured.
* A GCP Service Account JSON file with permissions to read backend services.
* Slack Bot Token with permission to post messages in your chosen channel.

---

## How to use

1. Clone this repo or copy the playbook to your environment.

2. Create a variables file or export environment variables:

```yaml
gcp_terraform_sa: /path/to/your/gcp-service-account.json
slack_token: xoxb-your-slack-bot-token
```

3. Run the playbook:

```bash
ansible-playbook check-unhealthy-lbs.yml -e "gcp_terraform_sa=/path/to/sa.json slack_token=xoxb-your-token"
```

---

## Example Slack message

```
Unhealthy Load Balancers:
-------------------------------------------------------------------------
| Load Balancer | Backend Service | Region        | Status        |
-------------------------------------------------------------------------
| Global LB    | my-backend-svc  | N/A           | UNHEALTHY    |
| Global LB    | another-backend | N/A           | UNHEALTHY    |
```

---

## Troubleshooting

* Make sure your GCP Service Account JSON has **read permissions** on backend services.
* Ensure your Slack token is valid and has **chat\:write** permission.
* `gcloud` CLI commands should work manually on your machine with the provided service account.

---

## Contributing

Feel free to open issues or submit pull requests if you want to improve this playbook!

---

## License

MIT License © 2025 Maged Elgamal

---

If you want, I can help you generate the README in Markdown format for direct GitHub use!
