# Database - training-environment-service

Tablas

| Tabla | PK | Campos |
|---|---|---|
| environments | id (UUID) | name, capacity, type, features JSONB, updated_at |
| environment_availability | id | environment_id, date, start_time, end_time, status |

Índices

- `CREATE INDEX idx_env_type ON environments(type);`
- `CREATE INDEX idx_availability_env_date ON environment_availability(environment_id, date);`
