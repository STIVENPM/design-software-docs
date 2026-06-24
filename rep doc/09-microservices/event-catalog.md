# Catálogo de Eventos

Tabla centralizada de eventos publicados y consumidos por servicio. Cada evento incluye versión, payload resumido y propósito.

| Event | Versión | Publicador | Consumidores | Propósito |
|---|---:|---|---|---|
| user.created | v1 | iam-service | actors-service, audit-service | Notificar nueva identidad registrada |
| reference.updated | v1 | reference-data-service | scheduling-service, academic-management-service | Propagar cambios en datos de referencia |
| course.created | v1 | academic-management-service | scheduling-service, document-service | Nuevo curso disponible |
| environment.updated | v1 | training-environment-service | scheduling-service | Cambios en aulas o capacidad |
| schedule.requested | v1 | scheduling-service | monitoring-service, audit-service | Inicia flujo de generación de horarios |
| schedule.completed | v1 | scheduling-service | actors-service, document-service, audit-service | Indica finalización del proceso de planificación |
| actor.updated | v1 | actors-service | scheduling-service, academic-management-service | Actualización de profesores/estudiantes |
| document.generated | v1 | document-service | audit-service, monitoring-service | Notificación de generación de documento |
| metric.alert | v1 | monitoring-service | audit-service, operations | Alertas operativas críticas |
| audit.recorded | v1 | audit-service | monitoring-service | Confirmación de registro de auditoría |

Eventos deben ser diseñados para idempotencia y contener metadatos de correlación.
# Catálogo de eventos del sistema

> Estado: 🟡 En progreso | Última actualización: 2026-06-24
> Autor: Arquitectura de documentación técnica

## Propósito

Este documento centraliza el catálogo de eventos utilizados por el ecosistema de microservicios del proyecto **Sistema de Gestión de Horarios SENA**.

El objetivo es garantizar que la producción, consumo y evolución de los eventos sea consistente con la arquitectura REST y basada en eventos del sistema. El catálogo sirve como referencia para los equipos de backend, integración, QA, operaciones y arquitectura.

## Alcance

Incluye eventos de dominio, eventos de orquestación, notificaciones de auditoría, sincronización de datos y generación de documentos.

Aplica a todos los servicios de `09-microservices/` y a las dependencias de eventos que se derivan de los flujos de negocio definidos en el proyecto.

## Contexto

La arquitectura principal del sistema es:

- **IAM → todos los servicios**
- **Reference Data → Academic Management → Actors → Scheduling → Monitoring → Audit**
- `Document Service` se consume desde `Scheduling`, `Academic Management`, `Actors` y `Monitoring`
- `Audit Service` consume eventos de todos los servicios para garantizar trazabilidad append-only

El evento central del dominio es el generado por `05-scheduling-service`, que refleja la planificación y la asignación de horarios.

## Convenciones de eventos

- `event.type`: formato `dominio.accion` (por ejemplo, `iam.user.created`).
- `producer`: servicio que publica el evento.
- `consumer`: servicio(s) que consumen el evento.
- `guarantee`: garantía de entrega (`at-least-once`, `exactly-once`, `best-effort`).
- `schema.version`: versión del payload cuando el evento evoluciona.
- No incluir datos sensibles en ninguna carga del evento.

## Catálogo de eventos

| Evento | Productor | Consumidor(es) | Descripción | Garantía | Observaciones |
|-------|-----------|----------------|-------------|----------|---------------|
| `iam.user.created` | `iam-service` | `audit-service`, `actors-service`, `reference-data-service` | Usuario creado con rol y permisos iniciales. | `at-least-once` | Actualiza la sincronización de identidad. |
| `iam.user.updated` | `iam-service` | `audit-service`, `actors-service`, `reference-data-service` | Actualización de datos, roles o permisos de usuario. | `at-least-once` | Debe propagar cambios de acceso. |
| `iam.token.revoked` | `iam-service` | `audit-service` | Token de sesión revocado. | `at-least-once` | Soporta invalidación de sesión. |
| `reference.catalog.created` | `reference-data-service` | `academic-management-service`, `scheduling-service` | Nuevo catálogo institucional creado. | `at-least-once` | Incluye macroregión o centro de formación. |
| `reference.catalog.updated` | `reference-data-service` | `academic-management-service`, `actors-service`, `scheduling-service` | Actualización de catálogo o parámetro maestro. | `at-least-once` | Usado para validar datos maestros. |
| `academic.program.created` | `academic-management-service` | `scheduling-service`, `actors-service`, `monitoring-service` | Programa académico creado. | `at-least-once` | Genera disponibilidad para horarios. |
| `academic.program.updated` | `academic-management-service` | `scheduling-service`, `actors-service`, `monitoring-service` | Modificación de programa, competencia o ficha. | `at-least-once` | Afecta reglas de asignación. |
| `actors.apprentice.enrolled` | `actors-service` | `scheduling-service`, `monitoring-service` | Aprendiz inscrito en un programa o ficha. | `at-least-once` | Dispara ajuste de capacidad. |
| `actors.instructor.assigned` | `actors-service` | `scheduling-service`, `monitoring-service` | Instructor asignado a sesión de clase. | `at-least-once` | Necesario para cálculo de horarios. |
| `training.environment.available` | `training-environment-service` | `scheduling-service`, `monitoring-service` | Ambiente actualizado como disponible. | `best-effort` | Actualiza la oferta de espacios. |
| `training.environment.booked` | `training-environment-service` | `scheduling-service`, `monitoring-service`, `audit-service` | Ambiente reservado correctamente. | `at-least-once` | Refleja ocupación real. |
| `training.environment.cancelled` | `training-environment-service` | `scheduling-service`, `monitoring-service` | Reserva cancelada. | `at-least-once` | Libera el ambiente para replanificación. |
| `scheduling.schedule.created` | `scheduling-service` | `actors-service`, `monitoring-service`, `document-service`, `audit-service` | Horario generado por el motor principal. | `at-least-once` | Evento central del core. |
| `scheduling.schedule.updated` | `scheduling-service` | `actors-service`, `monitoring-service`, `document-service`, `audit-service` | Actualización de un horario existente. | `at-least-once` | Incluye reprocesos y reasignaciones. |
| `scheduling.schedule.deleted` | `scheduling-service` | `actors-service`, `monitoring-service`, `audit-service` | Horario eliminado o invalidado. | `at-least-once` | Necesario para liberar recursos. |
| `scheduling.conflict.detected` | `scheduling-service` | `actors-service`, `monitoring-service`, `audit-service` | Conflicto en asignación de horario. | `at-least-once` | Dispara validación de conflicto. |
| `document.file.generated` | `document-service` | `audit-service`, `actors-service`, `monitoring-service` | Documento generado (PDF, plantilla, reporte). | `at-least-once` | Incluye metadatos y ubicación. |
| `document.template.updated` | `document-service` | `scheduling-service`, `academic-management-service`, `actors-service` | Plantilla de documento actualizada. | `best-effort` | Afecta generación futura. |
| `monitoring.alert.triggered` | `monitoring-service` | `audit-service`, `document-service` | Alerta de KPI u umbral superado. | `best-effort` | Puede iniciar generación de reporte. |
| `monitoring.notification.sent` | `monitoring-service` | `actors-service`, `document-service` | Notificación enviada a destino. | `best-effort` | Cadena de notificación de eventos. |
| `audit.event.logged` | `audit-service` | Ninguno | Registro inmutable de evento de auditoría. | `exactly-once` | Base para cumplimiento y evidencia. |

## Reglas de diseño de eventos

1. El nombre del evento debe describir el dominio y la acción: `dominio.accion`.
2. Las cargas (`payload`) deben ser planas y versionables.
3. Cada evento debe tener `producer`, `consumer`, `schema.version` y `trace_id`.
4. No transportar datos sensibles o personales; usar referencias y tokens cuando se requiera.
5. Definir el tipo de garantía de entrega en el catálogo.
6. Los eventos de `audit-service` deben ser append-only y no deben modificarse una vez registrados.

## Clasificación de eventos

- **Eventos de identidad:** `iam.user.created`, `iam.user.updated`, `iam.token.revoked`.
- **Eventos de catálogo:** `reference.catalog.created`, `reference.catalog.updated`.
- **Eventos académicos:** `academic.program.created`, `academic.program.updated`.
- **Eventos de actores:** `actors.apprentice.enrolled`, `actors.instructor.assigned`.
- **Eventos de ambientes:** `training.environment.available`, `training.environment.booked`, `training.environment.cancelled`.
- **Eventos de horario:** `scheduling.schedule.created`, `scheduling.schedule.updated`, `scheduling.schedule.deleted`, `scheduling.conflict.detected`.
- **Eventos documentales:** `document.file.generated`, `document.template.updated`.
- **Eventos de monitoreo:** `monitoring.alert.triggered`, `monitoring.notification.sent`.
- **Eventos de auditoría:** `audit.event.logged`.

## Mantenimiento del catálogo

- El catálogo se debe actualizar cada vez que se defina un evento nuevo o se modifique un payload.
- La versión del esquema debe mantener compatibilidad hacia atrás siempre que sea posible.
- Los eventos críticos del core `05-scheduling-service` deben revisarse en prioridad alta.

## Próximos pasos

- Validar la estructura de payload con los equipos de backend de cada servicio.
- Completar los esquemas de evento en `09-microservices/` y en los contratos de API.
- Sincronizar los nombres de eventos con `dependency-map.md`, `data-ownership-matrix.md` y `service-boundary-rules.md`.
