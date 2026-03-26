# Rutas de API (Capa BFF)

Esta carpeta contiene handlers de rutas de Next.js (`route.ts`) que actúan como la capa Backend for Frontend (BFF) entre el cliente React y el API Gateway central de la empresa.

## Decisión técnica

En una SPA estándar de React, las variables de entorno se empaquetan en el cliente y quedan expuestas en el navegador. Este proyecto usa Next.js API Routes como un proxy seguro del lado del servidor para evitar completamente ese problema.

Cada archivo `route.ts` se ejecuta exclusivamente en el servidor Node.js. Esto significa que:

- `GATEWAY_URL` y `GATEWAY_SECRET` nunca llegan al navegador
- El cliente React solo llama rutas internas (`/api/technicians`, `/api/users`, etc.)
- El cliente no tiene conocimiento de la ubicación ni de las credenciales del Gateway

## Flujo de solicitud

```
Componente React
  → feature/api (fetch /api/...)
    → app/api/route.ts        ← estás aquí (solo servidor)
      → core/gateway (Axios)
        → API Gateway
```

## Estructura

Cada subcarpeta corresponde a un recurso y contiene un único `route.ts`:

```
api/
├── auth/
│   └── route.ts
├── technicians/
│   └── route.ts
└── users/
    └── route.ts
```

## ¿Por qué no llamar al Gateway directamente desde el cliente?

El API Gateway también es usado por la app móvil y otros servicios internos. Exponer su URL y credenciales en el bundle del navegador comprometería toda la plataforma, no solo este panel.