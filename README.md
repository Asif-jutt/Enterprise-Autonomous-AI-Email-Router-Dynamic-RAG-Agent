#🤖 Enterprise Autonomous AI Email Router & Dynamic RAG AgentAn enterprise-grade, automated email triage and intelligent response pipeline built with n8n, Groq LLM, Pinecone Vector Store, and Gmail.This system acts as a 24/7 autonomous sales and technical operations assistant. It ingests incoming emails, classifies them into functional routes, executes vector search for deep context retrieval, and generates structured, context-grounded email responses.🏗 System Architecture & WorkflowPlaintext                        ┌────────────────────────┐
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
          ┌─────────────────────────┼─────────────────────────┐
          │                         │                         │
          ▼                         ▼                         ▼
  [ Output 1: Spam ]      [ Output 2: Business ]    [ Output 3: General ]
          │                         │                         │
          ▼                         ▼                         ▼
   ┌─────────────┐       ┌─────────────────────┐    ┌────────────────────┐
   │    NoOp     │       │ AI Agent + Pinecone │    │  Lightweight LLM   │
   │ (Terminates)│       │  Vector Search Tool │    │  (General Reply)   │
   └─────────────┘       └──────────┬──────────┘    └─────────┬──────────┘
                                    │                         │
                                    └────────────┬────────────┘
                                                 │
                                                 ▼
                                     ┌──────────────────────┐
                                     │  JavaScript Parser   │
                                     └───────────┬──────────┘
                                                 │
                                                 ▼
                                     ┌──────────────────────┐
                                     │  Gmail Reply Node    │
                                     └──────────────────────┘
Key FeaturesMulti-Stage LLM Email Classification: Intelligent intent detection that routes emails into three distinct execution paths:Spam: Silent termination to optimize API token usage.Business_Inquiry: Triggers full RAG pipeline for custom quotes, technical architecture inquiries, and service specs.General_Inquiry: Fast, lightweight AI responses for greetings, thank-you notes, and scheduling updates.Vector-Grounded RAG Engine: Integrates Pinecone Vector Database with Groq LLMs (llama-3.3-70b-versatile) to query internal documentation, tech stacks, and pricing schemas before writing responses.Deterministic Parsing & Payload Safety: Custom JavaScript transformation layer to parse unstructured Markdown output into sanitized, JSON-compliant parameters for Gmail API consumption.Thread-Aware Email Automation: Retains original message contexts, subject chains, and threadId metadata to preserve natural conversation threading.Tech Stack & IntegrationsOrchestration: n8n Workflow AutomationLLM Engine: Groq API (llama-3.3-70b-versatile)Vector Database: Pinecone Vector StoreEmbeddings: Ollama / Local Vector Embeddings (nomic-embed-text)Email Service: Gmail API (OAuth 2.0)Code Execution: Node.js / JavaScript (n8n Code Node)📁 Repository StructurePlaintext.
├── workflows/
│   └── enterprise_email_rag_router.json  # Complete n8n workflow export
├── scripts/
│   └── output_parser.js                  # Regex parser for Subject & Message extraction
├── docs/
│   └── category_definitions.json         # LLM classification schemas
└── README.md
🚀 Setup & Installation1. PrerequisitesRunning instance of n8n (Self-hosted via Docker or Cloud)Pinecone API Key & Index populated with company context embeddingsGroq API Key for high-speed inferenceGoogle Cloud Console OAuth 2.0 App with Gmail API enabled2. Environment Variables & CredentialsConfigure the following credentials in your n8n credentials panel:Groq API KeyPinecone Vector Store API KeyGmail OAuth2 API3. Workflow ImportDownload workflows/enterprise_email_rag_router.json.Open your n8n Dashboard $\rightarrow$ Click Workflows $\rightarrow$ Import from File.Select the exported JSON file.Re-assign your saved credentials to the Groq, Pinecone, and Gmail nodes.🧪 Testing & VerificationThe pipeline has been validated across distinct edge cases:Input IntentInput Sample SnippetCategory ResultAction TakenSpam / Unsolicited"Boost your website to #1 on Google fast with cheap backlinks!"SpamTerminated at NoOp (0 Tokens consumed on main Agent).Technical Proposal"Looking for a secure RAG pipeline using LangChain, FastAPI & Pinecone..."Business_InquiryPinecone vector search executed $\rightarrow$ Technical proposal drafted.Follow-up / Note"Thank you for the call yesterday, sending team notes soon!"General_InquiryDirect LLM response generated without querying vector store.🛡 Performance & Security ConsiderationsZero-Hallucination Constraints: The system message strictly restricts technical specs and pricing to retrieved context from the Pinecone vector index.Cost Efficiency: Token usage is optimized by terminating spam at stage one and reserving RAG vector queries solely for qualified business leads.
