# Database - document-service

Tablas

| Tabla | PK | Campos |
|---|---|---|
| documents | id (UUID) | owner_id, type, storage_path, checksum, size, created_at |
| document_versions | id | document_id, version_number, storage_path, created_at |

Índices

- `CREATE INDEX idx_documents_owner ON documents(owner_id);`
