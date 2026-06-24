# Database - scheduling-service

## Tablas principales

| Tabla | PK | Campos |
|---|---|---|
| schedules | id (UUID) | program_id, term, status, created_at, updated_at, result JSONB |
| allocations | id (UUID) | schedule_id (FK), course_id, subject_id, instructor_id, environment_id, start_ts, end_ts |
| conflicts | id (UUID) | schedule_id, allocation_id, reason |
| schedule_jobs | id (UUID) | schedule_id, status, worker_id, started_at, completed_at |

## Índices recomendados

- `CREATE INDEX idx_schedules_program_term ON schedules(program_id, term);`
- `CREATE INDEX idx_allocations_instructor_time ON allocations(instructor_id, start_ts, end_ts);`

## Particionado y mantenimiento

- Particionar `allocations` por `schedule_id` o `term` para altas tasas de inserción.
