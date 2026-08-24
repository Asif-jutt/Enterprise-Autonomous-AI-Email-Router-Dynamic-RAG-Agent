# 🤖 Enterprise Autonomous AI Email Router & Dynamic RAG Agent

[![n8n](https://img.shields.io/badge/Orchestration-n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Groq](https://img.shields.io/badge/LLM_Inference-Groq_API-f368e0?style=for-the-badge)](https://groq.com/)
[![Pinecone](https://img.shields.io/badge/Vector_DB-Pinecone-000000?style=for-the-badge&logo=pinecone&logoColor=white)](https://www.pinecone.io/)
[![Gmail](https://img.shields.io/badge/Integration-Gmail_API-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](https://developers.google.com/gmail/api)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

An end-to-end, enterprise-grade automated email triage and intelligent RAG response system built with **n8n**, **Groq LLM**, **Pinecone Vector Database**, and **Gmail API**.

This system acts as an autonomous 24/7 technical sales and operations assistant. It ingests incoming emails, classifies client intent into 3 execution routes, performs semantic vector search over internal knowledge bases, and drafts thread-aware, context-grounded email responses.

---

## 🏗 Workflow Architecture

```text
                            ┌────────────────────────┐
                            │   Gmail Trigger Node   │
                            └───────────┬────────────┘
                                        │
                                        ▼
                            ┌────────────────────────┐
                            │    Edit Fields Node    │
                            └───────────┬────────────┘
                                        │
                                        ▼
                            ┌────────────────────────┐
                            │ Text Classifier (LLM)  │
                            └───────────┬────────────┘
                                        │
          ┌─────────────────────────────┼─────────────────────────────┐
          │                             │                             │
          ▼                             ▼                             ▼
  [ Output 1: Spam ]          [ Output 2: Business ]        [ Output 3: General ]
          │                             │                             │
          ▼                             ▼                             ▼
   ┌─────────────┐           ┌─────────────────────┐       ┌────────────────────┐
   │    NoOp     │           │ AI Agent + Pinecone │       │  Lightweight LLM   │
   │ (Terminates)│           │  Vector Search Tool │       │  (General Reply)   │
   └─────────────┘           └──────────┬──────────┘       └─────────┬──────────┘
                                        │                            │
                                        └──────────────┬─────────────┘
                                                       │
                                                       ▼
                                           ┌──────────────────────┐
                                           │  JavaScript Parser   │
                                           └───────────┬──────────┘
                                                       │
                                                       ▼
                                           ┌──────────────────────┐
                                           │  Gmail Reply Node    │
                                           └───────────┬──────────┘
