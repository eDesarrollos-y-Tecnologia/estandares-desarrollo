# Guías del proyecto

Reglas y plantillas para escribir código en los proyectos React (TanStack Router + antd).
Empezar por [creacionModulo.md](./creacionModulo.md); cada paso enlaza a su guía.

| Guía                                     | Para qué                                                             |
| ---------------------------------------- | -------------------------------------------------------------------- |
| [general.md](./general.md)               | Estructura del proyecto, aliases, estilos, tamaño de archivos y menú |
| [creacionModulo.md](./creacionModulo.md) | Guía principal: carpetas, nombres e imports de un módulo CRUD        |
| [modelos.md](./modelos.md)               | Clases en `src/models/` (endpoints, expand, columnas)                |
| [index.md](./index.md)                   | Vista de listado (`TablaQuery`, búsqueda, paginación, eliminar)      |
| [detalle.md](./detalle.md)               | Vista de alta/edición (`DetalleLayout`, `usePagina`)                 |
| [formulario.md](./formulario.md)         | Formulario del módulo (`EdForm`, validaciones, `SelectorQuery`)      |

## Crear un módulo nuevo

1. **Modelo**: `src/models/<Nombre>.model.ts` → [modelos.md](./modelos.md)
2. **Carpeta**: `src/routes/(auth)/<carpeta>/` + `-components/` →
   [creacionModulo.md](./creacionModulo.md)
3. **Listado**: `src/routes/(auth)/<carpeta>/index.tsx` → [index.md](./index.md)
4. **Detalle**: `src/routes/(auth)/<carpeta>/detalle.tsx` → [detalle.md](./detalle.md)
5. **Formulario**: `src/routes/(auth)/<carpeta>/-components/formulario.tsx` →
   [formulario.md](./formulario.md)
6. **Menú**: agregar la entrada (con icono `lucide:*`) en `src/localMenuItems.gen.ts`
7. Verificar con `pnpm lint` y `pnpm build`

`<carpeta>` tiene que coincidir con el `BASE_ROUTE` del modelo y con el `key` del menú.
Módulo de referencia: `src/routes/(auth)/categorias/` + `src/models/Categoria.model.ts`.
