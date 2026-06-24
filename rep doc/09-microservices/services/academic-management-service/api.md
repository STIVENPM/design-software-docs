# API - academic-management-service

| Método | Ruta | Request | Response | Roles |
|---|---|---|---|---|
| GET | /api/v1/courses | - | `CourseDTO[]` | `academic:read` |
| POST | /api/v1/courses | `CreateCourseRequest` | `CourseDTO` | `academic:manage` |
| PUT | /api/v1/courses/{id} | `UpdateCourseRequest` | `CourseDTO` | `academic:manage` |

DTOs
- `CreateCourseRequest { code, name, weeklyHours, credits }`
