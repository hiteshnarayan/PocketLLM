# 🤖 PocketLLM

> An intelligent chat application with local LLM integration, document-backed evidence retrieval, and session management

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)
[![Node](https://img.shields.io/badge/Node-20+-green.svg)](https://nodejs.org/)

**PocketLLM** is a full-stack AI chat application that runs entirely on your machine. It leverages Ollama for local LLM inference, MongoDB for persistent storage, and a React frontend for an intuitive user experience. Upload documents for evidence-backed responses, manage multiple chat sessions, and maintain complete control over your data.

---

## ✨ Key Features

### 🧠 **Intelligent Chat**
- **Local LLM Integration**: Powered by Ollama (Llama 2 7B Chat model)
- **Streaming Responses**: Real-time Server-Sent Events (SSE) for smooth interactions
- **Session Isolation**: Switch between chats without losing context
- **Abort Control**: Stop streaming responses mid-generation

### 📚 **Document Evidence System**
- **Upload Knowledge Base**: Add text files, markdown, or paste content directly
- **TF-IDF Search**: Smart relevance ranking using NLP (Porter Stemmer + stopwords)
- **Cited Sources**: See which documents informed each response
- **Drag & Drop**: Easy file uploads with visual feedback

### 💾 **Session Management**
- **Persistent History**: All conversations saved to MongoDB
- **Export Chats**: Download sessions as JSON
- **Auto-titling**: First message becomes session title
- **Rename/Delete**: Full control over your chat library

### 🎨 **Modern UI**
- **Responsive Design**: TailwindCSS with mobile-first approach
- **Skeleton Loaders**: Fast perceived performance
- **Relative Timestamps**: Human-readable time formatting
- **Collapsible Evidence**: Clean, distraction-free reading

### 🔐 **Security & Admin**
- **JWT Authentication**: Secure user sessions
- **Role-based Access**: Admin dashboard with metrics
- **Request Logging**: Track API usage and performance
- **Environment Secrets**: Secure credential management

---

## 🏗️ Architecture

### System Overview
```
┌─────────────┐      ┌──────────────┐      ┌─────────────┐
│   React     │ HTTP │   Express    │ HTTP │   Ollama    │
│  Frontend   │─────▶│   Backend    │─────▶│  LLM API    │
│  (Port 80)  │      │  (Port 5001) │      │ (Port 11434)│
└─────────────┘      └──────────────┘      └─────────────┘
                            │
                            │ Mongoose
                            ▼
                     ┌─────────────┐
                     │   MongoDB   │
                     │ (Port 27017)│
                     └─────────────┘
```

**See detailed diagrams:** [deliverables/](./deliverables/)
- Component Architecture (Descriptive & Prescriptive)
- Deployment Diagram
- Use Case Diagram
- Backend & Frontend Structure

---

## 🚀 Quick Start

### Prerequisites
- **Docker** & **Docker Compose** (recommended)
- **Node.js 20+** (for local development)
- 8GB+ RAM (for LLM inference)

### Option 1: Docker Compose (Recommended)

```bash
# Clone the repository
git clone https://github.com/kushalac/PocketLLM.git
cd PocketLLM

# Start all services
docker-compose up -d

# Wait for Ollama model download (~4GB)
docker logs -f pocketllm-ollama

# Access the app
open http://localhost
```

**Services Started:**
- Frontend: `http://localhost` (port 80)
- Backend API: `http://localhost:5001`
- Ollama: `http://localhost:11434`
- MongoDB: `mongodb://localhost:27017`

### Option 2: Local Development

```bash
# 1. Start MongoDB & Ollama separately
docker-compose up mongodb ollama -d

# 2. Install backend dependencies
cd backend
npm install
npm start

# 3. Install frontend dependencies (separate terminal)
cd frontend
npm install
npm start
```

---

## 📖 Usage Guide

### First-Time Setup

1. **Register Account**: Create your user account at `/register`
2. **Start Chat**: Click "New Chat" to begin a conversation
3. **Upload Documents** (Optional):
   - Click "Docs" in the header
   - Drag & drop `.txt` or `.md` files, or paste content
   - Documents will be cited when relevant

### Chat Interface

- **Send Message**: Type and press Enter (Shift+Enter for newline)
- **Stop Response**: Click "Stop" button during streaming
- **View Sources**: Expand evidence cards below assistant replies
- **Switch Sessions**: Click any chat in the sidebar (instant cache)

### Admin Dashboard

Access at `/admin` (requires admin privileges):
- **Metrics**: Request counts, response times, error rates
- **Logs**: View recent API activity
- **Health**: Check Ollama and MongoDB status

---

## 🛠️ Tech Stack

| Layer | Technologies |
|-------|-------------|
| **Frontend** | React 18, React Router v6, TailwindCSS, Axios |
| **Backend** | Node.js, Express, Mongoose, JWT, SSE |
| **Database** | MongoDB (Mongoose ODM) |
| **LLM** | Ollama (Llama 2 7B Chat) |
| **NLP** | Natural (TF-IDF, Porter Stemmer, Stopwords) |
| **DevOps** | Docker, Docker Compose, Nginx |

---

## 📁 Project Structure

```
PocketLLM/
├── frontend/              # React application
│   ├── src/
│   │   ├── features/      # Chat, History, Admin pages
│   │   ├── components/    # Reusable UI components
│   │   ├── core/          # API services
│   │   └── App.jsx
│   ├── Dockerfile
│   └── nginx.conf
│
├── backend/               # Express API server
│   ├── controllers/       # Route handlers
│   ├── models/            # Mongoose schemas
│   ├── services/          # Business logic
│   │   ├── ChatService.js
│   │   ├── DocumentService.js  # TF-IDF search
│   │   └── LLMService.js       # Ollama integration
│   ├── routes/            # API endpoints
│   ├── middleware/        # Auth, logging
│   ├── server.js
│   └── Dockerfile
│
├── deliverables/          # Architecture & documentation
│   ├── *.puml             # PlantUML diagrams
│   ├── *.png              # Rendered diagrams
│   └── report.pdf         # Full project report
│
├── docker-compose.yml     # Multi-container setup
├── .env.example           # Environment template
└── README.md
```

---

## 🔧 Configuration

### Environment Variables

Create `.env` files in both `frontend/` and `backend/`:

**Backend (`backend/.env`):**
```env
PORT=5001
MONGODB_URI=mongodb://mongodb:27017/pocketllm
OLLAMA_BASE_URL=http://ollama:11434
JWT_SECRET=your-secret-key-here
ADMIN_USERNAME=admin
ADMIN_PASSWORD=securepassword
```

**Frontend (`frontend/.env`):**
```env
REACT_APP_API_BASE_URL=http://localhost:5001
```

### MongoDB Setup

**Option A: MongoDB Atlas (Cloud)**
1. Create cluster at [mongodb.com/atlas](https://www.mongodb.com/cloud/atlas)
2. Get connection string
3. Update `MONGODB_URI` in `backend/.env`

**Option B: Local MongoDB (Docker)**
- Already configured in `docker-compose.yml`
- Default: `mongodb://root:example@mongodb:27017/pocketllm?authSource=admin`

---

## 📊 API Endpoints

### Authentication
- `POST /api/auth/register` - Create user account
- `POST /api/auth/login` - Get JWT token

### Chat
- `GET /api/chat/sessions` - List all sessions
- `POST /api/chat/session` - Start new chat
- `POST /api/chat/message` - Send message (SSE response)
- `GET /api/chat/messages/:sessionId` - Get chat history

### Documents
- `GET /api/chat/documents` - List uploaded docs
- `POST /api/chat/documents` - Upload new document
- `DELETE /api/chat/documents/:id` - Remove document

### Admin
- `GET /api/admin/metrics` - System metrics
- `GET /api/admin/logs` - Recent API logs
- `GET /api/admin/health` - Service health checks

---

## 🧪 Testing

```bash
# Backend tests
cd backend
npm test

# Run with coverage
npm run test:coverage
```

---

## 🎥 Demo

**Video Walkthrough**: See `Pocket-LLM.mp4` in the repository root

**Screenshots**:

| Feature | Preview |
|---------|---------|
| Chat Interface | ![Chat](deliverables/pocketllm_frontend_descriptive.png) |
| Document Upload | Evidence-backed responses with citations |
| Session History | Searchable chat library with timestamps |

---

## 🤝 Contributing

Contributions welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit with descriptive messages (`git commit -m 'feat: add amazing feature'`)
4. Push to your branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

**Commit Convention:**
- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation updates
- `refactor:` Code refactoring
- `test:` Test additions/modifications
- `chore:` Maintenance tasks

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2025 PocketLLM Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🙏 Acknowledgments

- **Ollama Team** for local LLM inference capabilities
- **MongoDB** for robust document storage
- **Natural NLP** for text processing utilities
- **React Community** for excellent frontend tooling

---

## 📬 Contact

**Project Link**: [https://github.com/kushalac/PocketLLM](https://github.com/kushalac/PocketLLM)

**Contributors**:
- Hitesh Narayan - [@hiteshnarayan](https://github.com/hiteshnarayan)
- Kushal - [@kushalac](https://github.com/kushalac)

---

## 🗺️ Roadmap

- [ ] IndexedDB caching for offline-first experience
- [ ] Multi-model support (GPT-4, Claude, etc.)
- [ ] Voice input/output
- [ ] Code syntax highlighting in responses
- [ ] Bulk document operations
- [ ] Mobile app (React Native)
- [ ] RAG pipeline improvements

---

**Built with ❤️ for privacy-conscious AI interactions**
