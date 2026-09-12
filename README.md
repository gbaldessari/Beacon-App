# Beacon

Beacon es una aplicación personal para organizar el día a día: recordatorios, finanzas compartidas, notas y avisos. Este repositorio es el **monorepo de orquestación**: une el cliente web y la API, y levanta el stack completo con Docker Compose.

## Repositorios

| Repositorio | Rol |
| --- | --- |
| [Beacon-App](https://github.com/gbaldessari/Beacon-App) | Orquestación, Docker Compose y submódulos |
| [Beacon-Backend](https://github.com/gbaldessari/Beacon-Backend) | API NestJS + PostgreSQL |
| [Beacon-Frontend-Web](https://github.com/gbaldessari/Beacon-Frontend-Web) | Cliente web (React + Vite), instalable como PWA |

## Qué incluye

- **Tareas y calendarios** con recurrencia, invitaciones y avisos
- **Finanzas** por espacios (hogar o personal): movimientos, presupuestos, metas y etiquetas
- **Notas** con listas, colores, pines y etiquetas
- **Notificaciones** in-app y Web Push, más actualizaciones en tiempo real (Socket.IO)
- **Cuentas** con JWT, roles (admin / usuario) y recuperación de contraseña

## Stack

- Frontend: React 19, Vite, TypeScript
- Backend: NestJS 11, TypeORM, PostgreSQL 16
- Tiempo real: Socket.IO
- Contenedores: Docker Compose (Postgres, API y frontend con nginx)

## Requisitos

- Git
- Node.js 20+
- Docker Desktop (para el stack completo o solo la base de datos)

## Inicio rápido con Docker

```bash
git clone --recurse-submodules https://github.com/gbaldessari/Beacon-App.git
cd Beacon-App
```

Si el clon ya existe sin submódulos:

```bash
git submodule update --init --recursive
```

1. Copia las variables de entorno:

```bash
copy .env.example .env
copy Beacon-Backend\.env.example Beacon-Backend\.env
```

En macOS/Linux usa `cp` en lugar de `copy`.

2. Completa al menos `DB_PASSWORD` en `.env` y, en `Beacon-Backend/.env`, `JWT_SECRET`, `DB_PASSWORD` y el resto de secretos (correo, VAPID, admin inicial).

3. Levanta el stack:

```bash
docker compose up --build
```

- Frontend: [http://localhost:5173](http://localhost:5173)
- API: [http://localhost:3000](http://localhost:3000)
- PostgreSQL: `localhost:5433`

## Desarrollo local (sin Docker para la app)

Útil cuando quieres recarga en caliente. PostgreSQL sí puede ir en Docker.

```bash
# En Beacon-Backend
copy .env.example .env   # completar secretos
npm install
npm run db:up
npm run start:dev

# En Beacon-Frontend-Web
copy .env.example .env   # VITE_BACK_URL=http://localhost:3000
npm install
npm run dev
```

El frontend de Vite queda en [http://localhost:5173](http://localhost:5173) y habla con la API en el puerto 3000.

## Variables de entorno

La raíz solo necesita las variables de Compose (usuario, contraseña y nombre de Postgres, CORS y URL del API). Los secretos de la API viven en `Beacon-Backend/.env`. El detalle de cada clave está en:

- [`.env.example`](./.env.example)
- [`Beacon-Backend/.env.example`](./Beacon-Backend/.env.example)
- [`Beacon-Frontend-Web/.env.example`](./Beacon-Frontend-Web/.env.example)

En local, `DB_SYNC=true` y `REFRESH_COOKIE_SECURE=false`. En producción (HTTPS, por ejemplo Vercel + Railway) usa `DB_SYNC=false` y `REFRESH_COOKIE_SECURE=true`.

## Licencia

[MIT](./LICENSE) © 2026 Giacomo Baldessari
