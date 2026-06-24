# API - training-environment-service

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| GET | /api/v1/environments/available | `AvailabilityQuery` | `EnvironmentDTO[]` | `training:read` |
| POST | /api/v1/environments | `CreateEnvironment` | `EnvironmentDTO` | `training:manage` |

DTOs: `EnvironmentDTO { id, name, capacity, type, features }`
