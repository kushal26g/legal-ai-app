# Indian Legal AI Helper 🇮🇳

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://your-app-url.streamlit.app) A contextual, Retrieval-Augmented Generation (RAG) system designed to provide precise, explainable, and accessible answers to queries grounded in Indian law. Built for **CS787: Intro to Generative AI**.

## Features

* **Contextual Q&A:** RAG pipeline retrieves legal docs first, then synthesizes answers via LLM.
* [cite_start]**Source Attribution:** Every answer includes direct citations to source documents[cite: 17].
* [cite_start]**Curated Corpus:** Includes the Constitution, IPC, CrPC, and 26,000+ Supreme Court judgments[cite: 15].
* [cite_start]**User-Centric UI:** Simple Streamlit interface for non-lawyers[cite: 39].

## Tech Stack

| Component | Technology Used |
| :--- | :--- |
| **Web Framework** | Streamlit |
| **LLM** | Google Gemini 2.5 Flash |
| **Embedding** | `sentence-transformers/all-MiniLM-L6-v2` |
| **Vector DB** | FAISS (Facebook AI Similarity Search) |

## How to Run

### Prerequisites
* Python 3.10+
* [Git LFS](https://git-lfs.github.com/) (Required for the large `.faiss` index)

### 1. Local Setup

1.  **Clone Repository (with LFS):**
    ```bash
    git lfs install
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    git lfs pull
    ```

2.  **Install Dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Set Up API Key:**
    Create the secrets directory and file:
    ```bash
    mkdir .streamlit
    ```
    Create `.streamlit/secrets.toml` and add your key:
    ```toml
    GOOGLE_API_KEY = "your_actual_api_key_here"
    ```

4.  **Run App:**
    ```bash
    streamlit run app.py
    ```

### 2. Deployment (Streamlit Cloud)

1.  Push to GitHub (ensure Git LFS files are pushed).
2.  Go to [Streamlit Community Cloud](https://share.streamlit.io/).
3.  Connect repo and deploy.
4.  **Important:** Add `GOOGLE_API_KEY` in **Settings > Secrets**.

## Project Structure
. ├── 📄 AI_Legal_Rag.ipynb # Colab notebook for index building ├── 📄 app.py # Main Streamlit app ├── 📄 requirements.txt # Dependencies ├── 📄 README.md # This file │ ├── 🗃️ ground_truth_100.json # Evaluation dataset ├── 🗃️ main_chunks.json # Raw text knowledge base ├── 🗃️ main_chunk_metadata.json # Source metadata └── 🗃️ main_legal_index.faiss # Pre-built Vector Index (LFS)
