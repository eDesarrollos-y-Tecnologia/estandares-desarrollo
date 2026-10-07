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
  yii2-app-ed/ → plantilla de aplicaciones Yii2
  yii2-ed/     → librería Yii2 compartida
```

Cada stack tiene una guía de entrada con el índice de sus guías y el flujo paso
a paso para las tareas más comunes (`README.md` en cada stack; en React el flujo
completo está en `05-creacion-modulo.md`).

## Índice

| Stack | Ubicación                                        | Estado     |
| ----- | ------------------------------------------------ | ---------- |
| Astro | [frontend/astro/](./frontend/astro/README.md)    | Disponible |
| React | [frontend/react/](./frontend/react/README.md) | Disponible |
| PHP / Yii2 | [backend/](./backend/README.md)                | Disponible |

### Astro

| Guía                                                  | Para qué                                                          |
| ----------------------------------------------------- | ----------------------------------------------------------------- |
| [02-modelo.md](./frontend/astro/02-modelo.md)               | Clases en `src/models/` (endpoints, expand, columnas)             |
| [05-schema.md](./frontend/astro/05-schema.md)               | Validación Zod en `src/schemas/`                                  |
| [01-componentes.md](./frontend/astro/01-componentes.md)     | Reglas generales de componentes React y llamadas a la API         |
| [03-listado.md](./frontend/astro/03-listado.md)             | Vista índice del admin (`Tabla`, búsqueda, eliminar)              |
| [04-formulario.md](./frontend/astro/04-formulario.md)       | Vista detalle/alta del admin (`react-hook-form` + shadcn `Field`) |
| [06-directivas-client.md](./frontend/astro/06-directivas-client.md) | Directivas `client:*`, `<script>` y `<style>` en componentes Astro |

### React

| Guía                                                        | Para qué                                                                |
| ----------------------------------------------------------- | ----------------------------------------------------------------------- |
| [05-creacion-modulo.md](./frontend/react/05-creacion-modulo.md)     | Guía principal: pasos para crear un módulo CRUD completo               |
| [06-general.md](./frontend/react/06-general.md)                   | Estructura del proyecto, aliases, estilos, menú y reglas de código      |
| [01-modelos.md](./frontend/react/01-modelos.md)                   | Clases en `src/models/` (`ModeloBase`, endpoints, columnas, expand)     |
| [02-index.md](./frontend/react/02-index.md)                       | Vista de listado `index.tsx` (`CrudHeader`, `TablaQuery`)               |
| [03-detalle.md](./frontend/react/03-detalle.md)                   | Vista de alta/edición `detalle.tsx` (`DetalleLayout`, `usePagina`)      |
| [04-formulario.md](./frontend/react/04-formulario.md)             | Formulario en `-components/` (`EdForm`, antd `Form.Item`, `SelectorQuery`) |

## Formato de una guía

Todas las guías siguen la misma estructura:

1. **Cuándo usarlo** — en qué archivo o situación aplica, en una o dos líneas.
2. **Reglas** — lista corta de lo que se hace y cómo se nombra.
3. **Plantilla** — código completo y copiable, tomado de un proyecto real.
4. **Evitar** — errores comunes o patrones que ya no usamos.
5. **Referencias** — archivos de ejemplo en los proyectos y guías relacionadas.

## Agregar o cambiar una guía

1. Crea el archivo en `<capa>/<stack>/` (ej.
   `backend/yii2-app-ed/01-controlador.md`), en minúsculas y con el nombre del
   concepto. Cuando el índice tenga un orden deliberado, usa el prefijo numérico
   de dos dígitos.
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
