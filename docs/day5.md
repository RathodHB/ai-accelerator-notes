# Day 5 - n8n

<aside>
📍

**Index**

</aside>

# Session Overview

The fifth day of the AI Accelerator Program focused on practical, hands-on automation using **n8n**, a self-hostable workflow automation platform. This session aimed to enable participants to build intelligent, flexible AI workflows and introduce the concept of **AI agents**. Below is a comprehensive summary for those who were unable to attend.

---

### **1. Session Kick-off & Interaction Norms**

- Emphasis was placed on camera use for engagement and interaction through the chat.
- The mentor encouraged an interactive and paced learning style to suit the audience.

---

### **2. Introduction to n8n**

- **n8n (pronounced "native")** is a workflow automation tool similar to Zapier and Make, but with **granular control** and **self-hosting** capabilities.
- It allows users to build custom workflows with over **2,900 templates** and integrates with tools like Google Sheets, Discord, Telegram, Jira, and more.

---

### **3. Core Concepts Explained**

- **Trigger**: Starts the workflow (e.g., a chat message).
- **Action**: Performs a task based on trigger input (e.g., appending a row to Google Sheets).
- **Nodes**: Visual blocks that represent triggers or actions in the workflow.
- **Expression Editor**: Used for dynamic input manipulation (e.g., splitting strings or using JSON paths).

---

### **4. Use Case Walkthrough – Expense Tracker**

A simple workflow was built to demonstrate:

- Capturing a user’s expense input through a chat node.
- Parsing and mapping input (e.g., “debit, shopping, 5000”) into structured fields.
- Appending the parsed data to a Google Sheet.
- Utilizing **dynamic expressions** (e.g., `{{ $json["text"].split(",")[0] }}`) for parsing.

---

### **5. Limitations of Traditional Automation**

- Rigid: Any input format variation can break the flow.
- No intelligence: Manual field mapping is required.
- Cannot scale well to complex or ambiguous inputs.

---

### **6. Introducing AI with OpenAI Node**

- Replaced manual parsing with an **OpenAI model (GPT 3.5/4)** to extract structured data from natural language inputs.
- Users learned how to:
    - Configure API access with OpenAI.
    - Write effective prompts (e.g., asking GPT to return JSON-formatted outputs).
    - Connect model outputs directly to Google Sheets dynamically.

---

### **7. Challenges Encountered**

- Handling running totals required memory of past transactions, which LLM nodes do not retain.
- Prompt engineering was needed to improve output accuracy (e.g., ensuring proper categorization and total calculation).
- Issues with model selection and API limits were addressed.

---

### **8. Transition to AI Agents**

- **AI Agents** were introduced to solve limitations around memory and context.
- Differences from standard LLM nodes:
    - Agents retain **memory** (can recall previous inputs).
    - Support **tool usage**, **multiple model types**, and **contextual reasoning**.
    - Configurable **context window** to handle sequences of inputs and build persistent logic (e.g., cumulative totals).

---

### **9. Agent-Based Automation**

- Demonstrated how to:
    - Insert AI Agent nodes into existing workflows.
    - Assign memory types and configure agents with models like GPT-4, Claude, etc.
    - Enable agents to maintain running totals by remembering prior inputs and computing deltas.

---

### **10. Conclusion & Next Steps**

- Participants were encouraged to experiment with:
    - Replacing chat triggers with real-world inputs (e.g., Telegram or Slack).
    - Creating more robust automations using templates and agents.
    - Continuing to optimize prompts and model usage for complex workflows.

---

### **Key Takeaways**

- **n8n** empowers users to build custom automation with complete data control.
- Integrating **LLMs** enhances flexibility but requires strong prompt engineering.
- **AI Agents** provide contextual memory and tool use, making them vital for building intelligent, real-world applications.

# Demonstrations

## ✅ **Demonstration 1: Simple Expense Tracker Using n8n**

### Goal: Capture structured expense data and store it in Google Sheets.

**Steps:**

1. **Create a new workflow** in n8n and name it (e.g., “Expense Tracker”).
2. **Add a “Chat Trigger” Node**:
    - Simulate a chat input like `debit, shopping, 5000`.
3. **Add a “Google Sheets: Append Row” Node**:
    - Authenticate with your Google account.
    - Select your spreadsheet and sheet name (e.g., “Sheet1”).
    - Define columns: Credit/Debit, Type of Expense, Amount.
4. **Use Expression Editor** to dynamically parse the input:
    - Example:
        
        ```jsx
        {{$json["text"].split(",")[0]}} // Credit/Debit
        {{$json["text"].split(",")[1]}} // Type of Expense
        {{$json["text"].split(",")[2]}} // Amount
        
        ```
        
5. **Save and execute** the workflow.
6. **Test with various chat messages** and observe data being logged in the sheet.

---

## ✅ **Demonstration 2: Adding OpenAI LLM to Extract Data**

### Goal: Use GPT to intelligently parse natural language input and extract structured values.

**Steps:**

1. **Insert OpenAI Node between Chat Trigger and Google Sheets Node**.
2. **Create/Open an account at** `https://platform.openai.com`.
3. **Generate and copy API key** from OpenAI.
4. **Paste the API key in the OpenAI credentials** prompt in n8n.
5. **Configure the OpenAI node**:
    - Model: GPT-3.5-turbo or GPT-4.
    - Prompt:
        
        ```
        You are an expert assistant in expense tracking.
        Extract transaction type (credit or debit), expense type, and amount from this input:
        {{$json["text"]}}
        Return output as JSON.
        
        ```
        
    - Enable: Output as JSON.
6. **Update Google Sheets Node to read from OpenAI output**:
    - Drag and drop values from the OpenAI output (e.g., `{{$json["transaction_type"]}}`).
7. **Save and execute** the workflow.
8. **Test with varied natural inputs** (e.g., “I received my bonus of 10,000”).

---

## ✅ **Demonstration 3: Introducing Running Totals**

### Goal: Maintain a running total in Google Sheets using LLM logic.

**Steps:**

1. **Add a “Total” column** in the Google Sheet.
2. **Update OpenAI prompt** to calculate and return a running total:
    
    ```
    Maintain a running total. Add for credit, subtract for debit. Return total in JSON.
    
    ```
    
3. **Modify the output format** to include a “total” field.
4. **Update Google Sheets node**:
    - Map the “total” field from the OpenAI response to the Total column.
5. **Test multiple sequential entries** and observe that the total is **not accumulating**.
6. **Identify limitation**: LLM does not remember previous runs (no memory).

---

## ✅ **Demonstration 4: Using AI Agent for Context & Memory**

### Goal: Use an AI Agent node to retain memory across transactions.

**Steps:**

1. **Deactivate the OpenAI LLM node** (don’t delete).
2. **Insert “AI Agent” Node between Chat Trigger and Google Sheets**.
3. **In Agent Configuration**:
    - Chat Model: OpenAI GPT-4 or Claude.
    - Memory Type: Simple Memory.
    - Context Window: Set to 5 (to remember last 5 messages).
4. **In System Message (Prompt)**:
    
    ```
    You are an intelligent expense tracker.
    Extract credit/debit, expense type, amount.
    Maintain a running total from previous inputs.
    Return all in JSON.
    
    ```
    
5. **Map AI Agent output to Google Sheets columns**, including total.
6. **Test with multiple entries** (credit and debit).
7. **Observe**: Agent uses memory to compute correct totals dynamically.

---

## ✅ **Demonstration 5: Expanding Triggers Beyond Chat**

### Goal: Replace Chat Trigger with real-world input like Telegram or Slack.

**Steps:**

1. **Replace Chat Trigger Node** with a new app-based trigger:
    - e.g., Telegram Trigger, Slack Event Trigger.
2. **Authenticate the app integration**.
3. **Map the new input** to the AI Agent node as the first message.
4. **Retain rest of the workflow logic** (AI Agent → Google Sheets).
5. **Test by sending messages from the actual app** (e.g., Telegram).
6. **Observe**: Inputs from the app trigger the same workflow seamlessly.
