# n8n AI Automation Workflows

**Author: Rishiraj Bal**

This repository contains a curated collection of modular, AI-powered automation workflows, designed and maintained by **Rishiraj Bal**. Each workflow leverages **[n8n](https://n8n.io/)** as the orchestration backbone — combining LLM agents, API integrations, dynamic data pipelines, and structured output handling into robust, production-ready systems.

**➡ [Jump to Self-Hosting Guide](#self-hosting-n8n)**

---

## About n8n

**n8n** is an extensible, node-based workflow automation framework for developers who prefer open standards and granular control. It allows you to link disparate services, process data streams, and embed custom logic — all without sacrificing transparency or maintainability.

---

## Design Principles

* **Composable AI Pipelines:** Combine multiple LLMs, vector memory, and tools as modular nodes.
* **Deterministic Outputs:** Enforce schema correctness using structured output parsers.
* **Orchestrated Agents:** Delegate multi-step tasks to chainable AI agents and toolsets.
* **Real-World Data:** Integrate with external APIs like NASA, GitHub, or internal services.
* **Operational Security:** All secrets managed via environment variables and n8n’s credential store — no hardcoding.

---

## Workflow Structure

Each workflow generally includes:

1. **Trigger Node:** Listens for incoming events (e.g., chat messages via webhooks).
2. **Memory Node:** Buffers context to persist conversational or process state.
3. **Agent Node:** Executes LLM calls, tool invocations, and logic branches.
4. **External API Nodes:** Pulls or pushes data to services such as NASA’s public APIs.
5. **Output Parser:** Validates and transforms model outputs into strict JSON structures.
6. **Action Nodes:** Performs downstream actions — email dispatch, DB writes, notifications.

---

## Customizing Outputs

To adapt outputs to your use case:

* Locate the `Structured Output Parser` node.
* Modify the `inputSchema` JSON to define your target output format.
* When using a chat template, embed the updated schema directly in the prompt payload if needed. This ensures the language model produces output that matches the defined structure without post-hoc correction.

---

## Security Practices

* **API keys and OAuth credentials** must be configured in n8n’s encrypted credential manager or injected at runtime.
* Secrets should **never** be hardcoded in nodes or workflow definitions.
* Audit all integrations regularly — especially third-party plugins and custom nodes.

---

## Self-Hosting n8n

**Deploying your own n8n instance gives you full control over your workflows, secrets, and runtime.**

Below is a minimal self-hosting recipe using Docker — the standard for robust local or server deployments.

### 1. Install Docker

Install Docker and Docker Compose on your machine or VPS.

```bash
# Example for Ubuntu
sudo apt-get update
sudo apt-get install docker.io docker-compose
```

### 2. Pull the Latest n8n Image

```bash
docker pull n8nio/n8n
```

Always verify you’re pulling from the official source.

### 3. Create a Docker Compose File

Example `docker-compose.yml`:

```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=<your_username>
      - N8N_BASIC_AUTH_PASSWORD=<your_password>
      - N8N_HOST=<your_domain_or_ip>
      - N8N_PORT=5678
      - NODE_ENV=production
    volumes:
      - ~/.n8n:/home/node/.n8n
```

### 4. Start the Container

```bash
docker-compose up -d
```

Your n8n instance will be accessible at `http://<your_domain_or_ip>:5678`.

### 5. Secure It

* Use HTTPS (reverse proxy like NGINX + Let’s Encrypt).
* Enable basic auth or OAuth for the editor UI.
* Back up your `~/.n8n` folder regularly.

### Why Self-Host?

* **Data Privacy:** Full control over credentials and execution context.
* **Customization:** Deploy custom nodes and plugins.
* **Scalability:** Integrate with your infrastructure, autoscale containers as needed.
* **Cost Control:** No vendor lock-in or forced pricing tiers.

---

## License & Usage

All workflows in this repository are shared under an open-use fair-code approach. Feel free to fork, extend, or integrate them into your own pipelines. Attribution is expected if you build on this work at scale or redistribute derivative solutions.
