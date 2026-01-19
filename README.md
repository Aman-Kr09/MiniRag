# Mini RAG Application

A full-stack RAG (Retrieval-Augmented Generation) application built with **FastAPI** (Python) and **React** (Vite).

## Features

- **Ingestion**: Upload text files or paste text directly. 
- **Chunking**: Recursive character chunking (1000 tokens, 150 overlap).
- **Embeddings**: Uses OpenAI (`text-embedding-3-small`) or Google Gemini (`text-embedding-004`).
- **Vector DB**: Pinecone (Serverless) for storage and retrieval.
- **Reranking**: Optional Reranking step using Cohere (`rerank-english-v3.0`).
- **LLM**: Answer generation with citations using OpenAI (`gpt-4o-mini`) or Gemini Flash.
- **Frontend**: Premium, responsive UI with dark mode and glassmorphism.

## Architecture

1. **Frontend**: React + Vite + Framer Motion (Animations) + Lucide (Icons).
2. **Backend**: FastAPI.
3. **Storage**: Pinecone (Vector Search).

## Quick Start (Unified)

We have created an all-in-one script to build and run the application.

1. **Install Dependencies**
   ```bash
   # Root
   pip install -r backend/requirements.txt
   
   # Frontend
   cd frontend
   npm install
   cd ..
   ```

2. **Run Application**
   ```bash
   python app.py
   ```
   This command will:
   - Automatically build the React frontend (if not already built).
   - Start the FastAPI backend serving both the API and the UI at `http://localhost:8000`.

---

## Deployment on Render

This project is configured for easy deployment on **Render** as a single Web Service.

### Option 1: Blueprints (Recommended)
1. Fork/Push this repository to your GitHub.
2. Log in to [Render](https://render.com).
3. Click "New" -> "Blueprint".
4. Connect your repository.
5. Render will detect the `render.yaml` file and configure the service automatically.
6. **Important**: You will be prompted to enter your API Keys (`GEMINI_API_KEY`, `PINECONE_API_KEY`, etc.) during the setup wizard.
7. Click **Apply**.

### Option 2: Manual Web Service
1. Create a new **Web Service** explicitly connecting your repo.
2. **Build Command**: `./render_build.sh`
3. **Start Command**: `python app.py`
4. **Environment Variables**: Add your keys manually in the Environment tab.

---

### Previous Manual Setup (Legacy)
If you prefer to run them separately:

**1. Backend**
```bash
cd backend
python -m venv venv
# Activate venv...
pip install -r requirements.txt
python main.py
```

**2. Frontend**
```bash
cd frontend
npm install
npm run dev
```

## Trade-offs & Remarks
- **Ingestion Speed**: Currently runs synchronously; for larger files, a background task queue (Celery/BullMQ) would be better.
- **Duplicate Handling**: Basic unique ID generation; robust deduplication is not implemented.
- **Security**: Basic CORS allows all origins (*). In production, restrict this to the frontend domain.
