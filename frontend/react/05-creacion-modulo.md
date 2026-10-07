# Creación de un módulo (CRUD)

Guía principal. Sigue los pasos en orden; cada paso enlaza a su documento detallado.

Módulo de referencia: `src/routes/(auth)/categorias/` + `src/models/Categoria.model.ts`.

## 1. Estructura de carpetas

```
src/
├── models/
│   └── Marca.model.ts                 → modelo (ver 01-modelos.md)
└── routes/
    └── (auth)/                        → rutas privadas (requieren sesión)
        └── [grupo]/                   → opcional, ej. catalogos/, administracion/
            └── marcas/                → carpeta del módulo
                ├── index.tsx          → listado (ver 02-index.md)
                ├── detalle.tsx        → alta / edición (ver 03-detalle.md)
                └── -components/
                    ├── formulario.tsx → formulario (ver 04-formulario.md)
                    └── ...            → secciones extraídas de index/detalle/formulario
```

- La carpeta se llama `-components` (con guion). El `-` le indica a TanStack Router que **no** es una ruta.
- Rutas públicas (sin sesión) van en `src/routes/(public)/` con la misma estructura.

## 2. Convenciones de nombres

| Elemento | Convención | Ejemplo |
|---|---|---|
| Modelo (clase y archivo) | PascalCase, singular | `Marca`, `Marca.model.ts` |
| Carpeta del módulo | kebab-case, plural | `marcas`, `regimen-fiscal` |
| `BASE_ROUTE` del modelo | ruta de la carpeta sin `(auth)` | `/marcas`, `/catalogos/clientes` |
| Componente de la ruta | `RouteComponent` | — |
| Formulario | `-components/formulario.tsx`, `export default function Formulario` | — |
| Alias del modelo en index/detalle | `import { Marca as ModelClass }` | — |

## 3. Pasos

1. **Modelo** → crear `src/models/Marca.model.ts`. Ver [01-modelos.md](01-modelos.md).
2. **Carpeta** → crear `src/routes/(auth)/marcas/` y `-components/`.
3. **Listado** → crear `index.tsx`. Ver [02-index.md](02-index.md).
4. **Detalle** → crear `detalle.tsx`. Ver [03-detalle.md](03-detalle.md).
5. **Formulario** → crear `-components/formulario.tsx`. Ver [04-formulario.md](04-formulario.md).
6. **Menú** → agregar la entrada en `src/localMenuItems.gen.ts` (siempre con icono acorde, iconify `lucide:*`):
   ```ts
   {
     key: "/marcas",          // igual a ModelClass.BASE_ROUTE
     label: "Marcas",
     icon: "lucide:badge-check",
     visible: true,
   },
   ```
   Si el módulo está dentro de un grupo (ej. `/catalogos/marcas`), se agrega en el `children` del grupo.
   El `label` del menú también se usa en los breadcrumbs.
7. **Árbol de rutas** → correr `pnpm dev`. El plugin de TanStack regenera `src/routeTree.gen.ts` automáticamente. **No editar ese archivo a mano.**
8. **Verificar** (ver checklist abajo).

## 4. Imports: qué viene de dónde

Aliases: `@/` = `src/`, `@base/` = `base/`.

| Pieza | Import |
|---|---|
| `PaginaProvider`, `usePagina` | `@base/hooks/usePagina/usePagina` |
| `DefaultContainer` | `@base/components/layout/containers/DefaultContainer` |
| `CrudHeader` | `@base/components/layout/crud/CrudHeader` |
| `DetalleLayout` | `@base/components/layout/crud/DetalleLayout` |
| `TablaQuery` | `@base/components/tables/TablaQuery` |
| `SelectorQuery` | `@base/components/form/SelectorQuery/SelectorQuery` |
| `useHttp` (default) | `@base/hooks/useHttp/useHttp` |
| `useModelColumns` (default) | `@base/hooks/useModelColumns/useModelColumns` |
| `ActionsButton`, `EdForm`, `AntdFormValidation`, `IFormularioBaseProps`, `ModeloBase`, `ModelColumnsType` | `@e-desarrollos/react-core` |
| `createFileRoute`, `Link` | `@tanstack/react-router` |
| `Row`, `Col`, `Card`, `Form`, `Input`, `Button`... | `antd` |
| Iconos | `Icon` de `@iconify/react` |

## 5. Checklist final

- [ ] El modelo tiene `CLASS_NAME`, `BASE_ROUTE`, `ENDPOINTS.DEFAULT`, `COLUMNS`, `EXPAND.DEFAULT`, `fromJson`, `fromJsonList` y constructor.
- [ ] `BASE_ROUTE` coincide con la carpeta y con el `key` del menú.
- [ ] El módulo aparece en el sidebar con icono.
- [ ] El listado carga, busca, pagina y ordena.
- [ ] "Agregar" abre `detalle` vacío con título "Agregar X"; guardar regresa al listado.
- [ ] Clic en un registro abre `detalle?id=...` con los datos cargados y título "Detalle de X".
- [ ] Eliminar muestra confirmación y refresca la tabla.
- [ ] Breadcrumbs correctos (Inicio › Módulo › Nuevo/Editar).
- [ ] Ningún archivo pasa de 600 líneas.
- [ ] `pnpm lint` y `pnpm build` sin errores.

Reglas generales del proyecto: [06-general.md](06-general.md).
