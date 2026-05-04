# AI-Powered Legal Document System

## Overview

This project implements a multi-document assistant using hybrid retrieval techniques to answer questions based on a collection of PDF documents. The system leverages LangChain, Google Gemini, ChromaDB, and BM25 to provide a sophisticated legal document analysis and query interface.

## Features

- **Multi-Document Processing**: Load and process multiple PDF documents with automatic metadata tracking
- **Hybrid Retrieval**: Combines vector similarity search and BM25 lexical search for comprehensive document retrieval
- **Legal Assistant Interface**: AI-powered assistant trained to answer questions strictly based on contract content
- **Document Chunking**: Intelligent text splitting with configurable chunk size and overlap for context preservation
- **Source Attribution**: Query results include source documents and page numbers for verification
- **Batch Processing**: Efficient database creation with batching support for large datasets
- **Semantic Understanding**: Integration with Google Gemini embeddings and generative models

## Technical Stack

- **LangChain**: Framework for building LLM applications
- **ChromaDB**: Vector database for semantic search
- **Google Gemini**: Embedding generation and language model inference
- **BM25**: Ranking function for lexical-based document retrieval
- **PyPDF**: PDF document loading and parsing
- **Python**: Core programming language

## System Architecture

### 1. Document Loading and Preprocessing

- PDF documents are loaded using PyPDFLoader
- Each page is treated as a separate document with source metadata
- Documents are split into manageable chunks using RecursiveCharacterTextSplitter
- Configuration: 1200-character chunks with 100-character overlap

### 2. Hybrid Retrieval System

The system employs a dual-retrieval mechanism:

- **Vector-Based Search**: Uses Google Generative AI embeddings to understand semantic meaning and find conceptually similar content
- **Lexical-Based Search**: Utilizes BM25 algorithm for keyword-based relevance ranking
- **Combined Results**: Merges results from both methods, removes duplicates, and returns the most relevant documents

### 3. Language Model and Assistant

- Model: Google Gemini 2.5 Flash
- Temperature: 0 (deterministic responses)
- Role: Legal assistant specialized in contract analysis
- Output: Professional answers with identified clauses, obligations, and risks

## Installation

Install required dependencies:

```bash
pip install langchain langchain-community langchain-core chromadb pypdf rank_bm25 langchain-google-genai langchain-chroma
```

## Configuration

### API Key Setup

The system requires a Google Generative AI API key. Set this up in your environment:

```python
from google.colab import userdata
api_key = userdata.get('GEMINI_API_KEY')
```

### Document Paths

Configure PDF document paths in the document loading section:

```python
pdf_paths = [
    "/path/to/Legal-Services-Agreement.pdf",
    "/path/to/Contract_document.pdf"
]
```

### Vector Database

- Default persistence directory: `chroma_db`
- Batch size for database creation: 10 documents
- Database automatically loads existing instances or creates new ones

## Usage

### Query the Legal Assistant

```python
query = "What is the termination clause?"
answer, docs, vector_results, bm25_results = ask_legal_assistant(query)

print("Answer:", answer)
print("Sources:", docs)
```

### Retrieve Documents Using Hybrid Search

```python
docs, vector_results, bm25_results = hybrid_retrieve(query, k=3)
```

### Custom Prompts

Modify the prompt template in the Language Model and Assistant Setup section to customize assistant behavior:

```python
prompt = ChatPromptTemplate.from_template("""
You are a legal assistant.
...
""")
```

## Output

The assistant provides:

1. **Answer**: Response based on document context, following professional legal analysis standards
2. **Source Attribution**: Document name and page number for each relevant section
3. **Retrieval Insights**: Separate vector search and BM25 results for transparency and method comparison

## Performance Considerations

- **Chunk Size**: Larger chunks retain more context; smaller chunks improve precision
- **Overlap**: Prevents losing information at chunk boundaries
- **Batch Processing**: Essential for large document collections to manage memory usage
- **Database Persistence**: Eliminates need for re-embedding documents on subsequent runs

## Notes

- Questions are answered strictly based on provided document content
- The system indicates when information is not found in documents
- Vector and BM25 results are tracked separately for analysis and optimization
- Professional and precise language is maintained in all responses

## Future Enhancements

### Advanced Intent Routing System
Replace rule-based classification with lightweight LLM-based intent detection to dynamically route queries into QA, summary, comparison, and clause extraction modes.

### Multi-Document Cross-Reasoning
Extend the comparison engine to perform clause-level alignment across multiple documents with structured output highlighting differences, risks, and inconsistencies.

### Interactive UI Dashboard
Develop a Streamlit based interface with document upload, mode selection, and real-time visualization of retrieved chunks and sources.

### Citation and Traceability Improvements
Introduce page-level and chunk-level citation mapping with highlighted source references for improved transparency and auditability.

### Exportable Analysis Reports
Enable export of generated outputs into structured PDF or DOC reports for legal documentation and review workflows.

### Improved Retrieval Ranking Strategy
Enhance hybrid retrieval using BM25 and vector similarity score fusion, with optional LLM-based reranking for improved relevance without relying on paid external models.

### Multi-Format Document Support
Extend ingestion pipeline to support additional file formats such as DOCX and TXT using open-source document loaders.

### Lightweight Agentic Workflow
Introduce a modular reasoning pipeline where the system dynamically decides between retrieval, summarization, comparison, and extraction based on query intent.

### Free-Tier Deployment Strategy
Deploy the application using Streamlit Community Cloud or Hugging Face Spaces to maintain zero-cost hosting and public accessibility.