Worked for 14s

I analyzed the GitHub repository and the folder structure you shared. The current implementation uses FastAPI + FAISS + Sentence Transformers + Ollama/Llama 3.2 on the backend, with a lightweight HTML/CSS/JavaScript frontend. The RAG flow is: document upload → PDF/TXT extraction → text cleaning/chunking → embeddings → FAISS similarity search → context construction → Llama 3.2 response. 
G
GitHub
+3

One important point: the GitHub repository currently shows the files at the repository root, while your local structure separates them into Backend/ and Frontend/. The README below follows your intended folder structure, which is cleaner and more professional. 
G
GitHub

🤖 AI-Powered RAG Document Assistant
<p align="center"> <strong>Chat with your documents using Retrieval-Augmented Generation, semantic search, and a local LLM.</strong> </p> <p align="center"> <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python"> <img src="https://img.shields.io/badge/FastAPI-Backend-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI"> <img src="https://img.shields.io/badge/FAISS-Vector%20Search-0467DF?style=for-the-badge" alt="FAISS"> <img src="https://img.shields.io/badge/Ollama-Llama%203.2-black?style=for-the-badge&logo=ollama&logoColor=white" alt="Ollama"> <img src="https://img.shields.io/badge/RAG-AI%20Pipeline-8A2BE2?style=for-the-badge" alt="RAG"> </p> <p align="center"> <a href="https://github.com/anishkesh05/AI-Powered-RAG-Document-Assistant"> <img src="https://img.shields.io/badge/GitHub-Repository-181717?style=for-the-badge&logo=github" alt="GitHub Repository"> </a> </p>
📌 Overview

AI-Powered RAG Document Assistant is a locally running AI application that allows users to upload PDF or TXT documents and ask questions about their content using natural language.

Instead of sending the entire document directly to an AI model, the application uses a Retrieval-Augmented Generation (RAG) pipeline to find the most relevant parts of the uploaded document and provide them as context to a local Llama 3.2 model running through Ollama.

This approach helps keep responses focused on the uploaded document and reduces the risk of generating information that is not present in the source material.

✨ Features

📄 PDF & TXT Upload

Upload PDF or plain-text documents directly from the web interface.

🧹 Automatic Text Extraction

Extracts readable text from uploaded PDF and TXT files.

✂️ Intelligent Text Chunking

Documents are divided into overlapping chunks for better retrieval.
Default chunk size: 800 characters
Chunk overlap: 150 characters

🧠 Semantic Embeddings

Uses all-MiniLM-L6-v2 from Sentence Transformers to convert text into numerical vectors.

🔎 FAISS Vector Search

Uses FAISS with normalized embeddings and inner-product similarity for fast semantic retrieval.

🎯 Top-K Retrieval

Retrieves the most relevant document chunks for each question.

🤖 Local Llama 3.2 LLM

Uses Ollama to run Llama 3.2 locally.
No external AI API is required for answer generation.

🛡️ Context-Grounded Answers

The prompt instructs the model to answer only from the retrieved document context.

🚫 Hallucination Guard

If relevant information cannot be found, the application returns:

Information was not found in the uploaded document.

⚡ FastAPI REST API

Clean API endpoints for document upload and question answering.

🎨 Simple Responsive Frontend

Built using HTML, CSS, and vanilla JavaScript.
🧠 How RAG Works in This Project

The application follows this pipeline:

                    ┌──────────────────┐
                    │   Upload PDF/TXT │
                    └────────┬─────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │   Text Extraction   │
                  │   PDF / TXT Parser  │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │   Text Cleaning     │
                  │  Remove extra space │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │  Text Chunking      │
                  │  800 / 150 overlap  │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │ Sentence Transformer│
                  │ all-MiniLM-L6-v2    │
                  └──────────┬──────────┘
                             │
                             ▼
                  ┌─────────────────────┐
                  │   FAISS Vector DB   │
                  └──────────┬──────────┘
                             │
                             │
              ┌──────────────┘
              │ User Question
              ▼
       ┌─────────────────────┐
       │ Question Embedding  │
       └──────────┬──────────┘
                  │
                  ▼
       ┌─────────────────────┐
       │ Semantic Similarity  │
       │     Search           │
       └──────────┬──────────┘
                  │
                  ▼
       ┌─────────────────────┐
       │ Top 4 Relevant       │
       │ Document Chunks      │
       └──────────┬──────────┘
                  │
                  ▼
       ┌─────────────────────┐
       │ Context + Question   │
       │      Prompt          │
       └──────────┬──────────┘
                  │
                  ▼
       ┌─────────────────────┐
       │ Ollama + Llama 3.2  │
       └──────────┬──────────┘
                  │
                  ▼
       ┌─────────────────────┐
       │ Grounded AI Answer   │
       └─────────────────────┘

🔍 RAG Pipeline Explained
1. Document Upload

The user uploads either:

.pdf
.txt

The FastAPI /upload endpoint receives the file and stores it inside the uploads/ directory.

2. Text Extraction

For PDF files, the application uses PyPDF2 to extract text page by page.

TXT files are read directly using UTF-8 encoding.

3. Text Cleaning

Unnecessary whitespace is normalized before processing.

4. Chunking

The extracted text is divided into smaller overlapping pieces:

Chunk Size: 800 characters
Overlap:    150 characters


The overlap helps preserve context between neighboring chunks.

5. Embeddings

Each chunk is converted into an embedding using:

all-MiniLM-L6-v2


The embeddings are normalized before being inserted into FAISS.

6. Vector Search

When a user asks a question, the question is also converted into an embedding.

FAISS searches for the most semantically similar document chunks.

The application retrieves the top 4 matching chunks.

7. Context Construction

The retrieved chunks are combined into a context block.

The model receives:

Context
+
User Question

8. LLM Generation

The context is passed to:

Ollama
    ↓
Llama 3.2


The model is explicitly instructed to use only the provided context.

🏗️ Project Architecture
AI-Powered-RAG-Document-Assistant/
│
├── Backend/
│   ├── __pycache__/
│   ├── uploads/
│   ├── main.py
│   └── rag.py
│
├── Frontend/
│   ├── index.html
│   ├── script.js
│   └── style.css
│
└── uploads/

Backend
File / Folder	Purpose
main.py	FastAPI application, file upload, question answering and Ollama integration
rag.py	Text processing, chunking, embeddings, FAISS indexing and similarity search
uploads/	Stores uploaded documents
__pycache__/	Python-generated cache files
Frontend
File	Purpose
index.html	User interface
script.js	Upload and question-answer API communication
style.css	Application styling and layout
🛠️ Tech Stack
Backend
🐍 Python
⚡ FastAPI
📄 PyPDF2
🧠 Sentence Transformers
🔎 FAISS
🤖 Ollama
🦙 Llama 3.2
📦 Pydantic
🌐 Requests
Frontend
HTML5
CSS3
JavaScript
Fetch API
AI / RAG
Document Processing
        ↓
Sentence Transformers
        ↓
FAISS
        ↓
Semantic Retrieval
        ↓
Ollama
        ↓
Llama 3.2

🚀 Getting Started
Prerequisites

Make sure the following are installed:

Python 3.10 or higher
Ollama
Llama 3.2 model
A modern web browser
1️⃣ Clone the Repository
git clone https://github.com/anishkesh05/AI-Powered-RAG-Document-Assistant.git

cd AI-Powered-RAG-Document-Assistant

2️⃣ Create a Virtual Environment
Windows
python -m venv venv

venv\Scripts\activate

macOS / Linux
python3 -m venv venv

source venv/bin/activate

3️⃣ Install Python Dependencies

Install the packages required by the current backend:

pip install fastapi uvicorn python-multipart pydantic requests PyPDF2 sentence-transformers faiss-cpu numpy


Recommended: add these dependencies to a Backend/requirements.txt file so the project can be installed with a single command.

Example:

pip install -r Backend/requirements.txt

🤖 Set Up Ollama

Install Ollama and make sure it is running.

Then download the Llama 3.2 model:

ollama pull llama3.2


Start Ollama if it is not already running:

ollama serve


The application expects Ollama at:

http://localhost:11434


The backend sends requests to the Ollama generation API using the llama3.2 model.

▶️ Run the Backend

Navigate to the backend directory:

cd Backend


Start FastAPI with Uvicorn:

uvicorn main:app --reload


The backend will be available at:

http://127.0.0.1:8000


You can also check the API documentation at:

http://127.0.0.1:8000/docs

🌐 Run the Frontend

Open the frontend directory:

Frontend/


Then open:

index.html


in your browser.

The frontend communicates with the FastAPI backend at:

http://127.0.0.1:8000

🔌 API Endpoints
GET /

Checks whether the API is running.

Response
{
  "message": "RAG Document Assistant API is running."
}

POST /upload

Uploads and processes a PDF or TXT document.

Request
multipart/form-data
file=<document>

Supported Formats
.pdf
.txt

Example Response
{
  "message": "Document uploaded successfully.",
  "filename": "example.pdf",
  "chunks": 25
}

POST /ask

Ask a question about the uploaded document.

Request
{
  "question": "What is this document about?"
}

Example Response
{
  "answer": "The document discusses...",
  "sources": [
    {
      "score": 0.72,
      "text": "Relevant document content..."
    }
  ]
}

🖥️ Application Workflow
Step 1 — Upload

Select a PDF or TXT document.

📄 Select Document
        ↓
⬆️ Upload
        ↓
Text Extraction

Step 2 — Processing

The application:

Clean Text
    ↓
Split Text
    ↓
Generate Embeddings
    ↓
Create FAISS Index

Step 3 — Ask

Enter a natural-language question.

"What are the main objectives?"

Step 4 — Retrieval

The system finds the most relevant document chunks.

Question
   ↓
Embedding
   ↓
FAISS Search
   ↓
Top 4 Chunks

Step 5 — Answer

The retrieved context is sent to Llama 3.2 through Ollama.

Retrieved Context
       +
    Question
       ↓
   Llama 3.2
       ↓
  Final Answer

🎯 Example Use Cases

This project can be useful for:

📚 Research paper analysis
📄 Personal document Q&A
📝 Study material analysis
💼 Business document search
📖 Book and report exploration
🧑‍🎓 Student learning assistants
📑 Technical documentation Q&A
🔍 Knowledge-base exploration
🔐 Privacy-Friendly Architecture

One of the major advantages of this project is the local AI architecture.

              Your Computer
┌─────────────────────────────────────┐
│                                     │
│  📄 Documents                       │
│       ↓                             │
│  🧠 Sentence Transformer            │
│       ↓                             │
│  🔎 FAISS                          │
│       ↓                             │
│  🤖 Ollama + Llama 3.2             │
│       ↓                             │
│  💬 Answer                          │
│                                     │
└─────────────────────────────────────┘


The application is designed around local document processing and local LLM inference rather than requiring a hosted LLM API.

Note: Local execution does not automatically guarantee complete security. Production deployments should additionally address file validation, authentication, authorization, CORS restrictions, resource limits, and secure storage.

⚙️ Configuration

The frontend currently points to:

const API_URL = "http://127.0.0.1:8000";


If your backend is running on another host or port, update this value in:

Frontend/script.js


For example:

const API_URL = "http://localhost:8000";

📊 Current RAG Configuration
Component	Current Configuration
Document formats	PDF, TXT
Embedding model	all-MiniLM-L6-v2
Chunk size	800 characters
Chunk overlap	150 characters
Vector database	FAISS
Similarity method	Inner Product / Cosine-style normalized similarity
Retrieved chunks	Top 4
Similarity threshold	0.25
LLM	Llama 3.2
LLM runtime	Ollama
Backend	FastAPI
Frontend	HTML + CSS + JavaScript
🧪 Example Questions

After uploading a document, try questions such as:

What is this document about?

What are the main objectives mentioned in the document?

Summarize the key points.

Who are the main people mentioned?

What conclusions are presented?

What problem does the document attempt to solve?


The assistant is instructed to avoid using outside knowledge when answering document-based questions.

⚠️ Current Limitations

The current implementation is intentionally lightweight and suitable for learning, experimentation, and portfolio demonstration.

Some current limitations include:

Only PDF and TXT files are supported.
FAISS data is maintained in memory.
Uploaded documents are stored locally.
The vector index is recreated when a new document is processed.
There is no user authentication.
There is no persistent database.
There is no multi-user document isolation.
The frontend uses a hard-coded backend URL.
There is no streaming response from the LLM.
There is no advanced reranking stage.
Scanned/image-only PDFs may not produce useful text without OCR.
The current CORS configuration is permissive and should be restricted for production.
🚀 Future Improvements

Possible improvements for the next version:

 Add DOCX support
 Add Markdown support
 Add CSV and Excel document support
 Add OCR for scanned PDFs
 Persist FAISS indexes to disk
 Support multiple documents simultaneously
 Add document deletion and management
 Add chat history
 Add source citations with page numbers
 Add streaming LLM responses
 Add authentication and user accounts
 Add persistent database storage
 Add hybrid keyword + vector search
 Add reranking
 Add configurable embedding models
 Add configurable LLM models
 Add Docker support
 Add automated tests
 Add CI/CD
 Improve frontend UX with drag-and-drop uploads
 Add production-ready CORS configuration
 Add file-size and upload security limits
🧩 Why This Project Is Interesting

Traditional document search depends heavily on exact keyword matching.

RAG changes the interaction model:

Traditional Search
       ↓
Find matching words
       ↓
Read document yourself


RAG Document Assistant
       ↓
Understand question
       ↓
Find semantically relevant content
       ↓
Retrieve context
       ↓
Generate natural-language answer


This makes large documents much easier to explore using natural language.

📚 Core Concepts Demonstrated

This project is a practical implementation of several important AI engineering concepts:

Retrieval-Augmented Generation
Semantic Search
Vector Embeddings
Vector Databases
Similarity Search
Document Chunking
Context Retrieval
Prompt Engineering
Local LLM Inference
REST API Development
Frontend–Backend Integration
🤝 Contributing

Contributions, ideas, improvements, and bug reports are welcome.

Contribution Workflow
# Fork the repository

# Clone your fork
git clone <your-fork-url>

# Create a branch
git checkout -b feature/my-feature

# Make your changes

# Commit
git commit -m "Add my feature"

# Push
git push origin feature/my-feature


Then open a Pull Request.

⭐ Support

If you find this project useful:

⭐ Star the repository
🍴 Fork the project
🐛 Report bugs
💡 Suggest improvements
🔧 Submit Pull Requests
👨‍💻 Author

Anish Kesh

GitHub:
https://github.com/anishkesh05

Project:
https://github.com/anishkesh05/AI-Powered-RAG-Document-Assistant

📄 License

This project is open-source. Add a LICENSE file to the repository to explicitly define the license under which others may use, modify, and distribute the project.

<p align="center"> <strong>🤖 AI-Powered RAG Document Assistant</strong> <br> <sub>Upload your documents. Ask questions. Get grounded answers.</sub> </p> <p align="center"> Made with ❤️ using Python, FastAPI, FAISS, Sentence Transformers & Ollama </p> :::
G
Sources
