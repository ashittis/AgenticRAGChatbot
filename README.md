# Agentic RAG Chatbot with Multi-Format Support (MCP-based)

This project is an **agent-based Retrieval-Augmented Generation (RAG) chatbot** that allows users to upload documents of various formats and ask questions about their content. The architecture follows an **agentic design** and uses a **Model Context Protocol (MCP)** for message-based communication between agents.

## Features

- Upload documents in multiple formats: PDF, DOCX, PPTX, TXT, CSV, MD
    
- Semantic chunking and vector storage using FAISS
    
- Embedding via HuggingFace Sentence Transformers (`all-MiniLM-L6-v2`)
    
- Offline, free language model responses using Ollama (`gemma`, `mistral`, etc.)
    
- Modular architecture with three core agents:
    
    - IngestionAgent
        
    - RetrievalAgent
        
    - LLMResponseAgent
        
- Communication between agents via structured MCP messages
    
- Streamlit-based interactive UI
    

---

## Architecture Overview

```
User → UI (Streamlit)
        ↓
IngestionAgent → parses & chunks uploaded files
        ↓
RetrievalAgent → embeds chunks & stores in FAISS
        ↓
LLMResponseAgent → builds prompt + sends to Ollama LLM
        ↓
Streamlit → Displays response
```

**MCP Message Example:**

```json
{
  "type": "RETRIEVAL_RESULT",
  "sender": "RetrievalAgent",
  "receiver": "LLMResponseAgent",
  "trace_id": "rag-457",
  "payload": {
    "retrieved_context": ["slide 3: revenue up", "doc: Q1 summary..."],
    "query": "What KPIs were tracked in Q1?"
  }
}
```

---

## Tech Stack

|Component|Tool/Library|
|---|---|
|Language Model|Ollama (e.g., Gemma, Mistral)|
|Embeddings|HuggingFace Sentence Transformers|
|Vector DB|FAISS|
|UI|Streamlit|
|File Parsing|PyMuPDF, python-docx, python-pptx, pandas|
|Communication|Custom MCP (Model Context Protocol)|
|LLM Access|OpenAI-compatible API via Ollama|

---

## Installation

### Prerequisites

- Python 3.10+
    
- [Ollama](https://ollama.com/) installed and running locally
    
- A model pulled via Ollama, for example:
    
    ```
    ollama run gemma
    ```
    

### Install Python Dependencies

```bash
git clone https://github.com/your-username/agentic-rag-chatbot.git
cd agentic-rag-chatbot
python -m venv rag_env
source rag_env/bin/activate  # or rag_env\Scripts\activate on Windows

pip install -r requirements.txt
```

---

## Running the Application

Start your Ollama model:

```bash
ollama run gemma
```

Then launch the chatbot:

```bash
cd ui
streamlit run app.py
```

---

## Usage

1. Upload one or more documents in supported formats.
    
2. Wait for the system to parse and process the content.
    
3. Ask questions about the documents.
    
4. The answer will appear in the chat window, with "Thinking..." replaced upon response.
    

---

## Folder Structure

```
agentic-rag-chatbot/
├── agents/
│   ├── ingestion_agent.py
│   ├── retrieval_agent.py
│   └── llm_response_agent.py
├── core/
│   ├── mcp.py
│   └── embeddings.py
├── ui/
│   └── app.py
├── data/
├── README.md
```

---

## Challenges Faced

- Ensuring asynchronous behavior in Streamlit UI while maintaining stateful responses
    
- Handling multi-format document parsing reliably
    
- Maintaining model compatibility with Ollama's local inference API
    
- Ensuring UI updates dynamically (e.g., replacing "Thinking..." in real-time)
    

---

## Improvements & Suggestions

- Add support for multiple concurrent documents and chunk source attribution
    
- Stream results live using streaming APIs from Ollama-compatible backends
    
- Add a CoordinatorAgent for managing trace and logging
    
- Extend to support LangChain or LlamaIndex integration in future versions
    

---

