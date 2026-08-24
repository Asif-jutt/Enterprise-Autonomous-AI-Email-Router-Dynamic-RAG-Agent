# 🤖 Enterprise Autonomous AI Email Router & Dynamic RAG Agent

[![n8n](https://img.shields.io/badge/Orchestration-n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io/)
[![Groq](https://img.shields.io/badge/LLM_Inference-Groq_API-f368e0?style=for-the-badge)](https://groq.com/)
[![Pinecone](https://img.shields.io/badge/Vector_DB-Pinecone-000000?style=for-the-badge&logo=pinecone&logoColor=white)](https://www.pinecone.io/)
[![Gmail](https://img.shields.io/badge/Integration-Gmail_API-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](https://developers.google.com/gmail/api)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

An end-to-end, enterprise-grade automated email triage and intelligent RAG response system built with **n8n**, **Groq LLM**, **Pinecone Vector Database**, and **Gmail API**.

This system acts as an autonomous 24/7 technical sales and operations assistant. It ingests incoming emails, classifies client intent into three execution routes, performs semantic vector search over internal knowledge bases, and drafts thread-aware, context-grounded email responses.

---

## 🏗 Workflow Architecture

```text
                            ┌────────────────────────┐
                            │    Gmail Trigger Node   │
                            └───────────┬────────────┘
                                        │
                                        ▼
                            ┌────────────────────────┐
                            │    Edit Fields Node     │
                            └───────────┬────────────┘
                                        │
                                        ▼
                            ┌────────────────────────┐
                            │  Text Classifier (LLM)  │
                            └───────────┬────────────┘
                                        │
          ┌─────────────────────────────┼─────────────────────────────┐
          │                             │                             │
          ▼                             ▼                             ▼
  [ Output 1: Spam ]          [ Output 2: Business ]        [ Output 3: General ]
          │                             │                             │
          ▼                             ▼                             ▼
   ┌─────────────┐           ┌─────────────────────┐       ┌────────────────────┐
   │    NoOp     │           │  AI Agent + Pinecone │       │  Lightweight LLM   │
   │ (Terminates)│           │   Vector Search Tool │       │  (General Reply)   │
   └─────────────┘           └──────────┬──────────┘       └─────────┬──────────┘
                                         │                            │
                                         └──────────────┬─────────────┘
                                                         │
                                                         ▼
                                            ┌──────────────────────┐
                                            │   JavaScript Parser   │
                                            └───────────┬──────────┘
                                                         │
                                                         ▼
                                            ┌──────────────────────┐
                                            │   Gmail Reply Node    │
                                            └──────────────────────┘
```

---

## ✨ Key Features

- **3-Tier Intent Classification** — An intelligent LLM classifier routes incoming messages into designated operational execution paths:
  - **Spam** — Silent termination at `NoOp` to save token costs and eliminate junk emails.
  - **Business_Inquiry** — Triggers the full RAG pipeline (FastAPI, MERN, Flutter, LangChain, .NET, Machine Learning) to fetch technical capabilities, proposal specs, and pricing models.
  - **General_Inquiry** — Fast, lightweight AI responses for non-technical notes, thank-you messages, and meeting follow-ups.

- **Vector-Grounded RAG Pipeline** — Connects a Pinecone Vector Store with Groq LLMs (`llama-3.3-70b-versatile`) to query company context, technical architectures, and founder profiles before writing replies.

- **Zero-Hallucination Guardrails** — System prompts restrict technical claims strictly to facts retrieved from the Pinecone index.

- **Deterministic Output Parsing** — A custom JavaScript transformation layer extracts and sanitizes raw LLM output into clean `subject` and `message` payload properties.

- **Native Thread Preservation** — Extracts the original `id` and `threadId` metadata from Gmail payloads to ensure replies maintain ongoing thread continuity.

---

## 🛠 Tech Stack

| Component | Technology |
|---|---|
| Workflow Engine | n8n Automation |
| Inference Model | Groq API (`llama-3.3-70b-versatile`) |
| Vector Database | Pinecone Vector Store |
| Embeddings | Local Ollama (`nomic-embed-text`) / Sentence Transformers |
| Email API | Google Cloud Gmail API (OAuth 2.0) |
| Code Transformations | Node.js / JavaScript (n8n Code Node) |

---

## 📁 Repository Structure

```text
.
├── workflows/
│   └── enterprise_email_rag_router.json   # Exported n8n workflow file
├── scripts/
│   └── output_parser.js                   # JS regex parser for Subject & Body extraction
├── docs/
│   └── classification_rules.json          # Prompt definitions for email routing
├── README.md                              # System documentation
└── LICENSE                                # MIT License
```

---

## 🚀 Installation & Setup

### 1. Prerequisites

- A running n8n instance (self-hosted via Docker, or n8n Cloud).
- A Pinecone account with an active vector index containing your company knowledge base embeddings.
- A Groq API key for low-latency LLM inference.
- A Google Cloud Console project with the Gmail API enabled and OAuth 2.0 credentials configured.

### 2. Environment Configuration

Configure the following credential assets within your n8n credentials dashboard:

- Groq API Key
- Pinecone Vector Store API Key
- Gmail OAuth2 API

### 3. Workflow Import

1. Download `workflows/enterprise_email_rag_router.json`.
2. Navigate to your **n8n Dashboard → Workflows → Import from File**.
3. Select the `.json` file.
4. Link your saved credentials to the respective Groq, Pinecone, and Gmail nodes.

---

## 🧪 Pipeline Validation & Test Matrix

The pipeline was validated against three distinct incoming message profiles:

| Input Category | Incoming Sample Excerpt | Classification | Execution Outcome |
|---|---|---|---|
| Spam | "Boost your website to #1 on Google with cheap backlink packages!" | `Spam` | Terminated at `NoOp` (0 tokens consumed by RAG Agent). |
| Technical Lead | "Looking for a secure enterprise RAG platform using LangChain & Pinecone..." | `Business_Inquiry` | Pinecone vector search executed → structured proposal email generated. |
| General Note | "Thank you for the discovery call yesterday, talk soon!" | `General_Inquiry` | Direct LLM response generated without calling Pinecone. |

---

## 👨‍💻 Author

**Asif Hussain**
Full-Stack Engineer & AI Automation Specialist
Email: [asifhussain5115@gmail.com](mailto:asifhussain5115@gmail.com)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
