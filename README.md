# Python Final Project — MCP File Search + LLM Backend

This repository contains **Python_Final_Project**, integrating an MCP (Model Context Protocol) server for multi-backend file storage (Google Drive + Dropbox) with an LLM backend for intelligent natural-language query handling, persistent chat history, and user authentication.


## Overview

This project enables seamless interaction between:

- A **custom MCP server** that connects to Google Drive and Dropbox  
- An **LLM backend** that interprets natural-language requests and converts them into MCP actions  
- A **web-based GUI** for querying, storing, and retrieving files  
- A **PostgreSQL database** for user accounts and chat history persistence  

Users can search, upload, and retrieve files using natural language, while the LLM translates intent into structured operations executed by the MCP server. All user interactions and conversations are stored securely in the database for session continuity.


## Problem Statement & Significance

Managing files across multiple cloud providers is fragmented and inefficient. Each platform has its own interface, search mechanisms, and APIs. This project addresses that problem by:

- Providing a **single, unified interface** for multiple storage providers  
- Allowing **natural-language interaction** instead of manual navigation  
- Persisting **chat context and user sessions** using a database  
- Demonstrating how LLMs can orchestrate real backend systems  

This approach significantly improves usability and serves as a foundation for scalable, AI-assisted file management systems.


## Key Features

### MCP Server (Google Drive + Dropbox)

- Unified API for listing, searching, uploading, and retrieving files  
- Supports both Google Drive and Dropbox as storage backends  
- Backend-agnostic design (`backend="google"` or `backend="dropbox"`)  
- Modular structure allowing future providers (OneDrive, S3, Box)

### LLM Backend

- Converts natural-language prompts into structured MCP tool calls  
- Uses a configurable **XML-based system prompt**  
- Maintains **conversation context** across sessions  
- Integrates directly with PostgreSQL for persistence  

### Web Interface (GUI)

- Lightweight HTML + CSS + JavaScript frontend  
- Text-based chat interface for file queries  
- Displays LLM responses and file operation results  
- Designed for live demos and quick testing  

### Database & Authentication (PostgreSQL)

- Stores **user accounts** (login & signup)  
- Persists **chat history** per user  
- Enables session-based conversation continuity  
- Automatically initializes required tables on backend startup  

### Secure Credential Handling

- `.env` files for sensitive variables  
- OAuth credentials for Google Drive  
- Token-based authentication for Dropbox  
- Database credentials stored securely via environment variables  

---

## Project Structure
```
.
├── llm_backend/
│   ├── database/
│   │   ├── database.py
│   │   ├── auth.py
│   │   └── auth_routes.py
│   ├── prompts/
│   │   └── system_prompt.xml
│   ├── static/
│   │   ├── profile-pic.png
│   │   ├── script.js
│   │   └── style.css
│   ├── templates/
│   │   ├── index.html
│   │   ├── login.html
│   │   └── signup.html
│   ├── venv/
│   ├── main.py
│   ├── mcp_client.py
│   ├── README.md
│   └── requirements.txt
│
├── mcp_server/
│   ├── venv/
│   ├── credentials.json
│   ├── drive_utils.py
│   ├── dropbox_utils.py
│   ├── server.py
│   ├── tool_functions.py
│   ├── token.pickle
│   ├── README.md
│   └── requirements.txt
│
├── .gitignore
└── README.md
```

---

## Setup Guide

### Clone the Repository
```bash
git clone https://github.com/yourusername/Python_Final_Project.git
cd Python_Final_Project
```

### Set Up the MCP Server

Handles communication with Google Drive and Dropbox.
```bash
cd mcp_server
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

#### Run the MCP Server
```bash
fastmcp run server.py:mcp --transport http --port 8001
```

#### Google Drive Setup

1. Place `credentials.json` into `mcp_server/`
2. On first run, authenticate via browser
3. `token.pickle` is created automatically

#### Dropbox Setup

1. Create a Dropbox app: https://www.dropbox.com/developers/apps
2. Generate an access token
3. Save it as:
```bash
mcp_server/dropbox_token.txt
```

### Set Up the LLM Backend
```bash
cd llm_backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

#### PostgreSQL Database Setup

The project uses PostgreSQL for user authentication and chat history storage.

1. Start PostgreSQL locally
2. Create a database:
```sql
CREATE DATABASE python_final_project;
```

3. Create `.env` inside `llm_backend/`:
```ini
DATABASE_URL=postgresql://<user>:<password>@localhost:5432/python_final_project
```

On startup, the backend automatically:

- Creates user tables
- Creates chat history tables
- Links conversations to authenticated users

#### Run the LLM Backend
```bash
uvicorn main:app --reload
```

### Run the Web Interface

With both servers running, open:
```
http://localhost:5000
```

You can now:

- Sign up or log in
- Submit natural-language file queries
- View saved chat history across sessions


## Environment Variables

Create `llm_backend/.env`:
```ini
OPENAI_API_KEY=your_api_key_here
MCP_SERVER_URL=http://localhost:8001
DATABASE_URL=postgresql://<user>:<password>@localhost:5432/python_final_project
```


## Tech Stack

- **Python 3.10+**
- **FastAPI** — backend framework
- **FastMCP** — MCP server provider
- **OpenAI API** — LLM reasoning
- **Google Drive API + Dropbox API**
- **PostgreSQL** — users + chat persistence
- **HTML / CSS / JavaScript** — frontend


## Final Presentation / Demo Outline

### Introduction (~2 minutes)

- Introduce team members and roles
- High-level overview of the project
- Problem being solved and why it matters

### Project Demonstration (~5 minutes)

- Live walkthrough of the GUI
- User login and authentication
- Natural-language file search and retrieval
- Switching between Google Drive and Dropbox
- Demonstration of chat history persistence

### Technical Details (~3 minutes)

- Architecture overview (MCP Server + LLM Backend + Database)
- How natural-language requests are translated into MCP actions
- PostgreSQL integration for users and chats
- Challenges faced:
  - OAuth handling
  - Multi-backend abstraction
  - Maintaining conversational context

### Conclusion (~2 minutes)

- Summary of core features
- Real-world applicability
- Future expansion ideas
- Q&A


## Contributors

- Jonathan Conde
- Felipe Monsalvo
- Luis Palma


## Future Improvements

- Add more storage providers (OneDrive, S3, Box)
- Improve file preview (PDFs, images)
- Advanced user roles and permissions
- Conversation analytics and search
- Caching for faster file operations
