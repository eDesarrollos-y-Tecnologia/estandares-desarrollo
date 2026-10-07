# Guías de desarrollo

Reglas y plantillas para escribir código en nuestros proyectos de **Astro**,
**React** y **PHP**. La idea es que todos los proyectos se escriban igual: misma
estructura de carpetas, mismos nombres y mismos patrones. Sirven como referencia
para el equipo y como contexto para asistentes de IA.

## Estructura del repo

```
frontend/
  astro/   → proyectos Astro con islas React (admin + sitio público)
  react/   → proyectos React + TanStack Router + antd
backend/
  php/     → API en PHP (pendiente)
```

Cada stack tiene una guía de entrada con el índice de sus guías y el flujo paso
a paso para las tareas más comunes (`README.md` en Astro, `creacionModulo.md` en
React).

## Índice

| Stack | Ubicación                                        | Estado     |
| ----- | ------------------------------------------------ | ---------- |
| Astro | [frontend/astro/](./frontend/astro/README.md)    | Disponible |
| React | [frontend/react/](./frontend/react/creacionModulo.md) | Disponible |
| PHP   | `backend/php/`                                   | Pendiente  |

### Astro

| Guía                                                  | Para qué                                                          |
| ----------------------------------------------------- | ----------------------------------------------------------------- |
| [modelo.md](./frontend/astro/modelo.md)               | Clases en `src/models/` (endpoints, expand, columnas)             |
| [schema.md](./frontend/astro/schema.md)               | Validación Zod en `src/schemas/`                                  |
| [componentes.md](./frontend/astro/componentes.md)     | Reglas generales de componentes React y llamadas a la API         |
| [listado.md](./frontend/astro/listado.md)             | Vista índice del admin (`Tabla`, búsqueda, eliminar)              |
| [formulario.md](./frontend/astro/formulario.md)       | Vista detalle/alta del admin (`react-hook-form` + shadcn `Field`) |

### React

| Guía                                                        | Para qué                                                                |
| ----------------------------------------------------------- | ----------------------------------------------------------------------- |
| [creacionModulo.md](./frontend/react/creacionModulo.md)     | Guía principal: pasos para crear un módulo CRUD completo               |
| [general.md](./frontend/react/general.md)                   | Estructura del proyecto, aliases, estilos, menú y reglas de código      |
| [modelos.md](./frontend/react/modelos.md)                   | Clases en `src/models/` (`ModeloBase`, endpoints, columnas, expand)     |
| [index.md](./frontend/react/index.md)                       | Vista de listado `index.tsx` (`CrudHeader`, `TablaQuery`)               |
| [detalle.md](./frontend/react/detalle.md)                   | Vista de alta/edición `detalle.tsx` (`DetalleLayout`, `usePagina`)      |
| [formulario.md](./frontend/react/formulario.md)             | Formulario en `-components/` (`EdForm`, antd `Form.Item`, `SelectorQuery`) |

## Formato de una guía

Todas las guías siguen la misma estructura:

1. **Cuándo usarlo** — en qué archivo o situación aplica, en una o dos líneas.
2. **Reglas** — lista corta de lo que se hace y cómo se nombra.
3. **Plantilla** — código completo y copiable, tomado de un proyecto real.
4. **Evitar** — errores comunes o patrones que ya no usamos.
5. **Referencias** — archivos de ejemplo en los proyectos y guías relacionadas.

## Agregar o cambiar una guía

1. Crea el archivo en `<capa>/<stack>/` (ej. `backend/php/controlador.md`), en
   minúsculas y con el nombre del concepto que documenta.
2. Sigue el [formato](#formato-de-una-guía).
3. Agrégala a la guía de entrada del stack y a su tabla en este archivo; si es
   un stack nuevo, también a la tabla de [Índice](#índice).
4. Si cambias una regla, actualiza también la plantilla y las guías que la
   mencionan.

## Convenciones de redacción

- Todo en español. Los nombres de código (clases, funciones, rutas) se escriben
  tal como están en el proyecto.
- Archivos, rutas y nombres de código van entre backticks: `src/models/`.
- Reglas concretas, no recomendaciones generales: "`export default` al final del
  archivo", no "procura exportar bien".
- Los ejemplos deben ser mínimos pero completos, listos para copiar.
