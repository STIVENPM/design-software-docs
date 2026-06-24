# API - actors-service

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| GET | /api/v1/actors/{id} | - | `ActorDTO` | `actors:read` |
| GET | /api/v1/actors/available | `AvailabilityQuery` | `ActorDTO[]` | `actors:read` |
| POST | /api/v1/actors/{id}/availability | `AvailabilityRequest` | 201 | `actors:manage` |
