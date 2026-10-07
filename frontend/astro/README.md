# Guías del proyecto

Reglas y plantillas para escribir código en este repo. Cada guía tiene la misma
estructura: **Cuándo usarlo → Reglas → Plantilla → Evitar → Referencias**.

| Guía                               | Para qué                                                          |
| ---------------------------------- | ----------------------------------------------------------------- |
| [02-modelo.md](./02-modelo.md)           | Clases en `src/models/` (endpoints, expand, columnas)             |
| [05-schema.md](./05-schema.md)           | Validación Zod en `src/schemas/`                                  |
| [01-componentes.md](./01-componentes.md) | Reglas generales de componentes React y llamadas a la API         |
| [03-listado.md](./03-listado.md)         | Vista índice del admin (`Tabla`, búsqueda, eliminar)              |
| [04-formulario.md](./04-formulario.md)   | Vista detalle/alta del admin (`react-hook-form` + shadcn `Field`) |
| [06-directivas-client.md](./06-directivas-client.md) | Directivas `client:*`, `<script>` y `<style>` en componentes Astro |

## Crear un recurso nuevo en el admin

1. **Modelo**: `src/models/<Nombre>.model.ts` → [02-modelo.md](./02-modelo.md)
2. **Schema**: `src/schemas/<Nombre>Schema.ts` → [05-schema.md](./05-schema.md)
3. **Listado**: `src/pages/freshii-admin/<carpeta>/_components/index.tsx` +
   `index.astro` → [03-listado.md](./03-listado.md)
4. **Formulario**:
   `src/pages/freshii-admin/<carpeta>/_components/formulario.tsx` +
   `detalle.astro` → [04-formulario.md](./04-formulario.md)
5. **Menú**: agregar la entrada en `src/components/base/app-sidebar.tsx`
6. Verificar con `pnpm astro check`

`<carpeta>` tiene que coincidir con el `BASE_ROUTE` del modelo.
