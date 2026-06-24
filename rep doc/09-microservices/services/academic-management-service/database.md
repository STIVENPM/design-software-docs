# Database - academic-management-service

Tablas principales

| Tabla | PK | Campos |
|---|---|---|
| courses | id (UUID) | code, name, weekly_hours, credits, created_at |
| subjects | id (UUID) | course_id (FK), name |

Índices recomendados

- `CREATE UNIQUE INDEX idx_courses_code ON courses(code);`
- `CREATE INDEX idx_courses_name ON courses(name);`
