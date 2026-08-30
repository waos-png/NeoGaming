# NeoGaming  — Marketplace Gaming Colombia

Plataforma de comercio electrónico de videojuegos y periféricos para el mercado colombiano.

## Tech Stack

| Capa | Tecnologías |
|------|-------------|
| **Frontend** | Angular 21 · TypeScript 5.9 · NgRx · TailwindCSS 4 · SSR |
| **Backend** | Spring Boot 4.0.6 · Java 21 · PostgreSQL 17 · Redis |
| **IA** | FastAPI (servicio de IA) |
| **Pagos** | Mercado Pago |

## Estructura del proyecto

```
NeoGaming/
├── Backend-NeoGaming/     # API REST (Spring Boot) — repo independiente
├── Frontend-NeoGaming/    # SPA + SSR (Angular) — repo independiente
├── .gitignore
└── README.md
```

> ⚠️ **Nota:** Cada subcarpeta (`Backend-NeoGaming`, `Frontend-NeoGaming`) tiene su **propio repositorio Git** para despliegue independiente. Este repo raíz sirve como contenedor organizacional.

## Repositorios

| Proyecto | Repositorio |
|----------|-------------|
| **Raíz (este repo)** | [waos-png/NeoGaming](https://github.com/waos-png/NeoGaming) |
| **Backend** | *(enlazado en su propia carpeta)* |
| **Frontend** | *(enlazado en su propia carpeta)* |

## Requisitos previos

| Herramienta | Versión mínima |
|-------------|----------------|
| Java | 21 |
| Node.js | 20+ |
| npm | 10+ |
| PostgreSQL | 17 |
| Docker | (opcional) |

## Quick Start

```bash
# Backend
cd Backend-NeoGaming
./dev.sh

# Frontend
cd Frontend-NeoGaming
npm install
npm start
```

## Licencia

Proyecto privado — Todos los derechos reservados.
