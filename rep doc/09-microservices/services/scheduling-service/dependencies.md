# Dependencies - scheduling-service

## Consume

- `academic-management-service` (cursos y asignaturas)
- `training-environment-service` (disponibilidad de aulas)
- `actors-service` (disponibilidad de instructores/estudiantes)
- `reference-data-service` (datos de referencia)
- `iam-service` para validación de JWT

## Publica

- `schedule.requested`, `schedule.completed`.

## Consideraciones

- Aislar workers de optimización en pool separado; evento `schedule.completed` debe ser idempotente.
