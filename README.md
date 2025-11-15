# RAG Backend API

A production-ready Retrieval-Augmented Generation (RAG) backend system built with FastAPI, featuring document ingestion, conversational AI, and intelligent booking management.

## Features

- **Document Management**: Upload and process PDF/TXT files with smart chunking strategies
- **Vector Search**: Qdrant-powered semantic search with sentence-transformers embeddings
- **Conversational AI**: RAG-based Q&A with conversation history and context awareness
- **Booking System**: NLP-powered booking extraction and management
- **Production Ready**: Comprehensive error handling, logging, and validation

## Tech Stack

- **Framework**: FastAPI
- **Database**: SQLite (metadata), Qdrant (vectors)
- **LLM**: Groq (llama-3.1-8b-instant)
- **Embeddings**: sentence-transformers (all-MiniLM-L6-v2)
- **Text Processing**: LangChain, pdfplumber

## Quick Start

### Prerequisites

- Python 3.12+
- Qdrant (local or cloud)
- Groq API key

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd rag-backend-project
```

2. Create virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Configure environment:
```bash
cp .env.example .env
# Edit .env and add your GROQ_API_KEY
```

5. Start Qdrant (if using local):
```bash
docker run -p 6333:6333 qdrant/qdrant
```

6. Run the application:
```bash
python run.py
```

The API will be available at `http://localhost:8000`

## API Documentation

Once running, visit:
- **Interactive API Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

## API Endpoints

### Document Ingestion
- `POST /api/v1/ingest` - Upload and process documents
- `GET /api/v1/documents` - List all documents
- `GET /api/v1/documents/{id}` - Get document details
- `DELETE /api/v1/documents/{id}` - Delete document
- `GET /api/v1/documents/{id}/chunks` - Get document chunks

### Conversation
- `POST /api/v1/chat` - Ask questions with RAG
- `GET /api/v1/chat/history/{session_id}` - Get conversation history
- `DELETE /api/v1/chat/history/{session_id}` - Clear conversation
- `GET /api/v1/chat/sessions` - List all sessions

### Booking
- `POST /api/v1/booking` - Create booking from natural language
- `GET /api/v1/booking` - List bookings
- `GET /api/v1/booking/{id}` - Get booking details
- `PATCH /api/v1/booking/{id}` - Update booking status
- `DELETE /api/v1/booking/{id}` - Delete booking
- `GET /api/v1/booking/session/{session_id}` - Get session bookings

## Usage Examples

### Upload a Document
```bash
curl -X POST "http://localhost:8000/api/v1/ingest" \
  -F "file=@document.pdf" \
  -F "chunking_strategy=recursive"
```

### Ask a Question
```bash
curl -X POST "http://localhost:8000/api/v1/chat" \
  -H "Content-Type: application/json" \
  -d '{
    "question": "What is the main topic of the document?",
    "session_id": "user123",
    "document_ids": ["doc-uuid"]
  }'
```

### Create a Booking
```bash
curl -X POST "http://localhost:8000/api/v1/booking" \
  -H "Content-Type: application/json" \
  -d '{
    "message": "Book for John Doe on 2024-12-25 at 14:00, email john@example.com",
    "session_id": "user123"
  }'
```

## Project Structure

```
rag-backend-project/
├── app/
│   ├── api/v1/          # API endpoints
│   ├── core/            # Configuration & dependencies
│   ├── database/        # Database models & CRUD
│   ├── models/          # Pydantic schemas
│   ├── services/        # Business logic
│   └── utils/           # Utilities
├── storage/             # Data storage
├── logs/                # Application logs
├── tests/               # Test files
└── run.py              # Entry point
```

## Configuration

Key environment variables in `.env`:

```bash
GROQ_API_KEY=your_api_key
QDRANT_URL=http://localhost:6333
CHUNK_SIZE=500
CHUNK_OVERLAP=50
MAX_FILE_SIZE_MB=10
```

## Development

Run tests:
```bash
pytest
```

Format code:
```bash
black app/
```

Lint code:
```bash
flake8 app/
```

## Error Handling

The API includes comprehensive error handling:
- Custom exceptions for different error types
- Structured error responses
- Detailed logging

## Logging

Logs are stored in `logs/` directory:
- `app.log` - General application logs
- `api.log` - API requests/responses
- `errors.log` - Error logs only

## License

MIT License

## Contributing

Contributions welcome! Please read contributing guidelines first.

## Support

For issues and questions, please open a GitHub issue.