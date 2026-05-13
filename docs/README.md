# NeoGaming

NeoGaming is a gaming-focused ecommerce marketplace with customer, seller, and administrator workflows. The project combines an Angular storefront/admin UI with a Spring Boot REST API backed by PostgreSQL.

## High-Level Architecture

The `frontend` application is an Angular standalone-component app that calls the backend through a centralized `ApiClient` configured by `apiBaseUrl`. It handles routing, UI state, JWT persistence, auth guards, cart/wishlist UX state, checkout screens, and optional Angular SSR through an Express server.

The `backend` application is a Spring Boot modular monolith. It exposes REST endpoints under `/api`, validates requests with DTOs, secures most routes with JWT bearer authentication, persists data with Spring Data JPA, applies schema changes with Flyway, and sends transactional emails through SMTP when enabled.

## Tech Stack

### Frontend

- Angular `^21.2.0`
- Angular SSR `^21.2.0`
- TypeScript `~5.9.2`
- RxJS `~7.8.0`
- Tailwind CSS `^4.1.12`
- PostCSS `^8.5.3`
- Lucide / `lucide-angular`
- Express `^5.1.0` for SSR runtime
- Vitest `^4.0.8` and jsdom for tests
- Vercel static deployment config
- Docker / Docker Compose

### Backend

- Java 21
- Spring Boot `4.0.5`
- Spring Web
- Spring Security
- Spring Data JPA
- Spring Validation
- Spring Actuator
- Spring Mail
- PostgreSQL driver
- Flyway migrations
- JJWT `0.12.6`
- Lombok `1.18.38`
- Springdoc OpenAPI `3.0.0`
- Maven Wrapper
- Docker / Docker Compose

## Project Structure

```text
.
|-- .github/
|-- backend/
|   |-- compose.yaml
|   |-- Dockerfile
|   |-- mvnw
|   |-- mvnw.cmd
|   |-- pom.xml
|   |-- postman/
|   |   |-- Proyecto_Formativo_Ecommerce.postman_collection.json
|   |   |-- Proyecto_Formativo_Ecommerce.postman_environment.json
|   |-- src/
|   |   |-- main/
|   |   |   |-- java/com/neogamin/proyecto_formativo/
|   |   |   |   |-- admin/
|   |   |   |   |-- analitica/
|   |   |   |   |-- carrito/
|   |   |   |   |-- catalogo/
|   |   |   |   |-- checkout/
|   |   |   |   |-- compartido/
|   |   |   |   |-- facturacion/
|   |   |   |   |-- interaccion/
|   |   |   |   |-- inventario/
|   |   |   |   |-- notificacion/
|   |   |   |   |-- pago/
|   |   |   |   |-- pedido/
|   |   |   |   |-- resena/
|   |   |   |   |-- usuario/
|   |   |   |   |-- ProyectoFormativoApplication.java
|   |   |   |-- resources/
|   |   |       |-- application.yml
|   |   |       |-- db/migration/
|   |   |-- test/
|   |-- README.md
|-- frontend/
|   |-- angular.json
|   |-- Dockerfile
|   |-- docker-compose.yml
|   |-- package.json
|   |-- package-lock.json
|   |-- tailwind.config.js
|   |-- tsconfig*.json
|   |-- vercel.json
|   |-- docs/
|   |-- public/
|   |-- src/
|   |   |-- app/
|   |   |   |-- core/
|   |   |   |-- features/
|   |   |   |-- shared/
|   |   |   |-- app.config.ts
|   |   |   |-- app.routes.ts
|   |   |-- environments/
|   |   |-- main.ts
|   |   |-- main.server.ts
|   |   |-- server.ts
|   |   |-- styles.css
|   |-- README.md
|-- docs/
|   |-- README.md
|   |-- FRONTEND.md
|   |-- BACKEND.md
|-- PHASE6_QA_ADMIN_REPORT.md
```

## Run Locally

### Backend

Requirements:

- Java 21
- Docker Desktop
- Maven Wrapper from the repository

Install/start:

```powershell
cd backend
docker compose up -d
.\mvnw.cmd spring-boot:run
```

Run tests:

```powershell
cd backend
.\mvnw.cmd test
```

Useful local URLs:

- API: `http://localhost:8080`
- Swagger UI: `http://localhost:8080/swagger-ui/index.html`
- OpenAPI JSON: `http://localhost:8080/v3/api-docs`
- Health: `http://localhost:8080/actuator/health`

### Frontend

Requirements:

- Node.js 20+
- npm 10+
- Backend running on `http://localhost:8080`

Install/start:

```powershell
cd frontend
npm ci
npm start
```

Run tests:

```powershell
cd frontend
npm test -- --watch=false
```

Build:

```powershell
cd frontend
npm run build
```

Optional SSR runtime after build:

```powershell
cd frontend
npm run start:ssr
```

## Environment Variables

`.env` files: _Not detected_

Runtime environment keys referenced by code/configuration:

| Key | Used by |
| --- | --- |
| `PORT` | Backend server port and frontend SSR/Docker runtime |
| `DB_URL` | Backend PostgreSQL JDBC URL |
| `DB_USERNAME` | Backend PostgreSQL user |
| `DB_PASSWORD` | Backend PostgreSQL password |
| `MAIL_HOST` | Backend SMTP host |
| `MAIL_PORT` | Backend SMTP port |
| `MAIL_USERNAME` | Backend SMTP username |
| `MAIL_PASSWORD` | Backend SMTP password |
| `MAIL_SMTP_AUTH` | Backend SMTP auth toggle |
| `MAIL_SMTP_STARTTLS_ENABLE` | Backend SMTP STARTTLS enable toggle |
| `MAIL_SMTP_STARTTLS_REQUIRED` | Backend SMTP STARTTLS required toggle |
| `MAIL_SMTP_SSL_ENABLE` | Backend SMTP SSL toggle |
| `MAIL_SMTP_CONNECTION_TIMEOUT` | Backend SMTP connection timeout |
| `MAIL_SMTP_TIMEOUT` | Backend SMTP timeout |
| `MAIL_SMTP_WRITE_TIMEOUT` | Backend SMTP write timeout |
| `SPRINGDOC_API_DOCS_ENABLED` | Backend OpenAPI docs toggle |
| `SPRINGDOC_SWAGGER_UI_ENABLED` | Backend Swagger UI toggle |
| `APP_CORS_ORIGIN_1` | Backend CORS allowed origin |
| `APP_CORS_ORIGIN_2` | Backend CORS allowed origin |
| `JWT_SECRET` | Backend JWT signing secret |
| `JWT_EXPIRATION_MINUTES` | Backend JWT expiration window |
| `APP_NOTIFICACION_EMAIL_HABILITADO` | Backend transactional email toggle |
| `APP_NOTIFICACION_EMAIL_REMITENTE` | Backend transactional email sender |
| `POSTGRES_DB` | Docker Compose PostgreSQL database name |
| `POSTGRES_USER` | Docker Compose PostgreSQL user |
| `POSTGRES_PASSWORD` | Docker Compose PostgreSQL password |
| `NODE_ENV` | Frontend SSR/Docker runtime |
| `START_SERVER` | Frontend SSR server startup gate |
| `HOST` | Frontend SSR bind host |
| `NG_ALLOWED_HOSTS` | Frontend SSR allowed hosts list |
| `RAILWAY_PUBLIC_DOMAIN` | Frontend SSR Railway allowed host |
