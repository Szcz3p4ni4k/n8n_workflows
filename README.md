# n8n_workflows

## Project Overview
This project is a comprehensive financial assistant built with n8n. It leverages a dual-architecture approach:
1.  **Ingestion Pipeline:** Uses GPT-4o Vision to digitize physical receipts sent via Telegram into structured database records.
2.  **Autonomous AI Agent:** Deploys an LLM-based Agent equipped with custom tools (Google Sheets) to dynamically query financial history, perform calculations, and generate insights in natural language.

## Features
* **Agentic Workflow:** Utilizes n8n's AI Agent node with "Tool Use" capabilities to autonomously decide when and how to access the database.
* **Multimodal Input:** Automatically routes image files to the digitization pipeline and text queries to the AI Agent.
* **Receipt Digitization:** Extracts Date, Merchant, Category, and Amount from photos with high precision.
* **Dynamic Analytics:** The Agent can answer complex queries (e.g., "Compare my spending on food vs. transport") by executing real-time lookups on the Google Sheet.

## Tech Stack
* **Workflow Automation:** n8n
* **AI Architecture:**
    * **Vision:** OpenAI Chat Model (Receipt Parsing)
    * **Agent:** OpenAI-powered Agent with Tool Calling
* **Database:** Google Sheets (connected as a Tool)
* **Scripting:** JavaScript (ES6) for data transformation

## Workflow Architecture
The system logic is divided into two branches via a Switch node:

1.  **Receipt Branch (Image):**
    * Directs image files to a Vision Model.
    * Parses the output via JavaScript.
    * Commits structured data to Google Sheets.

2.  **Analyst Branch (Text):**
    * Directs text queries to an **AI Agent**.
    * The Agent is connected to a **Google Sheets Tool**.
    * Upon receiving a query, the Agent autonomously retrieves relevant rows from the spreadsheet to formulate an answer.


## Setup and Installation

### Prerequisites
* An active n8n instance.
* OpenAI API Key.
* Telegram Bot Token (via @BotFather).
* Google Cloud Console project with Google Sheets API enabled.

### Google Sheets Configuration
Create a new Google Sheet with the following headers in the first row:
* Column A: Date
* Column B: Store
* Column C: Category
* Column D: Amount
* Column E: Description

### Importing the Workflow
1.  Clone this repository.
2.  Open your n8n dashboard.
3.  Select "Import from File" and upload the `.json` workflow file included in this repository.

### Credential Configuration
You must configure the following credentials in n8n for the workflow to function:
1.  **Telegram API:** Enter your Bot Token.
2.  **OpenAI API:** Enter your API Key.
3.  **Google Sheets:** Authenticate using OAuth2.

## Usage
1.  Open the Telegram bot.
2.  Send a photo of a receipt.
3.  Wait for the confirmation message.
4.  Verify the new entry in the connected Google Sheet.

## Future Improvements
* **Conversational Analytics:** Implement a branch to handle text-based queries (e.g., "How much did I spend yesterday?") by reading data back from the Google Sheet.
* **Budget Alerts:** Add logic to send warnings when spending in a specific category exceeds a defined threshold.
* **Multi-user Support:** Filter data based on Telegram User ID to support multiple users in a single spreadsheet.
