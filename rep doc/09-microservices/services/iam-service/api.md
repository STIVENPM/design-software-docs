# API - iam-service

## Endpoints REST

| Método | Ruta | Request DTO | Response DTO | Roles/Scopes |
|---|---|---|---|---|
| POST | /api/v1/auth/login | `AuthRequest` | `AuthResponse` | public |
| POST | /api/v1/auth/refresh | `RefreshRequest` | `AuthResponse` | public |
| POST | /api/v1/users | `CreateUserRequest` | `UserDTO` | `iam:manage` |
| GET | /api/v1/users/{id} | - | `UserDTO` | `iam:read` |
| PUT | /api/v1/users/{id}/roles | `AssignRolesRequest` | `UserDTO` | `iam:manage` |
| GET | /.well-known/jwks.json | - | JWKS | public |

## DTOs

- `CreateUserRequest { email, password, fullName }`
- `AssignRolesRequest { roles: string[] }`

## Códigos de error

- 400 Validation Error
- 401 Unauthorized
- 403 Forbidden
- 409 Conflict (email existente)
