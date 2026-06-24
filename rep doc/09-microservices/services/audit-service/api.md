# API - audit-service

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| POST | /api/v1/audit | `AuditRecordRequest` | 201 | `audit:write` |
| GET | /api/v1/audit?resourceType=.. | - | `AuditRecord[]` | `audit:read` |
