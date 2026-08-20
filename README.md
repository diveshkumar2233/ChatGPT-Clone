# ChatGPT-Clone

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white" alt="Python 3.11">
  <img src="https://img.shields.io/badge/FastAPI-backend-009688?logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/Jinja-templates-B41717?logo=jinja&logoColor=white" alt="Jinja2">
  <img src="https://img.shields.io/badge/LangGraph-multi--agent-1C3C3C?logo=langgraph&logoColor=white" alt="LangGraph">
  <img src="https://img.shields.io/badge/🦜🔗_LangChain-tools-1C3C3C?logoColor=white" alt="LangChain">
  <img src="https://img.shields.io/badge/Gemini-LLM-4285F4?logo=googlegemini&logoColor=white" alt="Google Gemini">
  <img src="https://img.shields.io/badge/Tavily-web--search-0EA5E9?logoColor=white" alt="Tavily">
  <img src="https://img.shields.io/badge/ChromaDB-vector--store-FF6F00?logoColor=white" alt="ChromaDB">
  <img src="https://img.shields.io/badge/SQLite-database-003B57?logo=sqlite&logoColor=white" alt="SQLite">
  <img src="https://img.shields.io/badge/Docker-ready-2496ED?logo=docker&logoColor=white" alt="Docker">
  <img src="https://img.shields.io/badge/AWS-EC2%20%7C%20ECR-FF9900?logo=amazonaws&logoColor=white" alt="AWS">
  <img src="https://img.shields.io/badge/GitHub_Actions-CI%2FCD-2088FF?logo=githubactions&logoColor=white" alt="GitHub Actions">
  <img src="https://img.shields.io/badge/License-MIT-yellow.svg" alt="License">
</p>

<p align="center">
  <img src="https://img.shields.io/github/stars/YOUR_USERNAME/ChatGBT-Clone?style=social" alt="Stars">
  <img src="https://img.shields.io/github/forks/YOUR_USERNAME/ChatGBT-Clone?style=social" alt="Forks">
  <img src="https://img.shields.io/github/last-commit/YOUR_USERNAME/ChatGBT-Clone" alt="Last commit">
  <img src="https://img.shields.io/github/issues/YOUR_USERNAME/ChatGBT-Clone" alt="Issues">
</p>

ChatGPT-Clone is an open-source agentic AI chatbot built with Python, FastAPI, LangGraph, LangChain, Google Gemini, Tavily, ChromaDB, and SQLite.

**🔗 Live Demo:** [https://chatgpt-clone-3-9d1u.onrender.com](https://chatgpt-clone-3-9d1u.onrender.com/)

It supports real-time streaming chat, document uploads, retrieval-augmented generation (RAG), web search, conversation memory, a **multi-agent orchestration system**, and a simple web UI.

> Replace `YOUR_USERNAME` in the badge URLs above with your actual GitHub username once the repo is pushed — the stars/forks/commit/issues badges pull live data automatically from shields.io, no manual updates needed.

## Table of Contents

- [Live Demo](#-live-demo)
- [Features](#features)
- [Multi-Agent Architecture](#multi-agent-architecture)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Environment Variables](#environment-variables)
- [Run Locally](#run-locally)
- [Project Structure](#project-structure)
- [Docker Deployment](#docker-deployment)
- [AWS CI/CD Deployment](#aws-cicd-deployment-with-github-actions)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## 🔗 Live Demo

Try the app here: **[https://chatgpt-clone-3-9d1u.onrender.com](https://chatgpt-clone-3-9d1u.onrender.com/)**

> Note: this is hosted on Render's free tier, so the app may take 30–60 seconds to wake up if it's been idle.

## Features

- Chat with a multi-agent AI system powered by Google Gemini
- Multiple specialized agents (e.g. router, RAG agent, web-search agent, general chat agent) coordinated via LangGraph
- Stream responses in real time
- Upload documents such as PDF, DOCX, TXT, MD, PY, and CSV
- Use uploaded files as context through RAG
- Search the web with Tavily for current information
- Store and recall conversation history
- Simple FastAPI-based web interface
- Docker-ready deployment
- AWS CI/CD support using GitHub Actions, ECR, and EC2

## Project Overview

This project combines:

- **FastAPI** for the backend server and API endpoints
- **Jinja2** for rendering the frontend UI
- **LangGraph** for multi-agent orchestration — routing user queries to the right agent (RAG, web search, or general conversation) and managing shared state between agents
- **LangChain** for tools, messages, and RAG workflow
- **Google Gemini** as the LLM provider
- **Tavily** for web search
- **ChromaDB** for vector search over uploaded documents
- **SQLite** for conversation and persistence
- **Docker** for containerized deployment

## Multi-Agent Architecture

Instead of a single monolithic chat node, ChatGBT-Clone routes each user message through a graph of specialized agents:

- **Router Agent** — decides which agent(s) should handle the incoming query
- **RAG Agent** — answers questions using retrieved context from uploaded documents
- **Web Search Agent** — fetches current information via Tavily when the query needs up-to-date data
- **General Chat Agent** — handles conversational queries that don't require retrieval or search
- **Memory Layer** — persists conversation state across turns using SQLite-backed checkpointing

This lets each agent specialize in one task and keeps the overall system easier to extend (e.g. adding a code-execution agent or a summarization agent later).

```mermaid
flowchart TD
    U[User Message] --> R{Router Agent}
    R -->|needs document context| RAG[RAG Agent]
    R -->|needs current info| WEB[Web Search Agent]
    R -->|general conversation| CHAT[General Chat Agent]

    RAG --> DB[(ChromaDB\nVector Store)]
    WEB --> TAVILY[(Tavily\nWeb Search API)]

    RAG --> M[Memory / State]
    WEB --> M
    CHAT --> M

    M --> SQLITE[(SQLite\nCheckpoint Store)]
    M --> OUT[Streamed Response]
    OUT --> U
```

> GitHub renders Mermaid diagrams natively in Markdown — this block will show as a live flowchart on your repo page, no image export needed.

## Prerequisites

Make sure you have the following installed:

- Python 3.11
- pip or conda
- Git
- Google API key for Gemini
- Tavily API key for web search

Optional for deployment:

- Docker
- AWS account
- Amazon ECR repository
- EC2 instance
- GitHub Actions self-hosted runner

## Getting Started

### 1. Clone the repository

```
git clone https://github.com/<your-username>/ChatGBT-Clone.git
```

### 2. Navigate to the project directory

```
cd ChatGBT-Clone
```

### 3. Create a virtual environment

Using conda:

```
conda create -n chatgbtclone python=3.11 -y
```

### 4. Activate the virtual environment

```
conda activate chatgbtclone
```

### 5. Install dependencies

```
pip install -r requirements.txt
```

## Environment Variables

Create a `.env` file in the project root directory.

```
GOOGLE_API_KEY=your_google_api_key
GOOGLE_MODEL=gemini-2.5-flash

TAVILY_API_KEY=your_tavily_api_key

LANGSMITH_TRACING=false
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_API_KEY=your_langsmith_api_key
LANGSMITH_PROJECT=chatgbt-clone
```

If you do not want to use LangSmith tracing, keep:

```
LANGSMITH_TRACING=false
```

## Run Locally

Start the FastAPI app:

```
python app.py
```

The app will be available at:

```
http://127.0.0.1:8080
```

## Project Structure

```
ChatGBT-Clone/
│
├── app.py                  # FastAPI app and streaming chat endpoints
├── agents/                 # Multi-agent definitions and LangGraph orchestration
│   ├── router_agent.py     # Routes queries to the correct specialized agent
│   ├── rag_agent.py        # Handles document-grounded (RAG) queries
│   ├── web_search_agent.py # Handles web search queries via Tavily
│   └── chat_agent.py       # Handles general conversational queries
├── database.py             # Conversation and persistence logic
├── rag.py                  # Document ingestion and RAG logic
├── tools.py                # Agent tools such as web search, memory, and RAG
├── requirements.txt        # Python dependencies
├── Dockerfile              # Docker image configuration
├── .dockerignore           # Docker ignore rules
│
├── templates/
│   └── index.html          # Frontend UI
│
├── uploads/                # Uploaded documents
├── data/                   # SQLite database and app data
└── chroma_db/              # ChromaDB vector database storage
```

## Docker Deployment

### 1. Build the Docker image

```
docker build -t chatgbt-clone .
```

### 2. Run the Docker container

```
docker run -d \
  --name chatgbt-clone \
  --restart always \
  -p 8080:8080 \
  --env-file .env \
  chatgbt-clone
```

The app will be available at:

```
http://localhost:8080
```

## AWS CI/CD Deployment with GitHub Actions

This project can be deployed to AWS using:

- GitHub Actions
- Amazon ECR
- Amazon EC2
- Docker
- GitHub self-hosted runner

### 1. Create an IAM User

Create an IAM user for deployment and attach the following policies:

- AmazonEC2ContainerRegistryFullAccess
- AmazonEC2FullAccess

You can also use a more restricted custom IAM policy for production.

### 2. Create an ECR Repository

Create an Amazon ECR repository.

Example full ECR image URI:

```
<your-account-id>.dkr.ecr.us-east-1.amazonaws.com/chatgbt-clone
```

For GitHub Secrets, only save the repository name:

```
ECR_REPO=chatgbt-clone
```

Do not save the full ECR URI as `ECR_REPO`.

### 3. Create an EC2 Instance

Create an Ubuntu EC2 instance.

Recommended inbound security group rule:

```
Type: Custom TCP
Port: 8080
Source: 0.0.0.0/0
```

### 4. Install Docker on EC2

Connect to your EC2 instance and run:

```
sudo apt-get update -y
sudo apt-get upgrade -y
```

Install Docker:

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Add the Ubuntu user to the Docker group:

```
sudo usermod -aG docker ubuntu
newgrp docker
```

Check Docker:

```
docker --version
```

### 5. Configure EC2 as a GitHub Self-Hosted Runner

Go to your GitHub repository:

```
Settings → Actions → Runners → New self-hosted runner
```

Select Linux and follow the commands shown by GitHub.

After setup, start the runner:

```
./run.sh
```

For production, you can configure the runner as a service:

```
sudo ./svc.sh install
sudo ./svc.sh start
```

## GitHub Secrets

Add the following secrets in your GitHub repository:

```
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
AWS_DEFAULT_REGION
ECR_REPO

GOOGLE_API_KEY
GOOGLE_MODEL
TAVILY_API_KEY
LANGSMITH_TRACING
LANGSMITH_ENDPOINT
LANGSMITH_API_KEY
LANGSMITH_PROJECT
```

Path:

```
GitHub Repository → Settings → Secrets and variables → Actions → New repository secret
```

Example:

```
AWS_DEFAULT_REGION=us-east-1
ECR_REPO=chatgbt-clone
GOOGLE_MODEL=gemini-2.5-flash
LANGSMITH_TRACING=true
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_PROJECT=chatgbt-clone
```

## GitHub Actions Workflow

Create this file:

```
.github/workflows/cicd.yaml
```

This workflow will:

- Build your Docker image
- Push the image to Amazon ECR
- Pull the latest image on EC2
- Stop the old container
- Run the new container

## Usage

After running locally or deploying to AWS:

- Open the app in your browser.
- Start chatting with the multi-agent AI assistant.
- Upload documents to use them as context.
- Ask questions about uploaded files.
- Ask current-information questions to trigger web search.
- Continue conversations with saved chat history.

## Example Questions

- Summarize the uploaded PDF.
- Search the web for the latest AI agent news.
- Based on my uploaded document, what are the key points?
- Calculate 125 * 48 / 6.

## Notes

- Do not commit your `.env` file to GitHub.
- Keep API keys inside GitHub Secrets for deployment.
- For production, avoid using `reload=True` in Uvicorn.
- Make sure port 8080 is open in your EC2 security group.
- Rotate any API keys that were accidentally exposed publicly.

## Resume Highlights

Use these bullet points on your resume or portfolio to describe this project:

- Designed and developed **ChatGBT-Clone**, a production-style multi-agent conversational AI system orchestrated with **LangGraph**, routing user queries across specialized agents (RAG, web search, general chat) for context-aware responses.
- Implemented **retrieval-augmented generation (RAG)** using **ChromaDB** for document-grounded Q&A over PDF/DOCX/CSV uploads, and integrated **Tavily** for real-time web search capabilities.
- Engineered a **FastAPI** backend with streaming chat endpoints, **SQLite**-backed conversation memory, and containerized the app with **Docker**, deploying to **AWS EC2** via an automated **GitHub Actions CI/CD pipeline** (ECR + self-hosted runner).

> Tip: if this project is going on your resume, consider using a more original name than "ChatGBT-Clone" — something like "GraphMind AI" reads as an independent product rather than a reference to an existing brand.

## Contributing

Contributions are welcome.

To contribute:

1. Fork the repository.
2. Create a new branch.
3. Make your changes.
4. Submit a pull request.

## License

This project is open source. Please check the repository license for usage terms.
