# Student Work — n8n Workflows & Automation

Welcome to my personal collection of n8n automation workflows. I'm a student building these workflows both for fun and to generate some income on the side. This repository contains ready-to-import n8n workflow JSON files covering LinkedIn automation, HVAC lead generation, Instagram content, and AI-powered outreach.

---

## Workflows

| File | Description |
|------|-------------|
| `linkedin-ai-automation-workflow.json` | Automates LinkedIn engagement and outreach using AI |
| `linkedin-ai-content-automation.json` | AI-generated LinkedIn post creation and scheduling |
| `linkedin-ai-ml-learning-workflow.json` | Curates and shares AI/ML learning content on LinkedIn |
| `hvac-ai-email-outreach-workflow.json` | AI-driven email outreach for HVAC businesses |
| `hvac-lead-response-workflow.json` | Automated lead follow-up and response for HVAC companies |
| `instagram-wallpaper-automation.json` | Automates wallpaper content creation and posting to Instagram |

---

## How to Use

1. Open your n8n instance.
2. Go to **Workflows → Import from File**.
3. Select any `.json` file from this repository.
4. Configure the required credentials (LinkedIn, Gmail, Instagram, OpenAI, etc.).
5. Activate the workflow.

---

## n8n-MCP Server

The [`n8n-mcp/`](./n8n-mcp/) folder contains a full copy of the [n8n-MCP](https://github.com/czlonkowski/n8n-mcp) server — a Model Context Protocol (MCP) server that gives AI assistants (like Claude) deep knowledge of all n8n nodes. I use it to help build and validate workflows with AI assistance.

See [`n8n-mcp/README.md`](./n8n-mcp/README.md) for setup and usage instructions.

---

## Requirements

- [n8n](https://n8n.io/) v1.x or later
- API credentials for the services used in each workflow (see individual workflow documentation inside the JSON files)

---

## License

The workflows in this repository are shared under the [MIT License](./n8n-mcp/LICENSE).  
The n8n-MCP server in the `n8n-mcp/` folder is also MIT licensed — credit to [Romuald Członkowski](https://www.aiadvisors.pl/en).

---

*Built by a student, for learning and a little income. Feel free to fork and adapt!*
