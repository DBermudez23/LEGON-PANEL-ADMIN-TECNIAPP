# TeleTrack — Panel de Administración

Panel web para la gestión de técnicos de campo de una empresa de telecomunicaciones. Construido para escalar junto a la app móvil existente y la infraestructura interna de la empresa.

---

## Stack tecnológico

- **Framework** — Next.js 15 (App Router)
- **Lenguaje** — TypeScript
- **Estilos** — Tailwind CSS
- **Estado** — Zustand (slices por feature)
- **HTTP** — Axios
- **Datos** — TanStack Query

---

## Arquitectura

El proyecto sigue una **estructura basada en features**, consistente con las convenciones usadas en la app móvil. Cada feature es autocontenida y gestiona sus propias llamadas a la API, componentes, slice de estado, hooks y tipos.

```
src/
├── app/                    # App Router de Next.js (rutas + capa BFF)
│   ├── (auth)/             # Rutas de autenticación
│   ├── (admin)/            # Rutas protegidas del panel
│   └── api/                # API Routes del servidor (BFF)
├── core/                   # Infraestructura compartida
│   ├── gateway/            # Cliente Axios → API Gateway
│   ├── auth/
│   ├── components/
│   ├── config/
│   ├── hooks/
│   ├── store/
│   └── utils/
├── features/               # Módulos por feature
│   ├── auth/
│   ├── technicians/
│   ├── dashboard/
│   └── users/
└── shared/                 # UI y utilidades transversales
```

### Patrón BFF

Las API Routes de Next.js (`src/app/api/`) actúan como proxy seguro entre el cliente React y el API Gateway central de la empresa. Las credenciales del servidor nunca llegan al navegador.

```
Cliente React → /app/api/route.ts → core/gateway → API Gateway
```

Ver [`src/app/api/README.md`](./src/app/api/README.md) y [`src/core/gateway/README.md`](./src/core/gateway/README.md) para más detalle.

---

## Inicio rápido

```bash
npm install
cp .env.example .env.local
npm run dev
```

---

## Variables de entorno

| Variable | Descripción |
|---|---|
| `GATEWAY_URL` | URL base del API Gateway central |
| `GATEWAY_SECRET` | API key para autenticación con el Gateway |
| `NEXT_PUBLIC_APP_NAME` | Nombre visible de la aplicación |

> Las variables sin el prefijo `NEXT_PUBLIC_` son exclusivas del servidor y nunca llegan al bundle del cliente.

---

## Relacionado

- [App móvil](../mobile) — App en Expo que comparte las mismas convenciones y API Gateway