# 🔍 Consent Evidence Auditor

An end-to-end AI system for automatically classifying customer consent documents using LangGraph, OpenAI, and Pinecone.

## 🎯 Features

- **Automated Classification**: Classifies documents into Opt-In, Opt-Out, Invalid, or Unclear
- **OCR Support**: Extracts text from scanned images using EasyOCR
- **RAG-based Context**: Retrieves relevant policy and historical context
- **Human Review**: Streamlit UI for reviewing low-confidence cases
- **REST API**: FastAPI server for integration
- **Audit Trail**: Complete JSONL logging for compliance

## 🚀 Quick Start

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Setup Environment
```bash
cp .env.example .env
# Edit .env with your API keys
```

### 3. Initialize Pinecone Index
```bash
python main.py --setup
```

### 4. Run the System

**Streamlit UI (Recommended)**:
```bash
python main.py --ui
```

**FastAPI Server**:
```bash
python main.py --server
```

**CLI Classification**:
```bash
python main.py --classify document.pdf
```

**Test Sample Documents**:
```bash
python main.py --test
```

## 📋 Classification Categories

- **Opt-In**: Explicit consent to receive marketing
- **Opt-Out**: Request to stop communications  
- **Invalid**: Unrelated to marketing consent
- **Unclear**: Ambiguous or contradictory content

## 🔧 API Usage

### POST /classify
Upload a document for classification:

```bash
curl -X POST "http://localhost:8000/classify" \
  -H "Content-Type: multipart/form-data" \
  -F "file=@document.pdf"
```

Response:
```json
{
  "case_id": "uuid-here",
  "filename": "document.pdf", 
  "classification": {
    "label": "Opt-Out",
    "confidence": 0.88,
    "rationale": "Customer explicitly requested removal",
    "citations": [...]
  }
}
```

## 📊 Audit Logging

All classifications are logged to `audit_log.jsonl`:

```json
{
  "case_id": "case_123",
  "timestamp": 1727640000,
  "label": "Opt-Out", 
  "confidence": 0.88,
  "rationale": "...",
  "doc_text": "..."
}
```

## 🏗️ Architecture

```
Document → OCR → Chunking → RAG Retrieval → LLM Classification → Audit Log
```

- **LangGraph**: Orchestrates the workflow
- **OpenAI**: Embeddings and GPT-4 classification
- **Pinecone**: Vector storage for RAG
- **EasyOCR**: Text extraction from images

## 🔒 Environment Variables

```bash
OPENAI_API_KEY=your_openai_key
PINECONE_API_KEY=your_pinecone_key  
PINECONE_INDEX_NAME=consent-auditor
```

## 📁 Project Structure

```
├── consent_auditor.py    # Core LangGraph workflow
├── server.py            # FastAPI REST API
├── streamlit_app.py     # Streamlit UI
├── setup_pinecone.py    # Index initialization
├── main.py             # CLI runner
├── requirements.txt    # Dependencies
└── audit_log.jsonl    # Classification logs
```

## 🎛️ Customization

- **Modify prompts** in `consent_auditor.py` → `decide_node()`
- **Add new document types** by extending OCR logic
- **Customize UI** in `streamlit_app.py`
- **Adjust confidence thresholds** for human review routing

## 📈 Monitoring

The Streamlit UI provides:
- Real-time classification metrics
- Human review queue for low-confidence cases
- Complete audit trail visualization
- Manual override capabilities

## 🔧 Troubleshooting

**OCR Issues**: Ensure images are clear and text is readable
**API Errors**: Check API keys and rate limits
**Pinecone Connection**: Verify index name and API key
**Low Accuracy**: Add more policy documents to improve context
