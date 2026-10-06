# Guías del proyecto

Reglas y plantillas para escribir código en este repo. Cada guía tiene la misma
estructura: **Cuándo usarlo → Reglas → Plantilla → Evitar → Referencias**.

| Guía                               | Para qué                                                          |
| ---------------------------------- | ----------------------------------------------------------------- |
| [modelo.md](./modelo.md)           | Clases en `src/models/` (endpoints, expand, columnas)             |
| [schema.md](./schema.md)           | Validación Zod en `src/schemas/`                                  |
| [componentes.md](./componentes.md) | Reglas generales de componentes React y llamadas a la API         |
| [listado.md](./listado.md)         | Vista índice del admin (`Tabla`, búsqueda, eliminar)              |
| [formulario.md](./formulario.md)   | Vista detalle/alta del admin (`react-hook-form` + shadcn `Field`) |

## Crear un recurso nuevo en el admin

1. **Modelo**: `src/models/<Nombre>.model.ts` → [modelo.md](./modelo.md)
2. **Schema**: `src/schemas/<Nombre>Schema.ts` → [schema.md](./schema.md)
3. **Listado**: `src/pages/freshii-admin/<carpeta>/_components/index.tsx` +
   `index.astro` → [listado.md](./listado.md)
4. **Formulario**:
   `src/pages/freshii-admin/<carpeta>/_components/formulario.tsx` +
   `detalle.astro` → [formulario.md](./formulario.md)
5. **Menú**: agregar la entrada en `src/components/base/app-sidebar.tsx`
6. Verificar con `pnpm astro check`

`<carpeta>` tiene que coincidir con el `BASE_ROUTE` del modelo.
