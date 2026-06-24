# Catálogo de Servicios

Tabla de microservicios principales del Sistema de Gestión de Horarios SENA.

| Servicio | Objetivo principal | Responsabilidades clave | Owner sugerido |
|---|---:|---|---|
| iam-service | Gestión de identidad y acceso | Autenticación JWT, gestión de usuarios, roles y scopes | Seguridad / IAM |
| reference-data-service | Catálogo de datos de referencia | Países, sedes, turnos, tipologías de formación | Plataforma |
| academic-management-service | Gestión curricular | Cursos, programas, asignaturas, horarios académicos base | Académico |
| training-environment-service | Gestión de entornos de formación | Aulas, laboratorios, recursos físicos y virtuales | Infraestructura |
| scheduling-service | Planificación y generación de horarios (core) | Creación, optimización, asignación de turnos y colisiones | Producto |
| actors-service | Gestión de actores | Estudiantes, instructores, administradores, sus relaciones | Registro |
| document-service | Almacenamiento y generación de documentos | PDFs, evidencias, reportes, control de versiones | Documentación |
| monitoring-service | Observabilidad y métricas | Métricas, alertas, trazas distribuidas | Operaciones |
| audit-service | Registro de auditoría | Trazabilidad de acciones críticas, retención de logs | Cumplimiento |

Cada servicio tiene contrato API, modelo de datos PostgreSQL y un conjunto de eventos para integración asíncrona. El `scheduling-service` actúa como orquestador del dominio de horarios mediante eventos de dominio y colas para procesos largos.
# Catálogo de servicios

| Servicio | Descripción | Owner | Repo | Estado |
|----------|-------------|-------|------|--------|
