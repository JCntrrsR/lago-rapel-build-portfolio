# Arquitectura

> Documento de vitrina: explica **cómo está construido** el producto sin publicar el código.

## Decisión principal: una aplicación, tres audiencias

El sitio no es un folleto: convive con un portal de cliente y un panel de administración. En lugar
de mantener tres aplicaciones, se separó por **rutas y navegación**:

| Audiencia | Rutas | Navegación |
|---|---|---|
| Visitante | inicio, proyectos, nosotros, blog, citas, contacto | menú público |
| Cliente | portal, perfil, pagos | menú autenticado |
| Administrador | admin | panel con métricas |

Comparten componentes (tarjetas, formularios, tablas) pero no comparten menú: cada área se presenta
solo lo que le corresponde.

## Árbol de proveedores

```
ErrorBoundary (reporte de errores)
└── Idiomas           ← detección y traducción
    └── Autenticación
        └── Avisos (toasts)
            └── Router
                └── Rutas públicas · portal · administración
```

**Regla que costó un fallo:** cualquier proveedor que lea la ruta actual debe ir **dentro** del
enrutador. Si queda fuera, el hook se evalúa sin contexto y la aplicación entera cae en la pantalla
de error, aunque compile sin avisos.

## Capas

| Capa | Responsabilidad |
|---|---|
| **Datos** | obras, servicios, artículos y testimonios como módulos tipados |
| **Dominio** | componentes con lógica: catálogo, agenda, portafolio, métricas |
| **Servicios** | cliente de base de datos, pagos, analítica, monitoreo |
| **Presentación** | páginas y layouts por audiencia |

## Decisiones que sostienen el producto

**El contenido no vive en las páginas.** Cambiar una obra o un artículo no obliga a tocar la vista.

**La traducción es un proveedor, no una función suelta.** Si un componente llama a la función de
traducción sin que exista el proveedor, la aplicación se cae al montar. Con el proveedor arriba, el
idioma se resuelve una sola vez.

**Los pagos se degradan.** Sin clave publicable, el módulo de cobro se desactiva y el resto sigue
funcionando; nunca se envía una clave de ejemplo a un servicio real.

**La administración se separa del sitio público.** Código, rutas y datos distintos: un fallo en el
panel no debe tumbar la web que ven los clientes.

## Flujo del cliente

```
visita el sitio → ve obras → pide cotización
                → agenda visita → recibe acceso
                → entra al portal → revisa avance y pagos
```

## Rendimiento

| Medida | Valor |
|---|---|
| Peso del build | ~2,3 MB |
| Sourcemaps en producción | **desactivados** |
| Imágenes | locales, servidas desde el propio sitio |
| Estrategia PWA | cachea el shell y permite volver sin conexión |

## Construcción

```
npm run build    →  compila y agrupa por ruta
npm run test     →  pruebas de componentes
npm run test:e2e →  recorridos completos (Playwright)
```
