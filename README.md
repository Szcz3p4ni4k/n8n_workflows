# n8n_workflows

## Project Overview
This project is an automated financial tracking system built with n8n. It leverages Multimodal AI (GPT-4o) to process images of receipts sent via Telegram and automatically logs structured data into a Google Spreadsheet.

The goal of this tool is to eliminate manual data entry for personal finance management by converting unstructured data (images) into structured database records (JSON/CSV) in real-time.

## Features
* **Image Recognition:** Accepts photos of receipts directly from a Telegram chat.
* **AI Extraction:** Uses OpenAI's GPT-4o vision capabilities to identify date, merchant, category, total amount, and a brief description.
* **Data Validation:** Includes a JavaScript logic layer to parse and validate the AI's output before storage.
* **Cloud Storage:** Automatically appends data to a designated Google Sheet.
* **User Feedback:** Sends a confirmation message back to the user with the extracted details.

## Tech Stack
* **Workflow Automation:** n8n (Self-hosted or Cloud)
* **AI Model:** OpenAI GPT-4o (Chat Model with Vision)
* **Database:** Google Sheets
* **Interface:** Telegram Bot API
* **Scripting:** JavaScript (ES6) for data transformation

## Workflow Architecture
The workflow consists of the following nodes:

1.  **Telegram Trigger:** Listens for incoming image messages.
2.  **OpenAI Node:** Sends the binary image data to the LLM with a system prompt to extract specific fields in JSON format.
3.  **Code Node:** Executes JavaScript to parse the string response from the LLM into a JSON object and handles potential formatting errors.
4.  **Google Sheets Node:** Maps the JSON keys to specific spreadsheet columns and appends the row.
5.  **Telegram Response:** Sends a formatted success message back to the user.


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
