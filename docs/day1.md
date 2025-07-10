# Day 1 - Prompt Engineering & Local LLMs

<aside>
🔒

Link

- GuideJar - https://www.guidejar.com/guides/a040aa18-a29d-49a0-b127-e2e41d133a05#0c512ba6-fc76-4963-8c21-428193d62aaa
- 
</aside>

**Complete Summary: AI Generalist Accelerator Program – Day 1 (Parts 1 & 2)**

Day 1 of the AI Generalist Accelerator Program introduced foundational concepts of generative AI and guided participants through running local Large Language Models (LLMs). The session was structured in two parts: theoretical grounding and practical application.

---

### **Program Overview and Structure**

- The 14-day accelerator was outlined with a focus on AI tools, prompt engineering, voice agents, image/video manipulation, AI automation, and agent-based systems.
- Days 11–14 are dedicated to project-based work, culminating in group submissions of AI products.
- Mentors include Dilip KVS (Ph.D., IIT Madras) and Sagar Kalbande (IIT Kharagpur), both with extensive AI and product experience.

---

### **Key Learning Objectives**

1. Understand how LLMs work.
2. Gain hands-on experience running open-source LLMs locally.
3. Begin transitioning from AI tool users to AI problem solvers and thinkers.

---

### **Foundations of Generative AI and LLMs**

- **AI Models**: Mathematical systems trained on data, similar to how humans (e.g., babies) learn language through exposure.
- **Tokenization**: Splitting text into smaller units (tokens) for processing.
- **Embeddings**: High-dimensional vector representations of tokens that preserve semantic meaning.
- **Attention Mechanism**: Core to transformer models; enables context-aware predictions.
- **Prediction**: LLMs predict the next token based on input sequence and learned patterns.
- **Generative AI vs Traditional AI**: Generative AI produces visible outputs (e.g., text, images) while traditional AI often works in the background (e.g., recommendations).

---

### **AI Generalist Role**

- Not model developers but effective AI integrators.
- Use AI tools strategically across domains like marketing, education, healthcare, and operations.
- Understand when and how to apply different models and frameworks.

---

### **Retrieval-Augmented Generation (RAG)**

- Addresses LLM limitations with static knowledge.
- Uses a **vector database** to store embeddings of custom data (e.g., PDFs).
- Upon user query: relevant content is retrieved → augmented into the prompt → LLM generates a response.
- Key stages: **Retrieval → Augmentation → Generation**.

---

### **Running Local LLMs: Practical Session**

### **Why Run LLMs Locally?**

- Cost savings.
- Enhanced privacy and data security.
- Offline access and faster iteration.
- Independence from commercial APIs.

### **Tool Introduced: LM Studio**

- No-code interface for running local models.
- Compatible with Windows and macOS.
- Integrates with Hugging Face for model downloads.

### **Model Demonstrated**:

- **Mistral 7B Instruct v0.1 (GGUF, Q4_0)**: Chosen for balance of performance and resource efficiency.

### **Quantization and GGUF Format**:

- Reduces model size by lowering weight precision.
- Allows models to run on CPUs with limited RAM (8–16 GB).
- GGUF is the preferred format for local inference tools like LM Studio and Ollama.

### **Prompting Exercises**:

- Participants queried the model with both factual and creative prompts to evaluate local model performance.

---

### **Alternate Local Tools Mentioned**

- **Ollama**: CLI-based tool for local inference.
- **GPT4All**, **Text Generation Web UI**, and **Open WebUI** for further customization.

---

### **Conclusion and Day 2 Preview**

- Participants now understand both the theory behind LLMs and how to run them locally.
- Day 2 will focus on **voice agents** and real-world AI applications.
- Participants encouraged to explore LM Studio and test different open-source models in preparation.

---

This summary encapsulates all key concepts, activities, and learnings from Day 1 of the AI Generalist Accelerator Program.
