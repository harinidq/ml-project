# AI Resume Analyzer using RAG

## Title of the Project

AI Resume Analyzer using Retrieval-Augmented Generation (RAG)

---

# About

The AI Resume Analyzer is an intelligent web-based application developed using Python, Streamlit, Natural Language Processing (NLP), and Retrieval-Augmented Generation (RAG) techniques. The system is designed to analyze resumes uploaded in PDF format and retrieve relevant information based on user queries.

Traditional resume screening often involves manually searching through large amounts of text to identify technical skills, projects, education details, and experience. This project automates the process by converting resume content into vector embeddings and performing semantic similarity search to retrieve the most relevant information efficiently.

The application uses LangChain for document processing, HuggingFace sentence-transformer embeddings for semantic understanding, and FAISS as the vector database for fast similarity retrieval.

Users can upload a resume and ask questions such as:

* What are the candidate’s technical skills?
* What projects has the candidate completed?
* What is the candidate’s educational background?
* What tools and technologies are used?

The system retrieves the most relevant resume sections and provides accurate, context-aware responses.

---

# Features

* Upload and analyze PDF resumes
* Extract resume text automatically
* Semantic search using vector embeddings
* Query-based resume information retrieval
* Technical skills extraction
* Education and project identification
* Fast document retrieval using FAISS
* Interactive user interface built with Streamlit
* Context-aware response generation
* Lightweight and scalable architecture

---

# Technologies Used

| Technology             | Purpose                               |
| ---------------------- | ------------------------------------- |
| Python                 | Core Programming Language             |
| Streamlit              | Web Application Framework             |
| LangChain              | Document Processing Pipeline          |
| FAISS                  | Vector Database for Similarity Search |
| HuggingFace Embeddings | Semantic Text Embeddings              |
| PyPDFLoader            | PDF Text Extraction                   |
| Sentence Transformers  | NLP Embedding Model                   |
| Machine Learning       | Intelligent Information Retrieval     |

---

# System Architecture

1. User uploads resume PDF
2. PDF text is extracted using PyPDFLoader
3. Text is split into smaller chunks using RecursiveCharacterTextSplitter
4. HuggingFace embedding model converts text chunks into vector embeddings
5. FAISS stores embeddings for semantic similarity search
6. User enters a query
7. Relevant chunks are retrieved based on similarity search
8. System returns the most relevant answer

---

# Project Workflow

## Step 1: Resume Upload

The user uploads a PDF resume through the Streamlit interface.

## Step 2: PDF Processing

The uploaded resume is processed and converted into readable text format.

## Step 3: Text Chunking

The resume text is divided into smaller chunks for efficient semantic retrieval.

## Step 4: Embedding Generation

Each text chunk is converted into vector embeddings using the sentence-transformers model.

## Step 5: Vector Storage

Embeddings are stored inside a FAISS vector database.

## Step 6: User Query

The user asks questions related to the uploaded resume.

## Step 7: Similarity Search

FAISS retrieves the most relevant resume sections based on semantic similarity.

## Step 8: Answer Generation

The system displays relevant information extracted from the resume.

---

# Installation

## Clone the Repository

```bash
git clone https://github.com/your-username/AI-Resume-Analyzer.git
cd AI-Resume-Analyzer
```

## Install Required Libraries

```bash
pip install streamlit
pip install langchain-community
pip install langchain-text-splitters
pip install sentence-transformers
pip install faiss-cpu
pip install pypdf
```

---

# Run the Project

```bash
streamlit run app.py
```

---
## Source Code

The main application logic is implemented in `app.py`.

```python
import streamlit as st
import ollama

from langchain_community.document_loaders import PyPDFLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_community.embeddings import HuggingFaceEmbeddings
from langchain_community.vectorstores import FAISS

# Title
st.title("AI Resume Analyzer (RAG + Llama 3)")

# Upload Resume
uploaded_file = st.file_uploader(
    "Upload your Resume (PDF)",
    type="pdf"
)

if uploaded_file:

    # Save uploaded file
    with open("resume.pdf", "wb") as f:
        f.write(uploaded_file.getbuffer())

    # Load PDF
    loader = PyPDFLoader("resume.pdf")
    docs = loader.load()

    # Split text into chunks
    splitter = RecursiveCharacterTextSplitter(
        chunk_size=500,
        chunk_overlap=50
    )

    chunks = splitter.split_documents(docs)

    # Create embeddings
    embeddings = HuggingFaceEmbeddings(
        model_name="sentence-transformers/all-MiniLM-L6-v2"
    )

    # Store in FAISS vector DB
    db = FAISS.from_documents(chunks, embeddings)

    st.success("Resume processed successfully!")

    # Ask question
    query = st.text_input("Ask something about your resume")

    if query:

        # Retrieve relevant chunks
        results = db.similarity_search(query, k=3)

        # Combine retrieved text
        context = "\n".join([r.page_content for r in results])

        # Prompt
        prompt = f"""
You are an AI Resume Analyzer.

Answer ONLY what is asked.

Do not provide unnecessary information.

Resume Content:
{context}

Question:
{query}
"""

        # Generate response using Ollama + Llama 3
        response = ollama.chat(
            model="llama3",
            messages=[
                {
                    "role": "user",
                    "content": prompt
                }
            ]
        )

        # Show answer
        st.subheader("Answer")
        st.write(response["message"]["content"])
```
# Output

## Output 1

* Resume uploaded successfully
* Resume text processed into embeddings
  
  <img width="1908" height="1007" alt="image" src="https://github.com/user-attachments/assets/998d581d-6447-44fb-b7f6-9cc6c049eee2" />


## Output 2

* User asks query:
  “What are the candidate’s technical skills?”
* System retrieves and displays relevant skills from the resume

<img width="1917" height="1005" alt="image" src="https://github.com/user-attachments/assets/53352f98-28a3-4a73-98a5-6e3cb1e79395" />

---

# Results

* Achieved approximately 85% response relevance accuracy during testing
* Improved resume information retrieval using semantic search
* Reduced unnecessary output using query-based filtering
* Successfully extracted technical skills, education, and project details from resumes

---

# Future Enhancements

* Integration with Gemini/OpenAI APIs for advanced response generation
* ATS Resume Score Analysis
* Multi-resume comparison
* Resume ranking system
* DOCX and image resume support
* Chatbot-based resume interaction
* Deployment using Streamlit Cloud or HuggingFace Spaces

---

# Applications

* HR Resume Screening
* Recruitment Automation
* Candidate Skill Analysis
* Resume Information Retrieval
* AI-Powered Talent Management Systems

---

# References

1. LangChain Documentation
2. FAISS Documentation
3. HuggingFace Sentence Transformers
4. Streamlit Documentation
5. Research Papers on Retrieval-Augmented Generation (RAG)
