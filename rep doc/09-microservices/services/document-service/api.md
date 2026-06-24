# API - document-service

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| POST | /api/v1/documents/generate | `GenerateRequest` | 202 `{ documentId }` | `document:generate` |
| GET | /api/v1/documents/{id} | - | `DocumentDTO` | `document:read` |
| GET | /api/v1/documents/{id}/download | - | 302 Redirect (signed URL) | `document:read` |

DTOs: `GenerateRequest { sourceService, entityId, templateId }`
