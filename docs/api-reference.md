# API Reference

Base URL: `https://api.aiml.chat`

All dashboard endpoints require a JWT `Authorization: Bearer <token>` header.  
Widget endpoints use `X-Api-Key: aiml_pk_...` instead.

---

## Authentication

### Register

```
POST /v1/auth/register
```

```json
{
  "email": "you@example.com",
  "password": "securepassword",
  "name": "Your Name"
}
```

**Response 201**
```json
{
  "message": "Registration successful. Please verify your email."
}
```

---

### Login

```
POST /v1/auth/login
```

```json
{ "email": "you@example.com", "password": "securepassword" }
```

**Response 200**
```json
{
  "accessToken": "eyJ...",
  "refreshToken": "...",
  "tenantId": "uuid"
}
```

Access tokens expire in **15 minutes**. Use the refresh endpoint to obtain new ones.

---

### Refresh token

```
POST /v1/auth/refresh
```

```json
{ "refreshToken": "..." }
```

**Response 200**
```json
{ "accessToken": "eyJ...", "refreshToken": "..." }
```

---

## Websites

### List websites

```
GET /v1/websites
Authorization: Bearer <token>
```

**Response 200**
```json
[
  {
    "id": "uuid",
    "domain": "docs.example.com",
    "ingestionStatus": "Ready",
    "totalChunks": 1482,
    "apiKeyPrefix": "aiml_pk_ab12"
  }
]
```

---

### Register website

```
POST /v1/websites
Authorization: Bearer <token>
```

```json
{ "domain": "docs.example.com" }
```

**Response 201**
```json
{
  "id": "uuid",
  "domain": "docs.example.com",
  "apiKey": "aiml_pk_...",
  "ingestionStatus": "Pending"
}
```

> **Important:** The full API key is returned only once at creation. Store it securely.

---

### Get website status

```
GET /v1/websites/{id}/status
Authorization: Bearer <token>
```

**Response 200**
```json
{
  "id": "uuid",
  "domain": "docs.example.com",
  "ingestionStatus": "Ready",
  "totalChunks": 1482,
  "lastCrawledAt": "2025-05-15T10:30:00Z",
  "nextCrawlAt": "2025-05-22T10:30:00Z",
  "crawlSchedule": "weekly"
}
```

`ingestionStatus` values: `Pending`, `InProgress`, `Ready`, `Failed`

---

### Trigger ingestion

```
POST /v1/websites/{id}/ingest
Authorization: Bearer <token>
```

```json
{ "startUrl": "https://docs.example.com" }
```

Ingestion runs in the background. Poll `/status` to check progress.

**Response 202**
```json
{ "message": "Ingestion started." }
```

---

## Chat

### Send message (widget)

```
POST /v1/chat
X-Api-Key: aiml_pk_...
Content-Type: application/json
```

```json
{
  "message": "How do I reset my password?",
  "conversationId": "uuid-or-null",
  "visitorId": "v-abc123",
  "history": [
    { "role": "user", "content": "Hi" },
    { "role": "assistant", "content": "Hello! How can I help?" }
  ]
}
```

**Response: Server-Sent Events stream**

```
data: {"token":"Here"}
data: {"token":" is"}
data: {"token":" how"}
...
data: {"citations":[{"sourceUrl":"https://...","title":"Reset password"}]}
data: [DONE]
```

**Error responses:**
- `401` — Invalid API key
- `402` — Monthly quota exceeded
- `404` — No content indexed yet
- `429` — Rate limit hit (`Retry-After` header included)

---

## Analytics

All analytics endpoints require JWT auth and are scoped to the authenticated tenant.

### Top questions

```
GET /v1/analytics/{websiteId}/top-questions?days=30&limit=20
Authorization: Bearer <token>
```

**Response 200**
```json
[
  { "query": "How do I install the plugin?", "count": 47, "topic": null },
  { "query": "What is the free tier limit?", "count": 31, "topic": null }
]
```

`days` must be `7`, `30`, or `90`.

---

### Unanswered queries

```
GET /v1/analytics/{websiteId}/unanswered?days=30&limit=20
Authorization: Bearer <token>
```

**Response 200**
```json
[
  {
    "query": "Does it support Arabic?",
    "occurredAt": "2025-05-14T08:22:00Z",
    "topic": null
  }
]
```

---

### Usage trends

```
GET /v1/analytics/{websiteId}/usage?days=30
Authorization: Bearer <token>
```

**Response 200**
```json
{
  "dailyCounts": [
    { "date": "2025-05-01", "messages": 42, "conversations": 18 }
  ],
  "totalMessages": 1240,
  "totalConversations": 530
}
```

---

### Export CSV

```
GET /v1/analytics/{websiteId}/export?days=30
Authorization: Bearer <token>
```

Returns `text/csv` with `Content-Disposition: attachment; filename="aiml-analytics-*.csv"`.

Columns: `Timestamp, Query, Topic, WasAnswered, ConversationId`

---

## Lead Capture

### Capture lead

```
POST /v1/leads
X-Api-Key: aiml_pk_...
Content-Type: application/json
```

```json
{
  "websiteId": "uuid",
  "email": "visitor@example.com",
  "question": "Do you support Arabic?",
  "visitorId": "v-abc123"
}
```

**Response 201**
```json
{ "id": "uuid", "email": "visitor@example.com" }
```

The site owner receives an email notification via Resend.

---

## Billing

### Create checkout

```
POST /v1/billing/checkout
Authorization: Bearer <token>
```

```json
{ "plan": "pro" }
```

**Response 200**
```json
{ "checkoutUrl": "https://checkout.paddle.com/..." }
```

`plan` values: `pro`, `agency`, `business`

---

### Get subscription

```
GET /v1/billing/subscription
Authorization: Bearer <token>
```

**Response 200**
```json
{
  "plan": "Pro",
  "status": "active",
  "currentPeriodEnd": "2025-06-15T00:00:00Z",
  "cancelAtPeriodEnd": false
}
```

---

### Customer portal

```
POST /v1/billing/portal
Authorization: Bearer <token>
```

**Response 200**
```json
{ "portalUrl": "https://customer.paddle.com/..." }
```

---

## GDPR / Account

### Delete conversation

```
DELETE /v1/conversations/{id}
Authorization: Bearer <token>
```

**Response 204** — All messages in the conversation are deleted immediately.

---

### Delete account

```
DELETE /v1/account
Authorization: Bearer <token>
```

**Response 200**
```json
{
  "message": "Account deletion scheduled. Your data will be permanently deleted in 30 days.",
  "scheduledDeletionAt": "2025-06-29T00:00:00Z"
}
```

A confirmation email is sent. All data is hard-deleted after the 30-day grace period.

---

## Error format

All errors follow [RFC 7807 Problem Details](https://www.rfc-editor.org/rfc/rfc7807):

```json
{
  "type": "https://aiml.chat/errors/quota_exceeded",
  "title": "Monthly quota exceeded",
  "status": 402,
  "detail": "You have used 100/100 messages this month. Upgrade to continue."
}
```

Common `type` values:

| Type | Status | Description |
|------|--------|-------------|
| `quota_exceeded` | 402 | Monthly message quota reached |
| `rate_limited` | 429 | Too many requests from this IP |
| `invalid_api_key` | 401 | API key missing or invalid |
| `not_found` | 404 | Resource not found |
| `validation_error` | 422 | Request body validation failed |
