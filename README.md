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
