# AI Email Auto-Responder Agent

An event-driven, autonomous AI agent that monitors inbound emails, classifies user intent, assigns urgency scores, and executes conditional routing. Built to demonstrate production-grade LLM orchestration, strict data validation, and automated observability.

## 📸 System Architecture

*(Note: Upload your final screenshot to your GitHub repo and name it `workflow-architecture.png` so it displays here).*

## 🧠 Core Pipeline & Logic

The workflow is divided into three distinct operational phases:

### 1. AI Decision Engine

* **Event Trigger:** Monitors a designated Gmail inbox for unread messages matching specific search criteria to prevent processing spam or internal emails.
* **LLM Intent Analysis:** Sends the raw email body to the **Groq API** (running `Llama-3.3-70b-versatile`). A highly optimized system prompt forces the LLM to categorize the email (Support/Sales/General), assign an urgency level (Low/Medium/High), and draft a contextual response.
* **Strict JSON Parsing:** A custom JavaScript node intercepts the LLM's stringified output, parsing it into a structured JSON object. It includes `try/catch` fallback logic to prevent pipeline crashes in the rare event of LLM formatting hallucinations.

### 2. Intelligent Routing

* **Conditional Forking:** Evaluates the extracted `urgency` variable.
* **High-Priority Escalation:** If urgency is "High", the workflow bypasses the auto-reply and instantly dispatches an alert to the system admin for immediate human intervention.
* **Standard Processing:** "Low" or "Medium" urgency emails proceed to the automated execution phase.

### 3. Execution & Observability

* **Audit Logging:** Integrates with the **Google Sheets API** to append a real-time record of every processed email, including the timestamp, sender, assigned category, urgency score, and a generated 1-sentence summary.
* **Automated Dispatch:** Utilizes the **Gmail API** to send the white-labeled, AI-generated professional response back to the original sender.

## 🛠️ Tech Stack

* **Orchestration:** n8n (Self-Hosted)
* **LLM / Inference:** Meta Llama 3.3 70B via Groq API (Low-latency processing)
* **Integrations:** Gmail API, Google Sheets API
* **Data Processing:** JavaScript, JSON

## ⚙️ How to Deploy Locally

1. Clone this repository.
2. Open your local n8n instance and click **Import from File**.
3. Select the `workflow.json` file.
4. Authenticate your Gmail and Google Sheets credentials within the respective nodes.
5. Generate a Groq API key and add it to the HTTP Request node under Header Authentication (`Authorization: Bearer YOUR_API_KEY`).
6. Adjust the Gmail trigger filters (e.g., specific labels or subjects) to fit your environment, and set the workflow to **Active**.
