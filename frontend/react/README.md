# Guías del proyecto

Reglas y plantillas para escribir código en los proyectos React (TanStack Router + antd).
Empezar por [05-creacion-modulo.md](./05-creacion-modulo.md); cada paso enlaza a su guía.

| Guía                                     | Para qué                                                             |
| ---------------------------------------- | -------------------------------------------------------------------- |
| [06-general.md](./06-general.md)               | Estructura del proyecto, aliases, estilos, tamaño de archivos y menú |
| [05-creacion-modulo.md](./05-creacion-modulo.md) | Guía principal: carpetas, nombres e imports de un módulo CRUD        |
| [01-modelos.md](./01-modelos.md)               | Clases en `src/models/` (endpoints, expand, columnas)                |
| [02-index.md](./02-index.md)                   | Vista de listado (`TablaQuery`, búsqueda, paginación, eliminar)      |
| [03-detalle.md](./03-detalle.md)               | Vista de alta/edición (`DetalleLayout`, `usePagina`)                 |
| [04-formulario.md](./04-formulario.md)         | Formulario del módulo (`EdForm`, validaciones, `SelectorQuery`)      |

## Crear un módulo nuevo

1. **Modelo**: `src/models/<Nombre>.model.ts` → [01-modelos.md](./01-modelos.md)
2. **Carpeta**: `src/routes/(auth)/<carpeta>/` + `-components/` →
   [05-creacion-modulo.md](./05-creacion-modulo.md)
3. **Listado**: `src/routes/(auth)/<carpeta>/index.tsx` → [02-index.md](./02-index.md)
4. **Detalle**: `src/routes/(auth)/<carpeta>/detalle.tsx` → [03-detalle.md](./03-detalle.md)
5. **Formulario**: `src/routes/(auth)/<carpeta>/-components/formulario.tsx` →
   [04-formulario.md](./04-formulario.md)
6. **Menú**: agregar la entrada (con icono `lucide:*`) en `src/localMenuItems.gen.ts`
7. Verificar con `pnpm lint` y `pnpm build`

`<carpeta>` tiene que coincidir con el `BASE_ROUTE` del modelo y con el `key` del menú.
Módulo de referencia: `src/routes/(auth)/categorias/` + `src/models/Categoria.model.ts`.
