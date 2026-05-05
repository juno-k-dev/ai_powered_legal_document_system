# AI-Powered Legal Document System

## Overview

This project implements an AI-powered multi-document legal assistant using hybrid retrieval techniques and lightweight intent classification to dynamically route queries into specialized analysis modes. The system leverages LangChain, Google Gemini, ChromaDB, and BM25 to provide sophisticated legal document analysis and query interface based on a collection of PDF documents.

## Features

- **Multi-Document Processing**: Load and process multiple PDF documents with automatic metadata tracking
- **Hybrid Retrieval**: Combines vector similarity search and BM25 lexical search for comprehensive document retrieval
- **Legal Assistant Interface**: AI-powered assistant trained to answer questions strictly based on contract content
- **Document Chunking**: Intelligent text splitting with configurable chunk size and overlap for context preservation
- **Source Attribution**: Query results include source documents and page numbers for verification
- **Batch Processing**: Efficient database creation with batching support for large datasets
- **Semantic Understanding**: Integration with Google Gemini embeddings and generative models
- **Lightweight Intent Classifier**: Uses LLM-based intent detection using a Deterministic agent (rule-based) to dynamically route queries into specialized modes including general QA, termination clauses, definitions, obligations, payment terms, dispute resolution, and intellectual property rights

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

### Sample Queries by Intent

The system automatically classifies query intent and uses specialized pipelines:

- **General Inquiry**: "What is the general purpose of this legal document?"
- **Termination Clause Query**: "What are the conditions for contract termination?"
- **Definition Request**: "Define 'Institute'."
- **Obligation Identification**: "What are the responsibilities of the service provider?"
- **Payment Terms Inquiry**: "How are payments handled under this agreement?"
- **Dispute Resolution**: "How are disputes resolved in this contract?"
- **Intellectual Property Rights**: "Who owns the intellectual property created?"

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

## Implementation

The complete implementation and demonstration of the AI-powered legal document system, including the lightweight intent classifier with sample queries, is available in the `ai_powered_legal_document_system.ipynb` Jupyter notebook.

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

### Agentic Workflow
Upgrade Deterministic agent (rule-based) to a true agent.

### Free-Tier Deployment Strategy
Deploy the application using Streamlit Community Cloud or Hugging Face Spaces to maintain zero-cost hosting and public accessibility.
