# Day 6 - RAG

<aside>
📍

**TABLE OF CONTENT**

</aside>

# Session Summary

For those unable to attend Day 5 of the AI Accelerator Program, the session focused on building and deploying AI-powered agents for customer support using Retrieval-Augmented Generation (RAG) and automation via n8n. Below is a detailed summary structured for clarity and professional reference

### 1. **Session Objective**

The session aimed to implement a practical AI use case for automating customer support workflows. This involved:

- Understanding how to identify support queries.
- Filtering customer support emails.
- Creating an AI agent capable of reading, understanding, and responding to customer queries.

### 2. **Use Case: Customer Support Automation**

The use case revolved around a direct-to-consumer (D2C) brand (e.g., Sleepy Owl Coffee) receiving high volumes of customer queries via email. The goal was to automate handling these queries using AI agents.

**Workflow Steps:**

1. **Email Ingestion** – Use Gmail trigger in n8n to monitor new emails.
2. **Filtering** – An LLM determines whether the email qualifies as a customer support query.
3. **AI Agent Architecture**:
    - **Brain**: The LLM (e.g., GPT-4) serves as the agent’s reasoning engine.
    - **Skills (Tools)**: Email drafting, label reading, integration with apps like Jira, Google Sheets, or CRMs.
    - **Memory**: To handle context and follow-up interactions over time.
    - **System Prompt**: Defines how the agent behaves, including its tools and rules of engagement.

### 3. **RAG (Retrieval-Augmented Generation) Integration**

RAG was implemented to provide the agent with knowledge about company-specific documents such as:

- Refund and return policies
- FAQs and standard operating procedures

These documents were stored in a vector database. The AI agent retrieved relevant snippets based on user queries to craft accurate, context-aware responses.

### 4. **Tooling via n8n**

- n8n was the no-code/low-code platform used to orchestrate the automation.
- The workflow included Gmail triggers, OpenAI nodes for classification and response generation, and Slack notifications for human-in-the-loop approvals.
- Responses were saved as drafts and optionally notified to human agents on Slack, enabling a human review before sending.

### 5. **Human-in-the-Loop Design**

The session emphasized keeping a quality check by:

- Saving AI-generated responses as email drafts.
- Notifying the team via Slack for review.
- Transitioning to full automation only after building internal confidence in the system's quality.

### 6. **Best Practices Discussed**

- Start with a human-in-the-loop to ensure output quality.
- Use modular workflows (e.g., agent per function) for scalability.
- Leverage LLM reasoning for context-rich responses.
- Maintain versioned prompts and memory structures to improve accuracy over time.

### 7. **Key Takeaways**

- AI can handle up to 85–90% of support queries when paired with structured workflows and relevant context.
- A hybrid approach of automation + human review offers the best balance between speed and reliability.
- Designing the system with extensibility (e.g., integrating Slack, CRMs, Google Sheets) significantly increases business value.

# Demonstration

## 🔧 **Demonstration 1: AI-Powered Customer Support Agent**

### Objective: Automatically identify and respond to customer support emails using LLM and RAG.

### **Steps Demonstrated:**

1. **Trigger Setup (Gmail)**
    - Add a **Gmail Trigger** node in n8n.
    - Configure to run on **"Message Received"**.
    - Set polling interval (e.g., every 1 minute).
    - Connect Gmail credentials via OAuth.
2. **Test Email Injection**
    - Send a sample customer query email (e.g., “I want to return the product. What is your refund policy?”).
3. **Data Simplification**
    - Enable **“Simplify”** on the Gmail node to reduce output payload.
4. **Pin Test Data**
    - Use “Pin Data” in n8n for iterative testing during development.
5. **Field Extraction**
    - Add **Edit Fields** node to extract:
        - `email_body` → from `snippet`
        - `sender_email` → from `from`
        - `thread_id` → from `threadId`
6. **Email Classification (Is it Support?)**
    - Add **OpenAI “Message a Model”** node.
    - Use a **system prompt** like:
        
        > “You are an assistant. Classify the email content and determine whether it’s a customer support query.”
        > 
    - Use `email_body` as the user input.
7. **Conditional Branching**
    - Based on LLM output (Yes/No), route flow accordingly using **IF node** or similar.
8. **AI Agent Construction**
    - Add a custom **AI Agent** node (n8n).
    - Define:
        - **LLM** as brain (e.g., GPT-4)
        - **RAG tool** to access company policy database
        - **Memory** (to preserve conversational history)
        - **System Prompt** to guide behavior (e.g., answer using refund/return policies)
9. **Attach Tools to Agent**
    - RAG (retrieves policy documents from a vector DB)
    - Email Draft Generator
10. **Drafting Email Response**
    - Use **Gmail Node** → “Send Message” with `sendAsDraft: true`
    - Provide `threadId`, `sender`, and `reply message`.
11. **Slack Notification (Optional)**
    - Notify via Slack: “A customer support draft is ready.”
12. **Autonomy Toggle**
    - Initially save as draft + notify.
    - Later toggle to auto-send once trust in agent is established.

## 🤖 **Demonstration 2: Modular Multi-Agent Virtual Assistant**

### Objective: Build a personal/enterprise assistant named **Jerry** with distinct capabilities (email, calendar, meetings, expenses).

### **Modules Demonstrated:**

### A. **Orchestration Setup**

1. **Start with Chat Trigger**
    - Use **Chat Message Trigger** node for interactive agent commands.
2. **Create Master Agent**
    - Use **AI Agent** node to act as an orchestrator.
    - Add **System Prompt**:
        
        > “If the command is related to email, route to Email Assistant. If it’s related to calendar, route to Calendar Assistant…”
        > 
3. **Add Memory** to Master Agent
    - To preserve conversation context across agents.

### B. **Email Assistant Module**

1. **Agent Setup**
    - New **AI Agent** for email tasks.
2. **Tools Added**
    - **Gmail: Get Many Messages**
    - **Gmail: Send Message**
3. **Sample Commands**
    - “Read my important emails.”
    - “Send an email to [X] with this content…”

### C. **Calendar Assistant Module**

1. **Agent Setup**
    - New **AI Agent** for calendar tasks.
2. **Tools Added**
    - **Google Calendar: Get Many Events**
    - **Google Calendar: Create Event**
3. **Dynamic Inputs**
    - Use LLM to determine `startTime`, `endTime`, or event details based on natural language command.
4. **Sample Commands**
    - “Check my available slots this week.”
    - “Schedule a meeting with [Name] tomorrow at 10am.”

### D. **Meetings Assistant (Fireflies Integration)**

1. **Custom Tool Integration**
    - Used **HTTP Request Node** to connect with **Fireflies API**.
2. **API Key Setup**
    - Logged into Fireflies → Generated API Key from integrations page.
3. **Actions Demonstrated**
    - Read meeting transcripts.
    - Fetch summaries or notes via API.
4. **API Basics Recap**
    - GET: Retrieve data (e.g., get transcript)
    - POST: Create records (e.g., upload audio)
    - PUT/PATCH: Update
    - DELETE: Delete resources

## 📌 Best Practices Demonstrated

- **Memory Usage**
    - Use in multi-turn interactions for follow-ups (e.g., "Send a reply to the last email").
- **Modular Architecture**
    - Separate agents for each domain (email, calendar, etc.) for better maintainability and scale.
- **Human-in-the-Loop**
    - Maintain human approvals in early deployments; automate fully when confidence is built.
- **Prompt Engineering**
    - Each agent had a well-defined system prompt for accurate instruction and task execution.
