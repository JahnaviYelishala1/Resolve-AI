# Database Schema

# ResolveAI — Database Schema Design

# 1. Overview

This document defines the database architecture and schema design for ResolveAI, an enterprise-grade AI support copilot platform focused on intelligent ticket resolution, workflow automation, semantic retrieval, escalation management, and enterprise support operations.

The database architecture is designed to support:

- scalability,
- multi-tenancy,
- auditability,
- workflow orchestration,
- AI integrations,
- enterprise security,
- future microservice evolution.

Primary database used:
- PostgreSQL

Additional supporting databases:
- Redis
- ChromaDB

---

# 2. Database Design Principles

The following design principles are followed throughout the schema architecture:

## 2.1 UUID-Based Primary Keys

All entities use UUIDs instead of integer IDs for:
- distributed-system compatibility,
- security,
- scalability,
- easier future microservice migration.

---

## 2.2 Multi-Tenant Architecture

All enterprise entities contain:
- tenant_id

This ensures:
- tenant-level data isolation,
- SaaS scalability,
- enterprise security boundaries.

---

## 2.3 Auditability

All critical entities include:
- created_at
- updated_at
- created_by
- updated_by

This enables:
- change tracking,
- enterprise audit logs,
- compliance support.

---

## 2.4 Soft Deletion Strategy

Important entities may later support:
- deleted_at

instead of permanent deletion.

---

# 3. Core Entities

The following are the core business entities in ResolveAI.

| Entity | Purpose |
|---|---|
| Users | Employee, support agents, and admin users |
| Roles | RBAC and permission management |
| Tickets | Core support tickets |
| TicketComments | Ticket communication history |
| TicketAssignments | Ticket assignment tracking |
| Escalations | Escalation management |
| Workflows | Workflow state management |
| Notifications | User notifications and alerts |
| AuditLogs | Enterprise audit trail |
| KnowledgeBaseDocuments | Uploaded enterprise documents |
| DocumentChunks | Semantic search chunks for RAG |
| SLAConfigurations | SLA rules and timing policies |

---

# 4. Common Fields

The following fields are common across most tables.

| Field | Purpose |
|---|---|
| id | UUID primary key |
| tenant_id | Tenant isolation |
| created_at | Record creation timestamp |
| updated_at | Record update timestamp |
| created_by | User who created record |
| updated_by | User who last updated record |

---

# 5. Users Table

## Purpose

Stores all platform users including:
- employees,
- support agents,
- administrators.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| tenant_id | UUID | Tenant isolation |
| first_name | VARCHAR | User first name |
| last_name | VARCHAR | User last name |
| email | VARCHAR | Unique email |
| password_hash | TEXT | bcrypt hashed password |
| role_id | UUID | FK to roles |
| is_active | BOOLEAN | User active status |
| last_login | TIMESTAMP | Last login timestamp |
| created_at | TIMESTAMP | Creation timestamp |
| updated_at | TIMESTAMP | Update timestamp |

---

# 6. Roles Table

## Purpose

Stores RBAC roles.

---

## Supported Roles

- EMPLOYEE
- SUPPORT_AGENT
- ADMIN

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| name | VARCHAR | Role name |
| description | TEXT | Role description |

---

# 7. Tickets Table

## Purpose

Core support ticket entity.

---

## Ticket Statuses

- OPEN
- IN_PROGRESS
- PENDING_APPROVAL
- ESCALATED
- ON_HOLD
- RESOLVED
- REOPENED
- CLOSED

---

## Priority Levels

- LOW
- MEDIUM
- HIGH
- CRITICAL

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| tenant_id | UUID | Tenant isolation |
| title | VARCHAR | Ticket title |
| description | TEXT | Ticket description |
| status | VARCHAR | Current ticket status |
| priority | VARCHAR | Ticket priority |
| department | VARCHAR | Assigned department |
| created_by | UUID | FK to users |
| assigned_to | UUID | FK to users |
| escalation_level | INTEGER | Current escalation level |
| sla_deadline | TIMESTAMP | SLA resolution deadline |
| resolved_at | TIMESTAMP | Resolution timestamp |
| closed_at | TIMESTAMP | Closure timestamp |
| created_at | TIMESTAMP | Creation timestamp |
| updated_at | TIMESTAMP | Update timestamp |

---

# 8. TicketComments Table

## Purpose

Stores communication history for tickets.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| ticket_id | UUID | FK to tickets |
| user_id | UUID | FK to users |
| message | TEXT | Comment message |
| created_at | TIMESTAMP | Creation timestamp |

---

# 9. TicketAssignments Table

## Purpose

Tracks ticket assignment history.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| ticket_id | UUID | FK to tickets |
| assigned_from | UUID | Previous assignee |
| assigned_to | UUID | New assignee |
| assigned_by | UUID | User performing assignment |
| assigned_at | TIMESTAMP | Assignment timestamp |

---

# 10. Escalations Table

## Purpose

Tracks escalation events and escalation history.

---

## Escalation Types

- SLA Escalation
- Hierarchical Escalation
- Priority Escalation
- AI-Based Escalation (future)

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| ticket_id | UUID | FK to tickets |
| escalation_level | INTEGER | Escalation level |
| escalated_from | UUID | Previous owner |
| escalated_to | UUID | New owner |
| reason | TEXT | Escalation reason |
| escalated_by | UUID | User/system escalating |
| escalated_at | TIMESTAMP | Escalation timestamp |

---

# 11. Workflows Table

## Purpose

Stores workflow state transitions and workflow automation rules.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| workflow_name | VARCHAR | Workflow name |
| source_state | VARCHAR | Previous state |
| target_state | VARCHAR | Next state |
| approval_required | BOOLEAN | Approval requirement |
| created_at | TIMESTAMP | Creation timestamp |

---

# 12. Notifications Table

## Purpose

Stores user notifications and alerts.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| user_id | UUID | FK to users |
| title | VARCHAR | Notification title |
| message | TEXT | Notification content |
| is_read | BOOLEAN | Read status |
| notification_type | VARCHAR | Notification category |
| created_at | TIMESTAMP | Creation timestamp |

---

# 13. AuditLogs Table

## Purpose

Stores enterprise audit logs for compliance and tracking.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| entity_type | VARCHAR | Affected entity |
| entity_id | UUID | Entity identifier |
| action | VARCHAR | Performed action |
| performed_by | UUID | User performing action |
| metadata | JSONB | Additional audit metadata |
| created_at | TIMESTAMP | Event timestamp |

---

# 14. KnowledgeBaseDocuments Table

## Purpose

Stores uploaded enterprise knowledge documents.

---

## Supported File Types

- PDF
- DOCX
- TXT

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| tenant_id | UUID | Tenant isolation |
| file_name | VARCHAR | Original file name |
| file_type | VARCHAR | File type |
| storage_path | TEXT | File storage location |
| uploaded_by | UUID | FK to users |
| uploaded_at | TIMESTAMP | Upload timestamp |

---

# 15. DocumentChunks Table

## Purpose

Stores semantic document chunks for RAG retrieval.

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| document_id | UUID | FK to knowledge documents |
| chunk_text | TEXT | Document chunk |
| embedding_id | VARCHAR | Vector DB embedding reference |
| chunk_index | INTEGER | Chunk order index |
| created_at | TIMESTAMP | Creation timestamp |

---

# 16. SLAConfigurations Table

## Purpose

Stores SLA rules and escalation timing configurations.

---

## Example SLA Rules

| Priority | SLA |
|---|---|
| LOW | 48 Hours |
| MEDIUM | 24 Hours |
| HIGH | 4 Hours |
| CRITICAL | 1 Hour |

---

## Fields

| Field | Type | Description |
|---|---|---|
| id | UUID | Primary key |
| priority | VARCHAR | Ticket priority |
| response_time_minutes | INTEGER | Allowed response time |
| escalation_enabled | BOOLEAN | Escalation support |
| created_at | TIMESTAMP | Creation timestamp |

---

# 17. Relationships

## Entity Relationships

- One User → Many Tickets
- One Ticket → Many Comments
- One Ticket → Many Escalations
- One Ticket → Many Assignments
- One Document → Many Chunks
- One Role → Many Users

---

# 18. Indexing Strategy

Indexes will be created on:

- tenant_id
- ticket status
- priority
- assigned_to
- created_at
- sla_deadline
- document_id

This improves:
- filtering,
- dashboard queries,
- SLA tracking,
- semantic retrieval performance.

---

# 19. Future Enhancements

Future schema improvements may include:

- workflow templates,
- AI confidence scoring,
- sentiment analysis,
- vector metadata optimization,
- soft deletion,
- event sourcing,
- distributed audit streams.

---

# 20. Conclusion

The ResolveAI database architecture is designed using enterprise-grade principles to support scalability, multi-tenancy, workflow orchestration, AI integrations, semantic retrieval, auditability, and future distributed-system evolution.