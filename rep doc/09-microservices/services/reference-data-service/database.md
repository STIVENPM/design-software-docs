# Database - reference-data-service

## Tablas

| Tabla | PK | Campos clave |
|---|---|---|
| sites | id (UUID) | name, address, capacity, updated_at |
| shifts | id (UUID) | name, start_time, end_time |

## Índices

- `CREATE INDEX idx_sites_name ON sites(name);`
- `CREATE INDEX idx_shifts_range ON shifts(start_time, end_time);`

## Estrategia

- Versionado con `effective_from`/`effective_to` si se requiere historial.
