# Architecture

# ResolveAI — System Architecture Document

# 1. Overview

ResolveAI is an enterprise-grade AI support copilot platform designed to automate support operations, intelligent ticket resolution, workflow orchestration, semantic knowledge retrieval, and escalation management.

The architecture is designed to support:

- scalability,
- modularity,
- high concurrency,
- asynchronous processing,
- enterprise security,
- AI integrations,
- future microservice evolution.

The platform follows a Modular Monolith Architecture with clear domain boundaries and future microservice extraction capabilities.

---

# 2. Architecture Style

## Selected Architecture
Modular Monolith (Microservice-Ready)

---

## Why Modular Monolith?

The system is initially designed as a modular monolith because:

- faster MVP development,
- reduced distributed-system complexity,
- easier debugging,
- simplified deployment,
- lower operational overhead.

However, all modules are isolated using bounded contexts and clean interfaces to enable future microservice migration.

---

# 3. High-Level Architecture

Frontend Layer
↓
NGINX Reverse Proxy / Load Balancer
↓
FastAPI Backend (Modular Monolith)
↓
PostgreSQL + Redis + ChromaDB
↓
OpenAI APIs

---

# 4. Core System Components

## 4.1 Frontend Layer

### Technologies
- Next.js
- TypeScript
- Tailwind CSS
- ShadCN UI

### Responsibilities
- User dashboards
- Ticket management UI
- Workflow visualization
- Knowledge base interaction
- Analytics dashboards

---

## 4.2 Backend Layer

### Technologies
- FastAPI
- SQLAlchemy
- Pydantic
- Celery

### Responsibilities
- Business logic
- Authentication
- Ticket lifecycle management
- Workflow orchestration
- Escalation processing
- AI orchestration
- Semantic retrieval

---

## 4.3 Database Layer

### PostgreSQL
Stores:
- users
- tickets
- workflows
- escalations
- notifications
- audit logs

### Redis
Used for:
- caching
- sessions
- rate limiting
- Celery task queues

### ChromaDB
Used for:
- embeddings storage
- semantic retrieval
- vector search

---

## 4.4 AI Layer

### Technologies
- OpenAI
- LangChain
- OpenAI Embeddings

### Responsibilities
- ticket classification
- semantic retrieval
- RAG pipelines
- AI-assisted query answering

---

# 5. Backend Module Architecture

The backend is divided into isolated domain modules.

| Module | Responsibility |
|---|---|
| auth | Authentication & JWT |
| users | User management |
| tickets | Ticket lifecycle |
| workflows | Workflow orchestration |
| escalations | Escalation management |
| notifications | Notification delivery |
| ai | AI processing & RAG |
| analytics | Dashboard analytics |
| knowledge_base | Document management |

---

# 6. Backend Folder Structure

backend/
│
├── app/
│   ├── core/
│   ├── middleware/
│   ├── modules/
│   ├── shared/
│   ├── tasks/
│   └── tests/

---

# 7. Request Lifecycle

## Ticket Creation Flow

User
→ API Request
→ Authentication Middleware
→ Ticket Service
→ PostgreSQL Storage
→ Workflow Trigger
→ Notification Trigger
→ Response Returned

---

# 8. AI Query Flow

User Query
→ Embedding Generation
→ Semantic Vector Search
→ Relevant Context Retrieval
→ Prompt Construction
→ OpenAI Response Generation
→ Final Response

---

# 9. RAG Architecture

The platform uses Retrieval-Augmented Generation (RAG).

## RAG Pipeline

Documents
→ Parsing
→ Chunking
→ Embedding Generation
→ ChromaDB Storage
→ Semantic Retrieval
→ Context Injection
→ LLM Response Generation

---

# 10. Async Processing Architecture

ResolveAI uses asynchronous task processing for heavy background operations.

## Technologies
- Celery
- Redis
- Celery Beat

## Async Tasks
- embedding generation
- document processing
- SLA monitoring
- escalation checks
- notifications
- analytics aggregation

---

# 11. Workflow Engine Architecture

The workflow engine manages:
- ticket state transitions,
- approvals,
- escalation rules,
- SLA enforcement.

---

# 12. Escalation Architecture

ResolveAI supports:

- SLA-based escalation
- priority escalation
- hierarchical escalation
- AI-based escalation (future)

---

# 13. Security Architecture

## Authentication
- JWT Access Tokens
- Refresh Tokens

## Authorization
- RBAC

## Password Security
- bcrypt hashing

## Multi-Tenancy
- tenant_id based isolation

## API Protection
- middleware validation
- rate limiting

---

# 14. Scalability Strategy

## Horizontal Scaling

Backend services remain stateless and can scale horizontally behind NGINX load balancing.

## Shared Infrastructure
- PostgreSQL
- Redis
- ChromaDB

support centralized scaling.

---

# 15. Observability Architecture

## Monitoring
- Prometheus

## Dashboards
- Grafana

## Logging
- Structured JSON logging

## Audit Logging
Tracks:
- user actions,
- workflow changes,
- ticket updates,
- AI operations.

---

# 16. Deployment Architecture

## Local Development
- Docker Compose

## CI/CD
- GitHub Actions

## Initial Deployment
- Render / Railway

## Future Cloud Deployment
- AWS

---

# 17. Future Microservice Evolution

The system is designed for gradual microservice extraction.

## Planned Future Services
- AI Service
- Notification Service
- Analytics Service

The architecture will evolve using event-driven communication and service isolation principles.

---

# 18. Conclusion

ResolveAI follows an enterprise-grade modular architecture designed for scalability, maintainability, AI integration, asynchronous processing, workflow orchestration, and future distributed-system evolution.