---
name: backend-db
description: Resolver trabajo de backend Spring Boot y base de datos PostgreSQL en NeoGaming. Usar cuando la tarea implique controllers, services, DTOs, entidades, repositorios, validaciones, autenticacion/autorizacion, contratos API, Flyway, SQL, seeds o diagnostico de arranque del backend. Activar ante pedidos como "revisa este endpoint", "alinea este DTO", "haz el seed", "corrige la migracion", "por que no levanta Spring" o "valida el contrato con frontend". No usar para maquetacion frontend o tareas de diseno visual.
---

# Backend / Database Skill

## En NeoGaming
- Leer primero el contrato real: controller, DTO, entity, migration y consumo desde frontend si aplica.
- Tratar Flyway como append-only y preferir SQL rerunnable para seeds.
- Si el usuario pide "desde la base de datos", responder contra tablas y columnas reales.
- Si Spring falla en PowerShell, usar sintaxis nativa como `$env:` y `Remove-Item Env:`.
- No inventar endpoints, columnas ni payloads que el repo no tenga.

## API Design Principles
- REST: use nouns for resources (`/users`, `/orders`), HTTP verbs for actions.
- HTTP status codes: 200 OK, 201 Created, 204 No Content, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity, 500 Internal Server Error.
- Always version APIs: `/api/v1/users`.
- Return consistent error shapes: `{ error: { code, message, details } }`.
- Paginate list endpoints: `{ data: [], meta: { total, page, limit } }`.

## Endpoint Implementation Checklist
- [ ] Input validation before any business logic.
- [ ] Authentication check (is user logged in?).
- [ ] Authorization check (does user have permission?).
- [ ] Business logic in a service layer, not in the route handler.
- [ ] Error handling with appropriate HTTP status.
- [ ] Response serialization — never expose raw DB rows.

## Database Schema Design
- Use UUIDs or auto-increment IDs consistently — pick one per project.
- Every table needs: `id`, `created_at`, `updated_at`.
- Normalize to 3NF by default. Denormalize only with measured performance justification.
- Foreign keys always have indexes.
- Use soft deletes (`deleted_at`) for user-facing data.
- Name tables in snake_case plural (`user_sessions`, `order_items`).

## Query Best Practices
- Select only needed columns — never `SELECT *` in production.
- Always use parameterized queries — never string-interpolate user input (SQL injection).
- Use `EXPLAIN ANALYZE` before shipping any query that touches >1000 rows.
- Add indexes on: foreign keys, columns used in WHERE/ORDER BY, unique constraints.
- Avoid N+1: use JOINs or eager loading instead of querying in a loop.

## Migrations
- Migrations are **append-only** — never edit a migration that has been run in production.
- Each migration does one thing: add table, add column, add index.
- Every `up` migration has a matching `down` migration.
- Test rollback locally before deploying.
- Never drop columns in the same migration that removes code using them — do it in the next deploy.

## Authentication / Authorization
- Hash passwords with bcrypt (cost factor 12+). Never store plain text.
- Use short-lived JWTs (15min access token, 7-day refresh token).
- Store refresh tokens in the DB so they can be revoked.
- Validate all tokens server-side on every request.
- Use middleware for auth checks — never duplicate auth logic in route handlers.

## Performance
- Cache expensive queries with Redis (TTL based on data freshness requirements).
- Use database connection pooling (pg-pool, Prisma, etc.).
- Background jobs for anything that takes >200ms (emails, image processing, webhooks).
- Log slow queries (>100ms) and set up alerts.

## Error Handling
- Catch errors at the service layer, map to domain errors.
- Route handler catches domain errors, maps to HTTP status codes.
- Never let unhandled promise rejections crash the server.
- Always log the original error before re-throwing or transforming.
