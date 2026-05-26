# API Specification

# ResolveAI — API Specification Document

# 1. Overview

This document defines the REST API architecture for ResolveAI.

The APIs are designed following:
- REST principles,
- modular architecture,
- scalable API design,
- secure authentication,
- enterprise-grade response handling.

Base URL:
/api/v1

---

# 2. Authentication APIs

## Register User

POST /auth/register

### Request Body

{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "Password123"
}

### Response

{
  "success": true,
  "message": "User registered successfully"
}

---

## Login User

POST /auth/login

### Request Body

{
  "email": "john@example.com",
  "password": "Password123"
}

### Response

{
  "access_token": "",
  "refresh_token": "",
  "token_type": "bearer"
}

---

## Refresh Token

POST /auth/refresh

---

# 3. User APIs

## Get Current User

GET /users/me

---

## Get All Users

GET /users

---

## Update User

PUT /users/{id}

---

# 4. Ticket APIs

## Create Ticket

POST /tickets

### Request Body

{
  "title": "VPN Access Issue",
  "description": "Unable to connect to company VPN",
  "priority": "HIGH"
}

### Response

{
  "success": true,
  "ticket_id": ""
}

---

## Get All Tickets

GET /tickets

---

## Get Ticket By ID

GET /tickets/{id}

---

## Update Ticket

PUT /tickets/{id}

---

## Delete Ticket

DELETE /tickets/{id}

---

# 5. Ticket Comment APIs

## Add Comment

POST /tickets/{id}/comments

---

## Get Ticket Comments

GET /tickets/{id}/comments

---

# 6. Workflow APIs

## Update Ticket Status

POST /workflows/tickets/{id}/status

---

## Escalate Ticket

POST /workflows/tickets/{id}/escalate

---

## Approve Workflow

POST /workflows/{id}/approve

---

# 7. Escalation APIs

## Get Escalation History

GET /tickets/{id}/escalations

---

## Manual Escalation

POST /tickets/{id}/escalate

---

# 8. Notification APIs

## Get Notifications

GET /notifications

---

## Mark Notification As Read

PUT /notifications/{id}/read

---

# 9. AI APIs

## AI Ticket Classification

POST /ai/classify

### Request

{
  "query": "VPN not working"
}

### Response

{
  "department": "IT",
  "priority": "HIGH"
}

---

## AI Query Resolution

POST /ai/query

### Request

{
  "query": "How to reset VPN password?"
}

### Response

{
  "response": "Follow these steps..."
}

---

# 10. Knowledge Base APIs

## Upload Document

POST /documents/upload

---

## Get Documents

GET /documents

---

## Delete Document

DELETE /documents/{id}

---

# 11. Analytics APIs

## Dashboard Metrics

GET /analytics/dashboard

---

## Ticket Statistics

GET /analytics/tickets

---

# 12. Standard Response Format

{
  "success": true,
  "message": "",
  "data": {}
}

---

# 13. Standard Error Format

{
  "success": false,
  "error": {
    "code": "",
    "message": ""
  }
}

---

# 14. Authentication Strategy

Protected APIs require:
Authorization: Bearer <token>

---

# 15. API Security

Security measures include:
- JWT authentication
- RBAC authorization
- request validation
- rate limiting
- tenant isolation

---

# 16. Versioning Strategy

Current API version:
v1

Future versions:
- /api/v2
- /api/v3

---

# 17. API Documentation

Interactive API documentation provided using:
- Swagger UI
- OpenAPI

---

# 18. Conclusion

The ResolveAI API architecture is designed to support scalable enterprise workflows, secure authentication, AI integrations, semantic retrieval, workflow orchestration, and future extensibility.