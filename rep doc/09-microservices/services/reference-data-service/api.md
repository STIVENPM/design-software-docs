# API - reference-data-service

## Endpoints

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| GET | /api/v1/sites | - | `SiteDTO[]` | `reference:read` |
| POST | /api/v1/sites | `CreateSiteRequest` | `SiteDTO` | `reference:manage` |
| PUT | /api/v1/sites/{id} | `UpdateSiteRequest` | `SiteDTO` | `reference:manage` |
| GET | /api/v1/shifts | - | `ShiftDTO[]` | `reference:read` |

## DTOs

- `SiteDTO { id, name, address, capacity }`
- `ShiftDTO { id, name, start, end }`
