# 🤖 AI-Powered Portfolio Rebalancer

An AI-powered portfolio rebalancing workflow built with **n8n** that
reads portfolio data, retrieves market information, performs
calculations, determines rebalancing actions, updates the portfolio, and
sends notifications.

> **Project 1/3 --- AI Engineering Projects \| n8n Automation Series**

---

## 🎯 Project Objective

The objective of this project was to understand how an **AI Agent can
coordinate multiple tools and data sources to complete a real-world
business workflow**.

Instead of building a simple LLM chatbot, this project explores an
agentic workflow where the AI can:

- Understand a user's portfolio rebalancing instruction
- Read portfolio holdings from Google Sheets
- Retrieve market prices
- Reuse previously stored market data where possible
- Perform portfolio calculations
- Determine **BUY / SELL / HOLD** actions
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
      Google Sheets            Calculator             MarketStack
       Portfolio Data          Calculations            Market Data
             │                      │                      │
             └──────────────────────┼──────────────────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │ Rebalancing Decision │
                         │    BUY / SELL / HOLD │
                         └──────────┬───────────┘
                                    │
                                    ▼
                         ┌──────────────────────┐
                         │  Portfolio Update    │
                         │    Google Sheets     │
                         └──────────┬───────────┘
                                    │
                         ┌──────────┴──────────┐
                         ▼                     ▼
                       Gmail                Pushover
                  Email Notification      Push Notification
```

---

## 🛠️ Technology & Tools

Technology / Tool Purpose

---

**n8n** Workflow automation and AI orchestration
**LLM via OpenRouter** AI reasoning layer
**Google Sheets** Portfolio and market-data storage
**MarketStack** Market data API
**Calculator Tool** Portfolio and rebalancing calculations
**Gmail** Email notifications
**Pushover** Push notifications

---

## 🔄 How the Workflow Works

### 1. Portfolio Rebalancing Request

The workflow starts with an **n8n form submission**.

Example:

> Ensure portfolio is approximately 60% equity and 40% fixed income.

The request is passed to the AI Agent as the user's rebalancing
instruction.

---

### 2. Read Portfolio Data

The AI Agent reads portfolio information stored in Google Sheets,
including:

- Ticker
- Quantity
- Equity allocation
- Fixed-income allocation
- Price
- Total value

This provides the current portfolio state before making a rebalancing
decision.

---

### 3. Check Existing Market Data

The workflow uses a **market-data caching approach**.

Instead of calling the MarketStack API every time, it checks whether
market data is already available.

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

This approach helps reduce unnecessary API consumption.

### Why This Matters

During testing, the available free MarketStack API quota was consumed
quickly.

That became a practical lesson:

> **An AI workflow should not only work --- it should also use external
> APIs efficiently.**

---

### 4. Portfolio Calculation

The AI Agent uses the Calculator tool to determine:

- Current value of each position
- Total portfolio value
- Current allocation percentage
- Target allocation
- Allocation deviation
- Required share quantity

The workflow is designed around **whole-share trading**, so fractional
shares are not used.

---

### 5. Rebalancing Logic

The AI Agent generates:

- **BUY**
- **SELL**
- **HOLD**

decisions based on the target portfolio allocation.

The workflow attempts to keep each position within a defined **±2%
allocation tolerance**.

It can perform up to **3 rebalancing attempts** when the target
tolerance is not achieved on the first attempt.

---

### 6. Update Portfolio

After generating the rebalancing decisions, the workflow updates the
Google Sheet with the new quantity after rebalancing.

The workflow tracks information such as:

- Ticker
- Price
- Current quantity
- New quantity
- Total value
- New portfolio value

---

### 7. Notifications

Once the workflow completes, it can send:

#### 📧 Email

A rebalancing summary is sent through Gmail.

#### 📱 Push Notification

Pushover provides a quick success/failure notification.

The workflow also contains a final success/failure check so the user
receives a clear status.

---

# 🧠 Key AI Engineering Concepts

This project was more than connecting n8n nodes.

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

Learning that API usage needs to be considered when designing AI
workflows.

### 4. Data Reuse / Caching

Introducing a stored market-data layer to reduce unnecessary API calls.

### 5. Verification

The workflow includes verification steps rather than assuming that a
write or calculation succeeded.

### 6. Iterative Reasoning

The rebalancing process can be repeated when the portfolio remains
outside the target tolerance.

### 7. End-to-End Automation

The project connects:

**User Input → AI Reasoning → Data → Calculation → Decision → Update →
Notification**

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

# 📁 Project Structure

```text
AI-Powered Portfolio Rebalancer/
│
├── README.md
├── Equity Portfolio Rebalancer.json
└── Equity portfolio rebalancer.png
```

### Workflow File

**[Equity Portfolio
Rebalancer.json](./Equity%20Portfolio%20Rebalancer.json)**

Import this JSON into n8n to inspect and run the workflow.

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

1.  Clone or download the repository.
2.  Open your n8n instance.
3.  Import **`Equity Portfolio Rebalancer.json`**.
4.  Configure your own credentials for:
    - Google Sheets
    - MarketStack
    - LLM / OpenRouter
    - Gmail
    - Pushover
5.  Configure your Google Sheet structure.
6.  Review the AI Agent instructions.
7.  Test the workflow with sample portfolio data.
8.  Validate calculations and outputs before activating the workflow.

---

# 🧪 Example Use Case

A user submits a portfolio instruction such as:

```text
Ensure portfolio is approximately 60% equity and 40% fixed income.
```

The workflow then:

```text
User Request
     ↓
AI Agent
     ↓
Read Portfolio
     ↓
Get / Reuse Market Data
     ↓
Calculate Current Allocation
     ↓
Compare With Target
     ↓
BUY / SELL / HOLD
     ↓
Update Portfolio
     ↓
Verify Result
     ↓
Email + Push Notification
```

---

# 🔐 Security & Important Note

**Do not publish API keys, OAuth credentials, webhook secrets, personal
email addresses, user keys, or other credentials in a public
repository.**

Before making the repository public:

- Replace credentials with your own
- Remove sensitive IDs and secrets
- Review webhook configuration
- Review notification configuration
- Use environment variables where appropriate
- Use test portfolio data rather than real financial information

> ⚠️ **This project is for learning and experimentation. It is not
> financial advice and should not be used to automatically execute real
> trades without appropriate safeguards and human review.**

---

# 💡 What I Learned

The biggest takeaway from this project was that building AI automation
is not just about adding an LLM.

A useful AI workflow needs:

**Reasoning + Tools + Data + Validation + Error Handling + Cost
Awareness**

The MarketStack API experience was especially valuable because it forced
me to think about **API efficiency and data reuse**, rather than
treating external APIs as unlimited resources.

---

# 🚀 Project Status

**Project 1/3 --- Completed ✅**

This project is part of an AI Engineering learning journey focused on:

**AI Engineering \| LLMs \| AI Agents \| RAG \| APIs \| Automation \| AI
Product Engineering**

More projects will be added to the series as they are built.

---

## ⭐ Learning in Public

I'm documenting these projects to share what I'm learning while moving
deeper into AI Engineering and practical AI automation.

If you are also learning by building, feel free to explore the workflow,
experiment with it, and improve it.

⭐ If you find the project useful, consider starring the repository.
