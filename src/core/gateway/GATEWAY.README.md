# Gateway

Esta carpeta contiene la instancia de Axios configurada para comunicarse con el API Gateway central de la empresa.

## Decisión técnica

Todas las solicitudes desde el servidor de Next.js hacia el API Gateway pasan por este cliente. Está ubicado intencionalmente dentro de `core/` y **solo debe importarse desde los route handlers de `src/app/api/`** — nunca desde código a nivel de features ni desde componentes cliente.

Esto refuerza el patrón BFF (Backend for Frontend):

```
feature/api → /app/api/route.ts → core/gateway → API Gateway
```

Mantener el cliente del gateway aquí significa que las credenciales (`GATEWAY_URL`, `GATEWAY_SECRET`) permanecen siempre en el servidor y nunca se incluyen en el JavaScript del cliente.

## Qué va aquí

- La instancia base de Axios con headers por defecto y base URL
- Interceptores de request/response (inyección de token de auth, normalización de errores)
- Lógica de reintento si es necesaria

## Qué NO va aquí

- Lógica de negocio
- Transformación de datos
- Uso directo desde componentes de React o hooks de features