# Product Expert Voice Agent

An n8n-based AI agent that answers product-related questions by
combining an LLM with a Supabase vector knowledge base. The workflow
receives a question through a webhook, uses an AI Agent to determine the
answer, retrieves relevant product information from the vector store,
and returns the response through the webhook.

## Project Overview

**Project:** Product Expert Voice Agent

**Purpose:** Provide product-focused answers using retrieval-augmented
generation (RAG) through an n8n AI Agent.

### Workflow

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

The AI Agent receives the question from the webhook request body and can
use the Supabase vector store as a tool to look up product information.

## Key Features

- Webhook-based question intake using HTTP `POST`.
- AI Agent orchestration in n8n.
- OpenRouter-powered chat model.
- Supabase Vector Store used as a retrieval tool.
- Cohere `embed-english-v3.0` embeddings.
- Top 10 vector-search results configured for retrieval.
- Product-information lookup through the AI Agent.
- Webhook response returned through a dedicated response node.

## Technologies & Dependencies

Technology Usage

---

n8n Workflow automation and AI Agent orchestration
OpenRouter Chat model provider
`nex-agi/nex-n2.5-pro:free` Configured OpenRouter chat model
Supabase Vector store / knowledge base
Cohere Text embeddings
`embed-english-v3.0` Configured embedding model

### n8n Nodes

- Webhook
- AI Agent
- OpenRouter Chat Model
- Supabase Vector Store
- Embeddings Cohere
- Respond to Webhook

## Prerequisites

Before importing and running the workflow, you need:

1.  A working n8n instance.
2.  An OpenRouter API credential configured in n8n.
3.  A Supabase credential configured in n8n.
4.  A Supabase `knowledgebase` table containing the product knowledge
    used for retrieval.
5.  A Cohere API credential configured in n8n.
6.  Embeddings in the Supabase vector store that are compatible with the
    configured Cohere embedding model.

> The workflow JSON contains credential references, but API keys/secrets
> are not included in this documentation.

## Setup

### 1. Import the workflow

Import the workflow JSON file into your n8n instance.

### 2. Configure OpenRouter

Open the **OpenRouter Chat Model** node and configure the OpenRouter
credential.

The workflow is configured to use:

```text
nex-agi/nex-n2.5-pro:free
```

### 3. Configure Supabase

Open the **Supabase Vector Store** node and configure the Supabase
credential.

The configured table is:

```text
knowledgebase
```

The vector store is configured in **retrieve-as-tool** mode so the AI
Agent can call it when product information is needed.

### 4. Configure Cohere embeddings

The workflow uses:

```text
embed-english-v3.0
```

Configure the Cohere credential in the **Embeddings Cohere** node.

### 5. Verify the knowledge base

Make sure the `knowledgebase` table contains the product information
that the agent should retrieve.

The workflow configures:

```text
Top K = 10
```

so up to 10 relevant retrieval results are requested by the vector-store
tool.

## Usage

The workflow expects an HTTP `POST` request to the n8n webhook.

The AI Agent reads the question from:

```text
body.question
```

### Example request

```json
{
  "question": "What products does the company offer?"
}
```

The workflow then:

1.  Receives the request through the Webhook node.
2.  Passes `body.question` to the AI Agent.
3.  Uses the OpenRouter chat model for the agent's language-model
    reasoning.
4.  Allows the agent to query the Supabase Vector Store for product
    information.
5.  Uses Cohere embeddings for vector retrieval.
6.  Sends the agent's result to the Respond to Webhook node.

## Project Structure

```text
Product Expert voice agent/
├── README.md
└── Product Expert voice agent.json
```

> The exact JSON filename should match the workflow file stored in the
> repository.

## Workflow Configuration

Component Configuration

---

Webhook method POST
Webhook response mode Response Node
AI Agent input `{{$json.body.question}}`
Vector store mode Retrieve as Tool
Vector table `knowledgebase`
Retrieval Top K 10
Embedding model `embed-english-v3.0`
Chat model `nex-agi/nex-n2.5-pro:free`

## What This Project Demonstrates

This project demonstrates a compact RAG-based AI Agent pattern in n8n:

- Accepting external questions through a webhook.
- Connecting an LLM to an external knowledge source.
- Giving an AI Agent access to retrieval as a tool.
- Using vector embeddings for semantic product-information retrieval.
- Returning an automated answer through an API-style webhook workflow.

## Notes

The supplied workflow defines the AI Agent prompt input and tool
connections, but does not include a custom system prompt or additional
business rules in the `options` field. Documentation therefore does not
assume response formatting, voice synthesis, authentication,
conversation memory, or other functionality beyond what is present in
the workflow.

## Security

Do not commit API keys, access tokens, or other credentials to the
repository. Use n8n credentials/environment configuration for secrets.
