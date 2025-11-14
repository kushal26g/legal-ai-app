# Indian Legal AI Helper

A **Streamlit-based, Retrieval-Augmented Generation (RAG)** system for **precise, explainable, and accessible answers** to queries grounded in **Indian law**. Built as a course project for **CS787: Introduction to Generative AI**.


## 🚀 Live Demo

👉 **Try the app here:**  
[https://legal-ai-app-njbxk5tsbhunjjjeib9lre.streamlit.app/](https://legal-ai-app-njbxk5tsbhunjjjeib9lre.streamlit.app/)


## ✨ Key Features

- **Contextual Legal Q&A**  
  - Uses a RAG pipeline: retrieves relevant legal documents first, then synthesizes answers via an LLM.
  - Answers are grounded in **Indian constitutional and statutory text**, not just model priors.

- **Source Attribution by Design**  
  - Every answer is accompanied by **explicit citations** to the underlying source documents (Articles, Sections, case law, etc.).
  - Shows the **source snippet + metadata** so users can verify everything themselves.

- **Curated Indian Legal Corpus**  
  - Current corpus includes:
    - 🇮🇳 **Constitution of India**  
    - **Indian Penal Code (IPC / BNS equivalent if updated)**  
    - **Code of Criminal Procedure (CrPC / BNSS equivalents as applicable)**  
    - **26,000+ Supreme Court judgments** (indexed for semantic retrieval)

- **User-Centric Interface (Non-lawyer Friendly)**  
  - Clean, minimal **Streamlit UI**: single text box for queries, clear answer + citations panel.
  - Designed for **students, developers, and non-lawyers** who just want clear, cited answers.


## 🧠 System Architecture (High-Level)

1. **User Query** → entered in Streamlit UI  
2. **Embedding & Retrieval**  
   - Query is embedded using: `sentence-transformers/all-MiniLM-L6-v2`  
   - Top-k similar chunks retrieved from a **FAISS** vector index (`main_legal_index.faiss`)
3. **RAG Answering**  
   - Retrieved chunks + user question are passed to **Google Gemini 2.5 Flash**  
   - Prompt instructs LLM to:
     - Ground answers in retrieved context  
     - Explicitly **cite document IDs / sections / cases**  
3. **UI Rendering**  
   - Answer, citations, and metadata (source type, title, section) are displayed in Streamlit.


## 🛠 Tech Stack

| Component      | Technology                                      |
|---------------|--------------------------------------------------|
| Web Framework | Streamlit                                       |
| LLM           | Google **Gemini 2.5 Flash**                      |
| Embeddings    | `sentence-transformers/all-MiniLM-L6-v2`         |
| Vector Store  | **FAISS** (Facebook AI Similarity Search)        |
| Language      | Python 3.10+                                     |
| Deployment    | Streamlit Community Cloud                        |

## 📦 Project Structure

```bash
.
├── 📄 AI_Legal_Rag.ipynb        # Colab notebook for index building & experimentation
├── 📄 app.py                    # Main Streamlit application
├── 📄 requirements.txt          # Python dependencies
├── 📄 README.md                 # Project documentation (this file)
│
├── 🗃️ ground_truth_100.json      # Evaluation dataset (Q–A pairs with gold references)
├── 🗃️ main_chunks.json          # Raw text chunks forming the knowledge base
├── 🗃️ main_chunk_metadata.json  # Metadata for each chunk (source, section, etc.)
└── 🗃️ main_legal_index.faiss    # Pre-built FAISS vector index (stored via Git LFS)
