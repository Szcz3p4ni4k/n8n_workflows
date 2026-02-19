# Automated Incident Monitoring and AI Diagnostics Workflow


## Project Overview
This project is an automated incident response system built using n8n. It captures service outage alerts via webhooks, performs a real-time verification check to eliminate false positives, utilizes an AI Agent (LLM) to diagnose the potential root cause, and manages multi-channel notifications and logging.

The workflow demonstrates a robust "self-healing" logic by distinguishing between temporary glitches and sustained outages.

## Key Features
1. **Real-time Triggering:** Receives incident payloads from external monitoring tools (e.g., UptimeRobot, GitHub Actions) via a Webhook.
2. **Service Verification**: Executes an automated HTTP GET request to verify the current status of the reported service.
3. **Conditional Branching:** Implements logic to handle two distinct scenarios:
4. **Service Recovery:** If the verification returns a 200 OK status, the system notifies the team that the service is operational.
5. **Incident Diagnostic:** If the service remains down, the workflow triggers an AI-driven analysis.
6. **AI-Powered Diagnostics:** An AI Agent analyzes the error codes and service context to provide immediate troubleshooting steps for on-call engineers.
7. **Data Persistence:** Automatically logs every incident, error code, and AI analysis into a Google Sheets database for long-term reliability reporting.
8. **Instant Alerts:** Sends formatted Slack notifications with diagnostic data to ensure rapid response times.

## Workflow Architecture
1. Webhook Node: Entry point for JSON payloads containing service names and error details.
2. HTTP Request Node: Verifies the status of the target URL.
3. If Node: Checks if the HTTP Status Code equals 200.
3. True Branch (Recovery): Sends a Slack notification confirming the service is online.
4. False Branch (Failure):
    * AI Agent: Processes the incident data using the OpenAI Chat Model.
    * Google Sheets: Appends a new row with the incident timestamp, service name, and AI analysis.
    * Slack: Sends a high-priority alert containing the AI-generated troubleshooting guide.

## Technical Stack
* **Orchestration:** n8n (Workflow Automation)
* **AI & LLM:** OpenAI GPT-4o / GPT-4o-mini
* **Communication:** Slack API (Webhooks)
* **Database/Logging:** Google Sheets API

## Setup Instructions
1. n8n Configuration:
    * Import the provided JSON workflow file into your n8n instance.
    * Configure the Webhook URL to receive POST requests.

2. API Credentials:
    * Set up a Slack App and generate an Incoming Webhook URL.
    * Configure Google Cloud Console with Google Sheets API enabled and link OAuth2 credentials.
    * Provide an OpenAI API Key or use n8n credits for the AI Agent.