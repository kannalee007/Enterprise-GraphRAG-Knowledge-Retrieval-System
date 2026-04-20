# 🕸️ Nexus: Enterprise GraphRAG Knowledge Retrieval System

![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)
![Neo4j](https://img.shields.io/badge/Neo4j-018bff?style=for-the-badge&logo=neo4j&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

## 📌 Executive Summary
**Nexus** is an offline, production-grade Graph Retrieval-Augmented Generation (GraphRAG) system. Standard vector databases fail at multi-hop reasoning because they cannot map relationships between isolated text chunks. Nexus solves this by ingesting enterprise documents, utilizing local LLMs to automatically extract entities/relationships, and mapping them into a mathematical **Neo4j Knowledge Graph**. 

It features a fault-tolerant **Hybrid Search Engine** that dynamically routes queries between Vector Similarity and LLM-generated Cypher traversals, ensuring zero-hallucination data retrieval.

## 🏗️ Enterprise Architecture

```text
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   Browser UI    │────▶│  FastAPI Server │────▶│     Neo4j       │
│  (static/HTML)  │◄────│   (Python)      │◄────│  (Graph DB)     │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                │
                                ▼
                        ┌─────────────────┐
                        │     Ollama      │
                        │  (LLM + Embed)  │
                        │  qwen2.5:7b     │
                        │  nomic-embed    │
                        └─────────────────┘
🚀 Key Features
Automated ETL Pipeline: Upload PDFs/TXTs; the system automatically chunks, embeds, and extracts semantic entities (people, technologies, concepts).

Hybrid Search Engine: Combines 768-dimensional Vector Search (nomic-embed-text) with dynamic LLM-generated Cypher queries for multi-hop graph traversal.

Conversational Memory: Stateful chat interface allowing for multi-turn Q&A with deep context retention.

Visual Entity Explorer: Interactive UI to explore extracted nodes and visualize how documents mathematically connect to specific entities.

100% Offline & Zero-Trust: All embeddings, language generation, and data storage occur locally, guaranteeing zero proprietary data leakage.

🛠️ Technology Stack
Graph Database: Neo4j (with APOC plugin & Vector Index support)

Local Intelligence: Ollama (LLM: qwen2.5:7b | Embeddings: nomic-embed-text)

Backend Pipeline: FastAPI, LangChain (RecursiveCharacterTextSplitter)

Infrastructure: Docker & Docker Compose

Frontend: Vanilla HTML/JS (Lightweight, dependency-free)

🐳 Quick Start (Docker Integration)
1. Spin up the Database Environment:

Bash
# Starts the Neo4j Graph Database via Docker Compose
docker-compose up -d
Access the Neo4j browser at http://localhost:7474

2. Start the FastAPI Backend:

Bash
# Install dependencies
pip install -r requirements.txt

# Start the application server
uvicorn main:app --reload
Access the interactive UI at http://localhost:8000/ui

🛡️ Security & Known Limitations
This system is designed with explicit fault boundaries to ensure stability:

Cypher Injection Protection: User-prompted Cypher queries are strictly sandboxed to read-only execution (blocks DELETE, CREATE, etc.).

Entity Validation: The extraction pipeline drops empty or malformed entity nodes to prevent graph pollution.

Format Constraints: The ingestion pipeline requires text-based PDFs (scanned image OCR is currently out of scope).

Ephemeral Memory: Multi-turn conversation history is stored in-memory and resets upon server restart.

Protocol: Currently HTTP (Requires Nginx/Traefik reverse proxy for production HTTPS deployment).
