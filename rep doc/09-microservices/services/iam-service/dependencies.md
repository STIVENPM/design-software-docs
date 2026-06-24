# Dependencies - iam-service

## Dependencias externas

- No depende de otros microservicios críticamente para autenticación.
- Almacén de claves RSA (Vault) para firma y rotación de claves.

## Servicios que consumen `iam-service`

- `scheduling-service`, `actors-service`, `document-service`, `audit-service`.

## Consideraciones

- Mantener alta disponibilidad y baja latencia; desplegar en múltiples zonas.
