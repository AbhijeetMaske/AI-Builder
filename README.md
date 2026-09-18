# 🤖 AI Engineering Projects --- n8n Automation Series

> **Learning by building:** AI Agents, LLMs, APIs, Tools & Automation

This repository contains hands-on AI engineering and n8n automation
projects built to explore practical AI agents, LLM integrations, APIs,
RAG, vector databases, tool calling, and workflow automation.

## 📚 Projects

---

\# Project Status Documentation

---

1/3 [AI-Powered Portfolio ✅ Completed [Project
Rebalancer](#13-ai-powered-portfolio-rebalancer) README](./AI-Powered%20Portfolio%20Rebalancer/AI-Powered%20Portfolio%20Rebalancer.md)

2/3 [Product Expert Voice ✅ Completed [Project
Agent](#23-product-expert-voice-agent) README](./Product%20Expert%20Voice%20Agent/Product%20Expert%20Voice%20Agent.md)

3/3 Coming soon 🚧 Planned ---

---

---

## 1/3 --- AI-Powered Portfolio Rebalancer

An n8n automation project that combines an AI Agent, Google Sheets,
market data, calculations, and notifications to automate portfolio
rebalancing decisions.

### Core workflow

```text
Form Trigger
     ↓
AI Agent
     ├── Google Sheets
     ├── MarketStack
     └── Calculator
     ↓
BUY / SELL / HOLD Decision
     ↓
Update Portfolio
     ├── Gmail
     └── Pushover
```

### Key concepts

- AI Agent orchestration
- Tool calling
- LLM integration
- Market-data APIs
- Google Sheets integration
- Portfolio calculations
- Data reuse/caching
- Iterative verification
- Automated notifications

### Files

```text
AI-Powered Portfolio Rebalancer/
├── AI-Powered Portfolio Rebalancer.md
├── Equity Portfolio Rebalancer.json
└── image/
```

> The project documentation lives in the markdown file inside the project folder.

---

## 2/3 --- Product Expert Voice Agent

A compact n8n RAG workflow that accepts product questions through a
webhook and uses an AI Agent with a Supabase vector knowledge base to
retrieve relevant product information.

### Core workflow

```text
Webhook
   ↓
AI Agent
   ├── OpenRouter Chat Model
   └── Supabase Vector Store
          └── Cohere Embeddings
   ↓
Respond to Webhook
```

### Key concepts

- AI Agent orchestration
- RAG / vector retrieval
- Tool-based knowledge retrieval
- OpenRouter LLM integration
- Supabase Vector Store
- Cohere embeddings
- Webhook-based API interaction

### Main configuration

Component Configuration

---

Input HTTP POST webhook
Question field `body.question`
Chat model `nex-agi/nex-n2.5-pro:free`
Vector store Supabase
Knowledge table `knowledgebase`
Retrieval mode Retrieve as Tool
Top K 10
Embeddings Cohere `embed-english-v3.0`
Response Respond to Webhook

### Project files

```text
Product Expert Voice Agent/
├── Product Expert Voice Agent.md
├── Product Expert voice agent.json
└── images/
```

📖 **Detailed documentation:** [Product Expert Voice Agent
README](./Product%20Expert%20Voice%20Agent/Product%20Expert%20Voice%20Agent.md)

---

## 🗂️ Repository Structure

```text
AI-Engineering-Projects/
│
├── README.md
│
├── AI-Powered Portfolio Rebalancer/
│   ├── README.md
│   ├── Equity Portfolio Rebalancer.json
│   └── Equity portfolio rebalancer.png
│
├── Product Expert Voice Agent/
│   ├── Product Expert Voice Agent.md
│   └── Product Expert voice agent.json
│
└── Project 3/
    └── Coming soon
```

The structure above is the intended documentation structure. Keep
project-specific workflow files and documentation together so each
project can be understood and run independently.

---

## 🛠️ General Setup

Most projects in this repository are based on n8n workflows.

Typical setup:

1.  Install or run n8n.
2.  Open the target project folder.
3.  Import the project's `.json` workflow into n8n.
4.  Configure the required credentials.
5.  Configure project-specific data sources and tables.
6.  Test the workflow with sample input.
7.  Review the execution output and adjust configuration where required.

Each project README contains the setup requirements specific to that
workflow.

---

## 🔐 Security

Never commit:

- API keys
- Access tokens
- Database passwords
- Private credentials
- Production secrets

Use n8n's credential management or environment configuration for
sensitive values.

---

## 🎯 Learning Focus

The projects are intended to build practical experience across:

- AI Agents
- LLM integrations
- RAG
- Vector databases
- Embeddings
- API integrations
- Tool calling
- Workflow automation
- Data processing
- Notifications
- Verification and error handling

---

## 🚀 Project Roadmap

- [x] Project 1/3 --- AI-Powered Portfolio Rebalancer
- [x] Project 2/3 --- Product Expert Voice Agent
- [ ] Project 3/3 --- Coming soon

---

## 📌 Documentation Approach

Documentation is kept project-specific and implementation-focused.
Features, dependencies, inputs, and setup steps should reflect the
actual workflow implementation rather than assumed capabilities.

More projects will be added to this repository as the learning series
progresses.
