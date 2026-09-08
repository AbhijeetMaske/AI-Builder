# 🤖 AI Engineering Projects — n8n Automation Series

> **Learning by building: AI Agents, LLMs, APIs, Tools & Automation**

This repository documents my hands-on journey into **AI Engineering**, with a focus on building practical AI-powered workflows using **n8n, LLMs, APIs, data sources, automation tools, and notifications**.

The goal is simple:

**Learn → Build → Experiment → Break → Improve → Share**

This README will evolve as I complete each project in the **3-project learning series**.

---

## 📚 Project Series

| #       | Project                                                                          | Status         | Focus                                        |
| ------- | -------------------------------------------------------------------------------- | -------------- | -------------------------------------------- |
| **1/3** | [AI-Powered Portfolio Rebalancer](#-project-13--ai-powered-portfolio-rebalancer) | ✅ Completed   | AI Agent + n8n + Google Sheets + MarketStack |
| **2/3** | _Coming soon_                                                                    | 🚧 In progress | To be added                                  |
| **3/3** | _Coming soon_                                                                    | 🚧 Planned     | To be added                                  |

---

# 🚀 Project 1/3 — AI-Powered Portfolio Rebalancer

An AI-powered portfolio rebalancing workflow built with **n8n** that reads portfolio data, retrieves market information, performs calculations, determines rebalancing actions, updates the portfolio, and sends notifications.

### 🎯 Project Objective

The objective was to understand how an **AI Agent can coordinate multiple tools and data sources to complete a real-world business workflow**.

Instead of building a simple LLM chatbot, this project explores an agentic workflow where the AI can:

- Understand a user's portfolio rebalancing instruction
- Read portfolio holdings from Google Sheets
- Retrieve market prices
- Reuse previously stored market data where possible
- Perform portfolio calculations
- Determine BUY / SELL / HOLD actions
- Update portfolio quantities
- Send email and push notifications
- Verify the result before completing the workflow

---

## 🧩 Workflow Architecture

```text
                         ┌──────────────────────┐
                         │   Portfolio Request  │
                         │    n8n Form Trigger  │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │      AI Agent        │
                         │   LLM + Tool Calls   │
                         └──────────┬───────────┘
                                    │
             ┌──────────────────────┼──────────────────────┐
             │                      │                      │
             ▼                      ▼                      ▼
   ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
   │ Google Sheets   │    │  MarketStack    │    │   Calculator    │
   │ Portfolio Data  │    │  Market Prices  │    │ Rebalancing Math│
   └────────┬────────┘    └────────┬────────┘    └────────┬────────┘
            │                      │                      │
            └──────────────────────┼──────────────────────┘
                                   │
                                   ▼
                         ┌──────────────────────┐
                         │ Rebalancing Decision │
                         │   BUY / SELL / HOLD  │
                         └──────────┬───────────┘
                                    │
                     ┌──────────────┼──────────────┐
                     │              │              │
                     ▼              ▼              ▼
             ┌──────────────┐ ┌────────────┐ ┌───────────────┐
             │ Update Sheet │ │   Email    │ │ Push Notify   │
             └──────────────┘ └────────────┘ └───────────────┘
```

---

## 🛠️ Technology & Tools

- **[n8n](https://n8n.io/)** — Workflow automation and AI orchestration
- **LLM via OpenRouter** — AI reasoning layer
- **[Google Sheets](https://www.google.com/sheets/about/)** — Portfolio and market-data storage
- **[MarketStack](https://marketstack.com/)** — Market data API
- **Calculator Tool** — Portfolio and rebalancing calculations
- **Gmail** — Email notification
- **Pushover** — Push notifications

---

## 🔄 How the Workflow Works

### 1. Portfolio Rebalancing Request

The workflow starts with an **n8n form submission**.

Example request:

> Ensure portfolio is approximately 60% equity and 40% fixed income.

The request is passed to the AI Agent as the user's rebalancing instruction.

---

### 2. Read Portfolio Data

The AI Agent reads the portfolio information stored in Google Sheets, including:

- Ticker
- Quantity
- Equity allocation
- Fixed-income allocation
- Price
- Total value

This gives the agent the current portfolio state before making any decision.

---

### 3. Check Existing Market Data

One of the most important improvements I added was a **market-data caching approach**.

Instead of calling the MarketStack API every time:

```text
Portfolio Ticker
      │
      ▼
Check MarketStack Data Sheet
      │
      ├── Price available → Reuse stored data
      │
      └── Price unavailable → Call MarketStack API
                                      │
                                      ▼
                              Store latest data
```

This helps reduce unnecessary API consumption.

### 💡 Why I Added This

While testing the workflow, I consumed the available free MarketStack API quota very quickly.

That became a practical lesson:

> **An AI workflow should not only work — it should also use external APIs efficiently.**

The workflow therefore checks whether market data already exists before making another API request.

---

## 🧮 4. Portfolio Calculation

The AI Agent uses the Calculator tool to determine:

- Current value of each position
- Total portfolio value
- Current allocation percentage
- Target allocation
- Allocation deviation
- Required share quantity

The workflow is designed around **whole-share trading**, so fractional shares are not used.

---

## ⚖️ 5. Rebalancing Logic

The AI Agent generates:

- **BUY**
- **SELL**
- **HOLD**

decisions based on the target portfolio allocation.

The workflow attempts to keep each position within a defined **±2% allocation tolerance**.

It also allows up to **3 rebalancing attempts** when the target tolerance is not achieved on the first attempt.

---

## 📝 6. Update Portfolio

After generating the rebalancing decisions, the workflow updates the Google Sheet with the new quantity after rebalancing.

The workflow tracks information such as:

- Ticker
- Price
- Current quantity
- New quantity
- Total value
- New portfolio value

---

## 🔔 7. Notifications

Once the workflow completes, it can send:

### 📧 Email

A rebalancing summary is sent through Gmail.

### 📱 Push Notification

Pushover is used to provide a quick success/failure notification.

The workflow also contains a final success/failure check so the user receives a clear status.

---

# 🧠 Key AI Engineering Concepts Explored

This project was more than connecting n8n nodes.

The main learning areas were:

### 1. AI Agent Orchestration

Understanding how an LLM can decide when and how to use external tools.

### 2. Tool Calling

Connecting the AI Agent with:

- Google Sheets
- MarketStack
- Calculator
- Email
- Push notifications

### 3. External API Management

Learning that API usage needs to be considered when designing AI workflows.

### 4. Data Reuse / Caching

Introducing a stored market-data layer to reduce unnecessary API calls.

### 5. Verification

The workflow includes verification steps rather than assuming that a write or calculation succeeded.

### 6. Iterative Reasoning

The rebalancing process can be repeated when the portfolio remains outside the target tolerance.

### 7. End-to-End Automation

The project connects:

**User Input → AI Reasoning → Data → Calculation → Decision → Update → Notification**

---

# 📊 Learning Architecture

```text
                    USER
                     │
                     ▼
              n8n Form Trigger
                     │
                     ▼
                AI AGENT
                     │
        ┌────────────┼────────────┐
        ▼            ▼            ▼
     DATA          TOOLS        APIs
        │            │            │
        ▼            ▼            ▼
 Google Sheets   Calculator   MarketStack
        │            │            │
        └────────────┼────────────┘
                     │
                     ▼
             REBALANCING LOGIC
                     │
                     ▼
              PORTFOLIO UPDATE
                     │
             ┌───────┴────────┐
             ▼                ▼
           EMAIL          PUSH ALERT
```

---

# 📁 Repository Structure

```text
.
├── README.md
├── Equity Portfolio Rebalancer.json
└── Equity portfolio rebalancer.png
```

### Workflow File

**[Equity Portfolio Rebalancer.json](./Equity%20Portfolio%20Rebalancer.json)**

Import this JSON into n8n to inspect the workflow structure.

### Workflow Image

**[View workflow image](./Equity%20portfolio%20rebalancer.png)**

---

# ▶️ How to Use

## Prerequisites

You will need:

- An n8n instance
- Google Sheets access
- A MarketStack API account
- An LLM provider compatible with the workflow
- Gmail access for email notifications
- Pushover access for push notifications

## Setup

1. Clone this repository.
2. Open your n8n instance.
3. Import **`Equity Portfolio Rebalancer.json`**.
4. Configure your own credentials for:
   - Google Sheets
   - MarketStack
   - LLM / OpenRouter
   - Gmail
   - Pushover
5. Configure your own Google Sheet structure.
6. Review the AI Agent instructions.
7. Test the workflow with sample portfolio data.
8. Activate the workflow only after validating the calculations and outputs.

---

# 🔐 Security & Important Note

**Do not publish API keys, OAuth credentials, webhook secrets, personal email addresses, user keys, or other credentials in a public repository.**

The exported n8n workflow may contain credential references or environment-specific configuration.

Before making this repository public:

- Replace credentials with your own
- Remove sensitive IDs and secrets
- Review webhook configuration
- Review notification configuration
- Use environment variables where appropriate
- Use test portfolio data rather than real financial information

> ⚠️ **This project is for learning and experimentation. It is not financial advice and should not be used to automatically execute real trades without appropriate safeguards and human review.**

---

# 💡 What I Learned

The biggest takeaway from this project was that building AI automation is not just about adding an LLM.

A useful AI workflow needs:

**Reasoning + Tools + Data + Validation + Error Handling + Cost Awareness**

The MarketStack API experience was especially valuable because it forced me to think about **API efficiency and data reuse**, rather than treating external APIs as unlimited resources.

---

# 🚧 What's Next?

This is **Project 1 of 3** in my AI Engineering learning journey.

I'll continue adding the next projects to this same README as I build them.

### Coming next

**Project 2/3 → Coming soon 🚀**

**Project 3/3 → Coming soon 🚀**

The goal is not simply to complete a course.

The goal is to **build enough real workflows to understand how AI systems actually work end-to-end.**

---

## 🙌 Learning in Public

I'm documenting these projects to share what I'm learning while moving deeper into:

**AI Engineering | LLMs | AI Agents | RAG | APIs | Automation | AI Product Engineering**

If you are also learning by building, feel free to explore the workflow, experiment with it, and improve it.

⭐ If you find the project useful, consider starring the repository.

---

## 📌 Project Status

| Area                      | Status         |
| ------------------------- | -------------- |
| n8n AI Agent              | ✅ Completed   |
| Google Sheets integration | ✅ Completed   |
| MarketStack integration   | ✅ Completed   |
| Market data reuse         | ✅ Completed   |
| Portfolio calculations    | ✅ Completed   |
| Rebalancing logic         | ✅ Completed   |
| Portfolio update          | ✅ Completed   |
| Email notification        | ✅ Completed   |
| Push notification         | ✅ Completed   |
| Project documentation     | ✅ Completed   |
| Project 2/3               | 🚧 Coming soon |
| Project 3/3               | 🚧 Coming soon |

---

**Built as part of my AI Engineering learning journey — one project at a time. 🚀**
