# 🚀 Kotaemon - Full Stack Web Application with Local LLM Support

## Overview

**Kotaemon is already a complete full-stack web application** that provides a ChatGPT-like interface with local LLM support. This repository contains everything needed to run a privacy-focused RAG (Retrieval-Augmented Generation) system with local models.

![Kotaemon Interface](https://github.com/user-attachments/assets/7c040011-1d47-46a9-ada7-c8279693acd8)

## ✅ Features (Already Implemented)

- **🎯 ChatGPT-like Interface**: Modern, responsive web UI similar to ChatGPT
- **🏠 Local LLM Integration**: Full support for Ollama and other local models
- **💬 Conversation Management**: Save, load, and organize chat histories
- **📄 Document Upload & RAG**: Ask questions about uploaded documents
- **🌐 Web-based Full Stack App**: Complete FastAPI + Gradio application
- **🔒 Privacy-focused**: All processing happens locally on your machine

## 🏗️ Architecture

| Component | Implementation | Location |
|-----------|---------------|----------|
| **Frontend** | Gradio-based ChatGPT-like UI | `libs/ktem/ktem/pages/chat/` |
| **Backend** | FastAPI + Python | `app.py`, `sso_app.py` |
| **LLM Integration** | Ollama, OpenAI, Azure, etc. | `libs/ktem/ktem/llms/` |
| **Database** | SQLModel/SQLAlchemy | `libs/ktem/ktem/db/` |
| **Document Processing** | Multi-format support | `libs/kotaemon/kotaemon/indices/` |
| **RAG Pipeline** | Vector stores, retrievers | `libs/ktem/ktem/reasoning/` |

## 🚀 Quick Setup

### Prerequisites
- Python 3.10+
- Ollama (for local LLM support)

### 1. Install Ollama (Local LLM Backend)
```bash
# Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# Pull a model (choose one)
ollama pull qwen2.5:7b          # Recommended: Fast and capable
ollama pull llama3.2:3b         # Smaller, faster
ollama pull mistral:7b          # Alternative option
ollama pull phi3:3.8b          # Microsoft's model
```

### 2. Install Kotaemon Dependencies
```bash
# Clone the repository (if not already done)
git clone https://github.com/tadalavinay/kotaemon.git
cd kotaemon

# Install the packages in development mode
pip install -e libs/kotaemon
pip install -e libs/ktem

# Or install from requirements if available
pip install -r requirements.txt  # if exists
```

### 3. Configure Local LLM
The application is already configured for local LLMs in `flowsettings.py`:

```python
# Local LLM configuration (already in flowsettings.py)
if config("LOCAL_MODEL", default=""):
    KH_LLMS["ollama"] = {
        "spec": {
            "__type__": "kotaemon.llms.ChatOpenAI",
            "base_url": "http://localhost:11434/v1/",
            "model": config("LOCAL_MODEL", default="qwen2.5:7b"),
            "api_key": "ollama",
        },
        "default": False,
    }
```

### 4. Set Environment Variables
Create a `.env` file:
```bash
# Local model configuration
LOCAL_MODEL=qwen2.5:7b
LOCAL_MODEL_EMBEDDINGS=nomic-embed-text

# Optional: Enable first-time setup
KH_FIRST_SETUP=true
```

### 5. Run the Application
```bash
# Standard run
python app.py

# Or with SSO support
python sso_app.py

# The application will be available at http://localhost:7860
```

## 💡 Advanced Features

### Document Upload & RAG
- **Drag & drop**: Upload PDFs, Word docs, text files
- **Web URLs**: Paste URLs to ingest web content
- **Question answering**: Ask questions about your documents
- **Citation support**: Get references to source material

### Web Search Integration
- **@web command**: Search the internet from chat
- **Real-time results**: Get current information
- **Source attribution**: Links to original sources

### File References
- **@filename**: Reference specific uploaded files
- **Context switching**: Work with multiple documents
- **Selective retrieval**: Choose which documents to query

### Multiple LLM Support
- **Model switching**: Change models on the fly
- **Ollama integration**: Local models for privacy
- **Cloud options**: OpenAI, Azure, Google, Cohere
- **Custom endpoints**: Add your own model APIs

## 🔧 Customization

### Adding New Local Models
Edit `flowsettings.py` to add more Ollama models:

```python
KH_LLMS["my-custom-model"] = {
    "spec": {
        "__type__": "kotaemon.llms.ChatOpenAI",
        "base_url": "http://localhost:11434/v1/",
        "model": "my-custom-model:latest",
        "api_key": "ollama",
    },
    "default": False,
}
```

### UI Customization
The ChatGPT-like interface can be customized in:
- `libs/ktem/ktem/pages/chat/chat_panel.py` - Main chat interface
- `libs/ktem/ktem/pages/chat/__init__.py` - Chat page logic
- `libs/ktem/ktem/assets/` - CSS and static assets

## 🎯 Key Files

- **`app.py`** - Main application entry point
- **`flowsettings.py`** - Configuration for models and components
- **`libs/ktem/ktem/pages/chat/`** - ChatGPT-like interface implementation
- **`libs/ktem/ktem/llms/`** - LLM management and integration
- **`libs/kotaemon/kotaemon/`** - Core RAG and document processing

## 🐛 Troubleshooting

### Ollama Connection Issues
```bash
# Check if Ollama is running
ollama list

# Test model directly
ollama run qwen2.5:7b "Hello, how are you?"

# Check Ollama API
curl http://localhost:11434/api/tags
```

### Python Dependencies
```bash
# If imports fail, ensure packages are installed
pip install -e libs/kotaemon -e libs/ktem

# Install additional dependencies as needed
pip install langchain gradio fastapi sqlmodel plotly
```

### Port Conflicts
If port 7860 is busy, modify `app.py`:
```python
demo.launch(
    server_port=8080,  # Change to available port
    # ... other settings
)
```

## 🌟 What Makes This Special

1. **Complete Solution**: Not just a chat interface, but a full RAG system
2. **Privacy First**: All processing happens locally with Ollama
3. **ChatGPT-like UX**: Familiar interface with advanced features
4. **Production Ready**: Includes authentication, database, deployment options
5. **Extensible**: Modular architecture for easy customization

## 📖 Usage Examples

### Basic Chat
```
User: Hello! Tell me about artificial intelligence.
AI: [Response from your local Qwen2.5:7b model]
```

### Document Q&A
```
User: @my-document.pdf What are the main conclusions?
AI: [Analysis based on the uploaded PDF using RAG]
```

### Web Search
```
User: @web What's the latest news about AI?
AI: [Real-time search results with citations]
```

---

**Note**: This repository already contains a complete, production-ready full-stack web application with ChatGPT-like interface and local LLM support. The setup above gets you running with your own private, local AI assistant!