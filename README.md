# 🧪 RAG A/B Testing Framework with Local LLMs

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)
![Streamlit](https://img.shields.io/badge/Streamlit-App-red)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-pgvector-336791)
![Ollama](https://img.shields.io/badge/Ollama-Local%20LLM-white)
![Docker](https://img.shields.io/badge/Docker-Container-2496ED)

## 🚀 Overview
This project is a **Full-Stack Retrieval-Augmented Generation (RAG) platform** designed to scientifically test different retrieval strategies. 

Unlike standard RAG chatbots, this system implements a live **A/B Testing Framework** to compare user satisfaction between different context window sizes (e.g., Single-Chunk vs. Multi-Chunk retrieval). It runs entirely offline using **Local LLMs (Llama-3/Phi-3)** and a containerized **PostgreSQL (pgvector)** database, ensuring 100% data privacy and zero inference costs.

## 🏗 Architecture
The system follows a microservices-inspired architecture:
* **Frontend:** Streamlit (Chat Interface & Analytics Dashboard).
* **Backend:** Python (LangChain, Psycopg2).
* **Database:** Dockerized PostgreSQL with `pgvector` extension for semantic search.
* **Inference Engine:** Ollama running Llama-3 locally.

### Data Flow
1.  **Ingestion:** PDFs are parsed, chunked, embedded (HuggingFace MiniLM), and stored in Postgres.
2.  **Experiment Routing:** Users are hashed into consistent groups (Control vs. Variant) using a Sticky Session algorithm.
3.  **Retrieval:** * *Control Group:* Retrieves top-1 semantic match.
    * *Variant Group:* Retrieves top-3 matches to test "Context richness."
4.  **Generation:** Local LLM generates a response based on the retrieved context.
5.  **Analytics:** User feedback (👍/👎) and latency metrics are logged for statistical analysis.

## 🛠️ Tech Stack
* **Language:** Python 3.11
* **LLM Orchestration:** LangChain
* **Vector Database:** PostgreSQL (`pgvector`)
* **Local Inference:** Ollama
* **Visualization:** Plotly & Streamlit
* **Containerization:** Docker Desktop

## ⚙️ Setup & Installation

### 1. Prerequisites
* [Docker Desktop](https://www.docker.com/) installed and running.
* [Ollama](https://ollama.com/) installed.
* Python 3.10 or higher.

### 2. Initialize the "Brain" (Ollama)
Pull the local model (Llama-3 or Phi-3):
```bash
ollama run llama3
# Wait for the chat prompt, then type /bye to exit. The server remains active.
```
### **3. Start the Database**
Run the PostgreSQL container with vector support:

docker run -d \
  --name rag_db \
  -e POSTGRES_USER=admin \
  -e POSTGRES_PASSWORD=password123 \
  -e POSTGRES_DB=rag_experiment_db \
  -p 5432:5432 \
  pgvector/pgvector:pg16

### 4. Install Dependencies
pip install -r requirements.txt

### 5. Run the Application

#### Terminal 1: The Chatbot
streamlit run app.py

#### Terminal 2: The Analytics Dashboard
streamlit run dashboard.py

📊 Analytics Dashboard
The dashboard.py application provides real-time insights into the experiment:

Win Rate: Percentage of positive feedback (👍) per group.

Latency Tracking: Average inference time (ms) comparison.

Volume: Total queries processed per model version.
