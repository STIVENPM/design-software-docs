# Database - iam-service

## Tablas

| Tabla | PK | Campos clave |
|---|---|---|
| users | id (UUID) | email (unique), full_name, password_hash, status, created_at |
| roles | id (UUID) | name (unique), description |
| user_roles | id | user_id (FK), role_id (FK) |
| token_revocation | jti | jti, expires_at |

## Índices recomendados

- `CREATE UNIQUE INDEX idx_users_email ON users(email);`
- `CREATE INDEX idx_user_roles_user ON user_roles(user_id);`

## Estrategia de migraciones

- Flyway con scripts por release; copias de seguridad antes de migraciones que modifican hashes o formatos de token.
