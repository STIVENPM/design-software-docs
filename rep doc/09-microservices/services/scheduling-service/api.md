# API - scheduling-service

## Contrato

Endpoints principales detallados a continuación.

| Método | Ruta | Request DTO | Response DTO | Códigos |
|---|---|---|---|---|
| POST | /api/v1/schedules | `ScheduleRequest` | 202 `{ scheduleId }` | 202,400,401,403 |
| GET | /api/v1/schedules/{id} | - | `ScheduleStatus` | 200,404 |
| GET | /api/v1/schedules/{id}/result | - | `ScheduleResultDTO` | 200,404 |
| GET | /api/v1/schedules?programId=.. | - | `ScheduleSummary[]` | 200 |

## DTOs

- `ScheduleRequest { programId, term, constraints[], optimizationGoals[] }`
- `ScheduleResultDTO { scheduleId, allocations[], metadata }`

## Seguridad

- JWT requerido; roles `schedule:write`, `schedule:read`.
