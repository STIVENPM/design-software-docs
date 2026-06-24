# Database - audit-service

Tablas

| Tabla | PK | Campos |
|---|---|---|
| audit_records | id (UUID) | actor_id, action, resource_type, resource_id, details JSONB, timestamp |

Índices recomendados

- `CREATE INDEX idx_audit_resource ON audit_records(resource_type, resource_id);`
- `CREATE INDEX idx_audit_actor_time ON audit_records(actor_id, timestamp);`

Retención y archivado

- Archivar a almacenamiento frío después de periodo configurable (p.ej. 2 años) y mantener índices resumidos.
