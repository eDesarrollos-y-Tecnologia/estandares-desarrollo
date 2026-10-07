# Reglas generales

## Estructura del proyecto

| Ruta | Contenido |
|---|---|
| `src/models/` | Modelos (`Nombre.model.ts`). Ver [modelos.md](modelos.md). |
| `src/routes/(auth)/` | Vistas privadas. Una carpeta por módulo. Ver [creacionModulo.md](creacionModulo.md). |
| `src/routes/(public)/` | Vistas públicas. |
| `src/routes/**/-components/` | Componentes propios de un módulo (no son rutas). |
| `src/components/` | Componentes compartidos de la app (`utils/`, `dashboard/`...). |
| `base/` | Base reutilizable: layouts CRUD, `TablaQuery`, `SelectorQuery`, hooks (`usePagina`, `useHttp`, `useDetalle`, `useModelColumns`). |
| `src/config/` | Configuración (antd, axios, react-query, router, variables de entorno). |
| `src/localMenuItems.gen.ts` | Menú del sidebar. |
| `src/routeTree.gen.ts` | Generado por TanStack Router. **No editar.** |

Aliases: `@/` = `src/`, `@base/` = `base/`. Usarlos siempre en lugar de rutas relativas largas.

## Estilos

- Solo existe un archivo CSS: `src/global.css`. No crear otros.
- Estilos de componentes con Tailwind; usar CSS en `global.css` solo si Tailwind no alcanza.
- Colores y tema de antd: `src/config/settings/antd-config-provider.tsx`. Para personalización mayor, `src/global.css`.

## Tamaño y organización de archivos

- Ningún archivo debe pasar de 600 líneas. Para lograrlo:
  - separar secciones de la vista en `-components/` del módulo;
  - usar los modelos existentes en lugar de crear interfaces dentro de los componentes;
  - antes de crear un componente, buscar si ya existe en `base/components`, `src/components` o `@e-desarrollos/react-core` y reutilizarlo.

## Código

- Evitar código comentado y comentarios innecesarios.
- Orden dentro de los componentes: hooks/estado/variables → funciones → `useMemo` → `useEffect` → `return`.
- No escribir rutas ni endpoints a mano: usar `ModelClass.BASE_ROUTE` y `ModelClass.ENDPOINTS`.

## Menú (`src/localMenuItems.gen.ts`)

- Define las rutas que aparecen en el sidebar; el `label` también se usa en los breadcrumbs.
- Toda entrada lleva un icono acorde al nombre (iconify, preferentemente `lucide:*`).
- El `key` es la ruta (igual al `BASE_ROUTE` del modelo). Los submódulos van en `children`.

## Otros

- Las plantillas de `base/scripts/templates/modelo/` (interface + hook) son de una versión anterior y no se usan. Para modelos seguir [modelos.md](modelos.md).
