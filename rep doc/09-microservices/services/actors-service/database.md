# Database - actors-service

Tablas

| Tabla | PK | Campos |
|---|---|---|
| actors | id (UUID) | type, full_name, email, qualifications JSONB |
| availability | id | actor_id, date, start_ts, end_ts |

Índices

- `CREATE INDEX idx_avail_actor_date ON availability(actor_id, date);`
