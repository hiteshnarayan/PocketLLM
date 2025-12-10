# PocketLLM

A full-stack AI chat application with local LLM integration, document evidence retrieval, and multi-session support. Runs entirely on your machine using Ollama for inference, MongoDB for persistence, and React for the user interface.

## Features

- **Local LLM**: Ollama integration with Llama 2 7B Chat model
- **Document Evidence**: Upload documents and receive cited responses with TF-IDF relevance ranking
- **Streaming Responses**: Real-time message generation via Server-Sent Events
- **Session Management**: Multiple concurrent chat sessions with persistent history
- **Responsive Design**: Mobile-friendly interface with TailwindCSS
- **Admin Dashboard**: System metrics, logs, and health monitoring
- **Authentication**: JWT-based user authentication with role-based access

## Quick Start

### Prerequisites

- Docker and Docker Compose
- Node.js 20+ (for local development)
- 8GB RAM (for LLM inference)

### Docker Compose (Recommended)

```bash
git clone https://github.com/kushalac/PocketLLM.git
cd PocketLLM
docker-compose up -d
```

Wait for Ollama model download (~4GB), then access the app at `http://localhost`

**Services:**
- Frontend: http://localhost (port 80)
- Backend API: http://localhost:5001
- MongoDB: localhost:27017
- Ollama: localhost:11434

### Local Development

```bash
# Start services
docker-compose up mongodb ollama -d

# Backend
cd backend
npm install
npm start

# Frontend (new terminal)
cd frontend
npm install
npm start
```

## Usage

1. Register an account at `/register`
2. Create a new chat session
3. Upload documents (optional) via the "Docs" button
4. Send messages - responses will cite relevant documents
5. Switch between sessions in the sidebar
6. Access admin metrics at `/admin`

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 18, React Router, TailwindCSS, Axios |
| Backend | Node.js, Express, Mongoose, JWT |
| Database | MongoDB |
| LLM | Ollama (Llama 2 7B Chat) |
| NLP | Natural (TF-IDF, Porter Stemmer) |
| DevOps | Docker, Docker Compose |

## Configuration

### Environment Variables

**Backend** (`backend/.env`):
```
PORT=5001
MONGODB_URI=mongodb://mongodb:27017/pocketllm
OLLAMA_BASE_URL=http://ollama:11434
JWT_SECRET=your-secret-key
ADMIN_USERNAME=admin
ADMIN_PASSWORD=securepassword
```

**Frontend** (`frontend/.env`):
```
REACT_APP_API_BASE_URL=http://localhost:5001
```

## API Endpoints

**Authentication**
- `POST /api/auth/register` - Create account
- `POST /api/auth/login` - Get JWT token

**Chat**
- `GET /api/chat/sessions` - List sessions
- `POST /api/chat/session` - Create session
- `POST /api/chat/message` - Send message (SSE)
- `GET /api/chat/messages/:sessionId` - Get history

**Documents**
- `GET /api/chat/documents` - List documents
- `POST /api/chat/documents` - Upload document
- `DELETE /api/chat/documents/:id` - Delete document

**Admin**
- `GET /api/admin/metrics` - System metrics
- `GET /api/admin/logs` - API logs
- `GET /api/admin/health` - Service health

## Testing

```bash
cd backend
npm test
npm run test:coverage
```

## Project Structure

```
PocketLLM/
├── frontend/           # React app
│   ├── src/
│   │   ├── features/   # Pages (Chat, History, Admin)
│   │   ├── components/ # Reusable components
│   │   └── core/       # API services
│   ├── Dockerfile
│   └── nginx.conf
├── backend/            # Express server
│   ├── controllers/    # Route handlers
│   ├── models/         # Mongoose schemas
│   ├── services/       # Business logic
│   │   ├── ChatService.js
│   │   ├── DocumentService.js  # TF-IDF search
│   │   └── LLMService.js
│   ├── routes/         # API endpoints
│   └── Dockerfile
├── deliverables/       # Architecture diagrams & reports
└── docker-compose.yml
```

## Documentation

- **Demo**: See [DEMO.md](DEMO.md) for video walkthrough and feature overview
- **Setup Guide**: Check [SETUP.md](SETUP.md) for detailed installation instructions
- **Architecture**: View diagrams in [deliverables/](./deliverables/) directory
- **Full Report**: See [deliverables/report.pdf](./deliverables/report.pdf)

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit changes: `git commit -m 'feat: add my feature'`
4. Push to branch: `git push origin feature/my-feature`
5. Open a pull request

Commit conventions:
- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation
- `refactor:` Code refactoring
- `test:` Tests
- `chore:` Maintenance

## License

MIT License - see [LICENSE](LICENSE) for details.

Copyright (c) 2025 PocketLLM Contributors

## Support

- GitHub Issues: [Report bugs or request features](https://github.com/kushalac/PocketLLM/issues)
- Documentation: [Full project report](./deliverables/report.pdf)

## Roadmap

- Offline-first support with IndexedDB caching
- Multi-model support (GPT-4, Claude, Mistral)
- Voice input/output
- Code syntax highlighting
- Bulk document operations
- Mobile app (React Native)
- Advanced RAG pipeline
