# Legal AI Helper 🇮🇳⚖️

A context-aware AI assistant for Indian law, powered by Retrieval-Augmented Generation (RAG) and Google's Gemini model. This application provides accurate, relevant, and cited answers to complex legal questions, making Indian legal information accessible to everyone.

**Built as a course project for CS787: Introduction to Generative AI.**

## 🚀 Live Demo

Try the application here:
👉 [https://legal-ai-app-njbxk5tsbhunjjjeib9lre.streamlit.app/](https://legal-ai-app-njbxk5tsbhunjjjeib9lre.streamlit.app/)

![Legal AI Helper Demo](images/ss.png)
*Screenshot: Legal AI Helper in action*

## 📖 Project Overview

This project is an advanced RAG (Retrieval-Augmented Generation) system designed to make Indian legal information more accessible to students, developers, and non-lawyers. When a user asks a question, the system retrieves relevant passages from a comprehensive knowledge base of Indian legal documents and uses a Large Language Model (LLM) to generate a precise answer based only on those retrieved sources.

The entire development process—from data collection to evaluation—is detailed in the `AI_Legal_Rag.ipynb` notebook. The final application is a user-friendly web app built with Streamlit (`app.py`).

## ✨ Key Features

### 🎯 Context-Aware Answers
The LLM's response is grounded in retrieved legal texts, minimizing hallucinations and ensuring accuracy.

### 📚 Source Citations by Design
Every answer includes explicit citations to the underlying source documents (Articles, Sections, case law, etc.), allowing for complete verification.

### 📊 Confidence Scoring
An internal confidence score is calculated based on the relevance of retrieved documents to help assess answer reliability.

### 📖 Comprehensive Knowledge Base
Built on a diverse corpus of Indian legal data:
- 🇮🇳 Constitution of India
- Indian Penal Code (IPC / BNS)
- Code of Criminal Procedure (CrPC / BNSS)
- 26,000+ Supreme Court judgments (indexed for semantic retrieval)
- Legal Q&A datasets and bare acts

### 💻 User-Centric Interface
Clean, minimal Streamlit UI designed for non-lawyers: single text box for queries, clear answer with citations panel.

## 🏗️ System Architecture

The project follows a standard RAG pipeline:

```
User Query → Embedding → FAISS Retrieval → Context Augmentation → LLM Generation → Cited Answer
```

### Detailed Workflow

#### Data Ingestion
A diverse corpus of Indian legal data was collected from sources like Hugging Face (ILC dataset), Kaggle (Supreme Court Judgments), and web URLs (IndicLegalQA, IPC Bare Act).

#### Data Processing
All text data, including from PDFs, was cleaned and segmented into smaller, overlapping "chunks" for precise retrieval.

#### Embedding & Indexing
- Each chunk is converted into a numerical vector embedding using `sentence-transformers/all-MiniLM-L6-v2`
- Vectors are stored in a FAISS index (`main_legal_index.faiss`) for rapid similarity search

#### RAG Pipeline (At Query Time)
- **Retrieve:** User query is embedded and FAISS index is searched for most semantically similar chunks
- **Augment:** Relevant chunks (with metadata from `main_chunk_metadata.json`) are compiled into a prompt
- **Generate:** Prompt is sent to the LLM, which generates a natural language answer grounded in the provided context

## 🛠️ Technology Stack

| Component | Technology |
|-----------|-----------|
| Web Framework | Streamlit |
| LLM | Google Gemini 2.5 Flash |
| Embeddings | sentence-transformers/all-MiniLM-L6-v2 |
| Vector Database | FAISS (Facebook AI Similarity Search) |
| Core Libraries | google-generativeai, faiss-cpu, sentence-transformers, numpy |
| Language | Python 3.10+ |
| Deployment | Streamlit Community Cloud |

## 🔄 A Note on LLMs

This project was developed and tested with two different Large Language Models:

- **Google Gemini 2.5 Flash:** Used in the final deployed Streamlit application (`app.py`) for fast and capable RAG responses.
- **Cohere Command-R-08-2024:** Used extensively in the development and evaluation notebook (`AI_Legal_Rag.ipynb`) for rigorous testing due to API rate limits on the free Gemini tier and to leverage Cohere's specialized RAG capabilities.

The notebook retains code for both models, allowing for easy comparison and adaptation.

## 📦 Project Structure

```
.
├── 📄 AI_Legal_Rag.ipynb          # Colab notebook for index building & experimentation
├── 📄 app.py                      # Main Streamlit application
├── 📄 requirements.txt            # Python dependencies
├── 📄 README.md                   # Project documentation (this file)
├── 📄 Project Report final        # Project report for final submission
│
├── 📁 images/
│   └── ss.png                     # Application screenshot
│
├── 🗃️ ground_truth_100.json       # Evaluation dataset (Q–A pairs with gold references)
├── 🗃️ main_chunks.json            # Raw text chunks forming the knowledge base
├── 🗃️ main_chunk_metadata.json    # Metadata for each chunk (source, section, etc.)
└── 🗃️ main_legal_index.faiss      # Pre-built FAISS vector index (stored via Git LFS)
```

## ⚙️ How to Run This Project

You can either run the Streamlit app directly (using the pre-built files) or re-generate the knowledge base yourself.

### A. Running the Streamlit App (Locally)

This method uses the pre-compiled knowledge base (`.faiss` and `.json` files) to run the web application.

#### 1. Clone the Repository

```bash
git clone [YOUR_REPOSITORY_URL]
cd legal-ai-app
```

#### 2. Install Dependencies

Ensure you have Python 3.10+ installed.

```bash
pip install -r requirements.txt
```

#### 3. Ensure Knowledge Base Files Are Present

Make sure the following files are in the root directory with `app.py`:
- `main_legal_index.faiss`
- `main_chunk_metadata.json`
- `main_chunks.json`

#### 4. Set Up API Key

Create a file at `.streamlit/secrets.toml` and add your Google API key:

```toml
# .streamlit/secrets.toml
GOOGLE_API_KEY = "YOUR_GOOGLE_AI_API_KEY_HERE"
```

#### 5. Run the App

```bash
streamlit run app.py
```

Your app will be available at `http://localhost:8501`.

### B. Re-generating the Knowledge Base (Using the Notebook)

Use this method if you want to modify the data sources or re-build the FAISS index from scratch.

#### 1. Open in Google Colab

Upload and open the `AI_Legal_Rag.ipynb` notebook in Google Colab. A GPU runtime is recommended for faster embedding.

#### 2. Set Up Secrets

- **Kaggle:** Upload your `kaggle.json` file when prompted by Cell 2 to download datasets.
- **Colab Secrets:** In the "Secrets" tab (🔑 icon) on the left, add your API keys:
  - `GOOGLE_API_KEY`: For the Gemini LLM
  - `COHERE_KEY`: For the Cohere LLM (used for evaluation)

#### 3. Run All Cells

Execute the notebook cells sequentially. The notebook will:
- Install all dependencies
- Download and analyze all source datasets (this may take time)
- Process, chunk, and embed all text data
- Test the RAG pipeline and run a full evaluation (uses Cohere API, takes 20-30 minutes due to rate limit pauses)

#### 4. Download Artifacts

After Cell 9 ("Create Embeddings") successfully runs, download these three files from the Colab file explorer:
- `main_legal_index.faiss`
- `main_chunk_metadata.json`
- `main_chunks.json`

You can now use these files to run the Streamlit app as described in **Part A**.

## 📊 Evaluation & Performance

The system was rigorously evaluated using a curated test set of 100 legal questions (`ground_truth_100.json`) with gold-standard answers.

### Final RAG Performance Report

✅ **Successfully processed and judged 99/100 questions**

#### 1️⃣ Generative Accuracy
- Semantic Similarity: **72.98%**
- F1 Score: **14.86%**
- Answer Relevancy: **85.15%**

#### 2️⃣ Trust & Grounding
- Faithfulness: **93.99%**
- Hallucination Rate: **1.01%**

#### 3️⃣ Retrieval Quality
- System Confidence: **99.85%**

The evaluation demonstrates that the system maintains high faithfulness to source material with minimal hallucinations, while providing highly relevant answers. The metrics focus on:
- Retrieval accuracy (precision of relevant documents)
- Answer quality (factual correctness and grounding)
- Citation accuracy (proper source attribution)

Detailed evaluation results and methodology are documented in `AI_Legal_Rag.ipynb`.

## 🤝 Contributing

Contributions are welcome! If you'd like to improve the system, add new data sources, or enhance the UI, please:

1. Fork the repository
2. Create a feature branch
3. Submit a pull request with a clear description of changes

## 📄 License

This project is created for educational purposes as part of CS787: Introduction to Generative AI.

## 👥 Authors

Devansh Verma (220346)\
Kushal Bansal (220575)\
Alankrit Gupta (220110)\
Karunaya Garg (220507)\
Kushal Gupta (220576)

## 🙏 Acknowledgments

- Hugging Face for the ILC legal dataset
- Kaggle for Supreme Court judgments dataset
- Google for Gemini API access
- Cohere for evaluation LLM access

---

<div align="center">
  <strong>Made with ⚖️ for accessible legal information</strong>
</div>
