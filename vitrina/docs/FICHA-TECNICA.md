# Ficha técnica

## Stack

| Capa | Tecnología | Por qué |
|---|---|---|
| Framework | **React 18 + Vite** | interfaz reactiva con build rápido por ruta |
| Lenguaje | **TypeScript** | obras, pagos y formularios con tipos |
| Estilos | **Tailwind CSS** | consistencia visual sin CSS disperso |
| Componentes | **Headless UI + Lucide** | accesibles, sin imponer diseño |
| Rutas | **React Router** | áreas pública, cliente y administración |
| Formularios | **React Hook Form + esquemas** | validación declarativa por campo |
| Idioma | **i18next + react-i18next** | detección y traducción en un proveedor |
| Datos | **Supabase** | autenticación y base de datos |
| Pagos | **Stripe** | cobro con tarjeta y webhooks |
| Gráficos | **Chart.js** | métricas del panel de administración |
| Contenido | **React Markdown + rehype** | blog en Markdown con HTML controlado |
| Fechas | **date-fns + date-fns-tz** | agenda con zona horaria |
| PWA | **vite-plugin-pwa** | instalable y con soporte sin conexión |

## Tamaño del proyecto

| Métrica | Valor |
|---|---|
| Archivos `.ts` / `.tsx` | 64 |
| Líneas de código | ~15.000 |
| Dependencias de producción | 29 |
| Rutas | 12 |
| Peso del build | ~2,3 MB |

## Verificación

```
npm run build     →  exit 0 · 0 sourcemaps · ~2,3 MB
npm run test      →  pruebas de componentes
npm run test:e2e  →  recorridos con Playwright
```

## Defectos corregidos en el escaparate público

| Defecto | Efecto | Corrección |
|---|---|---|
| JSX dentro de `monitoring.ts` | el build **no compilaba** | renombrado a `.tsx` |
| `@sentry/react` y `web-vitals` sin declarar | imports rotos | dependencias declaradas |
| API antigua de `web-vitals` (`getCLS`, `getFID`) | `not exported` | migrado a `onCLS`/`onINP` |
| `t()` de traducción sin proveedor | `ReferenceError: t is not defined` → app caída | se usa el texto ya definido |
| 47 imágenes pedidas a Pexels en caliente | sin red, sin imágenes | descargadas y locales |
| `sourcemap: true` | publicaba el código fuente | desactivado |

## Límites conocidos

| Límite | Motivo |
|---|---|
| Sin backend ni datos reales | el escaparate no depende de servicios |
| Pagos simulados | sin claves publicables |
| Portal de cliente demostrativo | requiere base de datos conectada |
| Analítica desactivada | no se envían datos a terceros |

## Qué demuestra al cliente

- Un producto con **tres audiencias** en una sola aplicación.
- Capacidad de rescatar un proyecto que no compilaba y no renderizaba.
- Criterio de producción: configuración por entorno, degradación limpia, cero claves de ejemplo.
