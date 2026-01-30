# Intelligent RAG Helpdesk Agent

## Project Overview
An enterprise-grade automated support system designed to handle technical tickets efficiently. Unlike standard auto-responders, this agent uses **RAG (Retrieval-Augmented Generation)** logic. It understands the context of incoming emails by querying a vector database (Pinecone) for relevant internal documentation before drafting a response using GPT-4o.

The system includes a **Human-in-the-Loop** mechanism, ensuring no AI-generated email is sent without manager oversight via Slack.

## Key Features
* **Semantic Search Engine:** Utilizes OpenAI Embeddings to convert incoming queries into vectors and retrieves the most relevant knowledge base articles from Pinecone.
* **Vector Database Integration:** Implements Pinecone index with cosine similarity for high-precision context retrieval.
* **Automated Drafting:** Generates context-aware, empathetic, and technically accurate email drafts using GPT-4o.
* **Manager Oversight:** Sends notifications to Slack for approval before releasing the response to the client.
* **Spam Filtering:** Gmail trigger configured with advanced filters to process only relevant support tickets.

## Tech Stack
* **Orchestration:** n8n (Workflow Automation)
* **AI & LLM:** OpenAI GPT-4o (Reasoning), text-embedding-3-small (Vectorization)
* **Database:** Pinecone (Vector Store)
* **Communication:** Gmail API, Slack API

## Architecture

The system consists of two distinct pipelines:

### 1. Knowledge Ingestion Pipeline (Utility)
A dedicated workflow that:
1.  Reads internal documentation (PDF/Text).
2.  Chunks the text into manageable segments.
3.  Generates vector embeddings.
4.  Upserts vectors into the **Pinecone** index.

### 2. Support Response Pipeline (Main Agent)
The core workflow operates as follows:
1.  **Trigger:** Listens for new emails with specific labels/subjects in Gmail.
2.  **Embed:** Converts the user's problem description into a vector.
3.  **Retrieve:** Queries Pinecone for the top 3 most relevant documentation fragments.
4.  **Synthesize:** Passes the user query + retrieved context to GPT-4o to draft a solution.
5.  **Notify:** Pushes the draft to a private Slack channel for review.
6.  **Action:** Sends the email upon verification.


## How It Works (RAG Logic)
Instead of hallucinating answers, the bot is grounded in facts.
* *User asks:* "Error 691 on VPN".
* *Bot searches:* Finds internal policy regarding "VPN Error 691" and "Cisco AnyConnect reset".
* *Bot answers:* "Based on our policy, please reset Cisco AnyConnect. If that fails, contact Admin..."

## Setup & Requirements
1.  **n8n** (Self-hosted or Cloud).
2.  **Pinecone API Key** (Index dimensions: 1536, Metric: Cosine).
3.  **OpenAI API Key**.
4.  **Slack App** with incoming webhook/bot permissions.
5.  **Google Cloud Console** project with Gmail API enabled.
