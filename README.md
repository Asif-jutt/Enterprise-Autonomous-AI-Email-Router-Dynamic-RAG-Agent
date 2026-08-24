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
✨ Key Features3-Tier Intent Classification: Intelligent LLM classifier routes messages into designated operational execution paths:Spam: Silent termination at NoOp to save token costs and eliminate junk emails.Business_Inquiry: Triggers full RAG pipeline (FastAPI, MERN, Flutter, LangChain, .NET, Machine Learning) to fetch technical capabilities, proposal specs, and pricing models.General_Inquiry: Fast, lightweight AI responses for non-technical notes, thank-you messages, and meeting follow-ups.Vector-Grounded RAG Pipeline: Connects Pinecone Vector Store with Groq LLMs (llama-3.3-70b-versatile) to query company context, technical architectures, and founder profiles before writing replies.Zero-Hallucination Guardrails: System prompts restrict technical claims strictly to facts retrieved from the Pinecone index.Deterministic Output Parsing: Custom JavaScript transformation layer extracts and sanitizes raw LLM output into clean subject and message payload properties.Native Thread Preservation: Extracts original id and threadId metadata from Gmail payloads to ensure replies maintain ongoing thread continuity.🛠 Tech StackWorkflow Engine: n8n AutomationInference Model: Groq API (llama-3.3-70b-versatile)Vector Database: Pinecone Vector StoreEmbeddings: Local Ollama (nomic-embed-text) / Sentence TransformersEmail API: Google Cloud Gmail API (OAuth 2.0)Code Transformations: Node.js / JavaScript (n8n Code Node)📁 Repository StructurePlaintext.
├── workflows/
│   └── enterprise_email_rag_router.json  # Exported n8n workflow file
├── scripts/
│   └── output_parser.js                  # JS Regex parser for Subject & Body extraction
├── docs/
│   └── classification_rules.json         # Prompt definitions for email routing
├── README.md                             # System documentation
└── LICENSE                               # MIT License
🚀 Installation & Setup1. PrerequisitesA running n8n instance (Self-hosted via Docker or Cloud).Pinecone account with an active vector index containing company knowledge base embeddings.Groq API Key for low-latency LLM inference.Google Cloud Console project with Gmail API enabled and OAuth 2.0 credentials configured.2. Environment ConfigurationConfigure the following credential assets within your n8n credentials dashboard:Groq API KeyPinecone Vector Store API KeyGmail OAuth2 API3. Workflow ImportDownload workflows/enterprise_email_rag_router.json.Navigate to your n8n Dashboard $\rightarrow$ Workflows $\rightarrow$ Import from File.Select the .json file.Link your saved credentials to the respective Groq, Pinecone, and Gmail nodes.🧪 Pipeline Validation & Test MatrixThe pipeline was validated against three distinct incoming message profiles:Input CategoryIncoming Sample ExcerptClassificationExecution OutcomeSpam"Boost your website to #1 on Google with cheap backlink packages!"SpamTerminated at NoOp (0 Tokens consumed by RAG Agent).Technical Lead"Looking for a secure enterprise RAG platform using LangChain & Pinecone..."Business_InquiryPinecone vector search executed $\rightarrow$ Structured proposal email generated.General Note"Thank you for the discovery call yesterday, talk soon!"General_InquiryDirect LLM response generated without calling Pinecone.👨‍💻 AuthorAsif HussainFull-Stack Engineer & AI Automation SpecialistGitHub: github.com/asifhussainLinkedIn: linkedin.com/in/asifhussainPortfolio: asif-ai-solutions.com
