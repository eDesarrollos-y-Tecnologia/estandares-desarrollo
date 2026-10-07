# Vista de listado (`index.tsx`)

Archivo: `src/routes/(auth)/[modulo]/index.tsx`. Muestra la tabla del módulo con búsqueda, paginación, orden, botón "Agregar" y acciones de editar/eliminar.

Referencia: `src/routes/(auth)/categorias/index.tsx`.

## Reglas

- Aprovechar al máximo el espacio en blanco.
- Botones, filtros y buscadores alineados de forma limpia y ordenada (usar el `children` de `CrudHeader` o `topContent` de `TablaQuery`).
- Usar siempre componentes de antd, `@e-desarrollos/react-core` o `base/`.
- El modelo se importa con alias: `import { Categoria as ModelClass } from "@/models/Categoria.model";`. Así el resto del archivo es igual en todos los módulos.
- Orden dentro del componente:
  1. hooks, `useState`, `useRef`, variables
  2. funciones (`handleAgregar`, etc.)
  3. `useMemo` (columnas)
  4. `useEffect` justo antes del `return`
  5. `return`

## 1. Imports

```tsx
import { DefaultContainer } from "@base/components/layout/containers/DefaultContainer";
import { CrudHeader } from "@base/components/layout/crud/CrudHeader";
import { TablaQuery } from "@base/components/tables/TablaQuery";
import { PaginaProvider, usePagina } from "@base/hooks/usePagina/usePagina";
import useHttp from "@base/hooks/useHttp/useHttp";
import useModelColumns from "@base/hooks/useModelColumns/useModelColumns";
import { ActionsButton } from "@e-desarrollos/react-core";
import { createFileRoute, Link } from "@tanstack/react-router";
import { Col, Row } from "antd";
import { ColumnType } from "antd/es/table";
import { useMemo } from "react";
import { Categoria as ModelClass } from "@/models/Categoria.model";
```

## 2. Ruta

```tsx
export const Route = createFileRoute("/(auth)/categorias/")({
  component: () => (
    <PaginaProvider titulo={`Listado de ${ModelClass.CLASS_NAME}s`}>
      <RouteComponent />
    </PaginaProvider>
  ),
});
```

- El string de la ruta es la ruta del archivo con `/` final: `"/(auth)/catalogos/clientes/"`.
- Vista pública: `createFileRoute("/(public)/categorias/")`.
- `PaginaProvider` pone el título de la pestaña y genera los breadcrumbs.

## 3. Componente

```tsx
function RouteComponent() {
  const { router, requestParams, setRequestParams } = usePagina();
  const http = useHttp();

  const handleAgregar = () => {
    router.navigate({ to: `${ModelClass.BASE_ROUTE}/detalle` });
  };

  const modelColumns = useModelColumns({
    columns: ModelClass.COLUMNS,
    actionsButtonRender: (value, data) => (
      <ActionsButton
        onEliminar={() => {
          http.defaultDelete({
            modalContent: (
              <p>
                ¿Está seguro de querer eliminar el{" "}
                {ModelClass.CLASS_NAME.toLowerCase()} <b>{data.nombre}</b>?
              </p>
            ),
            requestParams: {
              endpoint: ModelClass.ENDPOINTS.DEFAULT,
              params: { id: value },
              onSuccess: () => {
                setRequestParams((prev) => ({ ...prev, timestamp: Date.now() }));
              },
            },
          });
        }}
        onEditar={() => {
          router.navigate({
            to: `${ModelClass.BASE_ROUTE}/detalle`,
            search: { id: value },
          });
        }}
      />
    ),
  });

  // paso 4 (opcional): columnsEdit

  return (/* paso 5 */);
}
```

Qué hace cada pieza:

| Pieza | Función |
|---|---|
| `usePagina()` | `router` para navegar; `requestParams`/`setRequestParams` sincronizan filtros, página y búsqueda con la URL. |
| `useModelColumns` | Convierte `ModelClass.COLUMNS` en columnas de antd y agrega al inicio la columna de acciones con `actionsButtonRender(id, registro)`. Cada celda es link a `detalle?id=`. |
| `http.defaultDelete` | Muestra el modal de confirmación y hace el DELETE. |
| `timestamp: Date.now()` | Cambia los `requestParams` para que `TablaQuery` vuelva a consultar después de eliminar. |

## 4. Personalizar columnas (opcional)

Para cambiar ancho, alineación, render u ocultar columnas, se crea **un** `useMemo` llamado `columnsEdit` que recorre `modelColumns` por `key`:

```tsx
const columnsEdit: ColumnType<any>[] = useMemo(() => {
  return modelColumns.map((column: any) => {
    if (column.key === "nombre") {
      return {
        ...column,
        width: 400,
        render: (_: string, r: any) => (
          <Link
            to={`${ModelClass.BASE_ROUTE}/detalle`}
            search={{ id: r.id }}
            className="font-semibold text-slate-800 hover:text-blue-600"
          >
            {r.nombre}
          </Link>
        ),
      };
    }

    if (column.key === "descripcion") {
      return { ...column, width: 0, className: "hidden", hidden: true };
    }

    if (column.key === "activo") {
      return {
        ...column,
        width: 130,
        align: "center",
        render: (value: any) => {
          const isActive = value === true || value === 1 || value === "1";
          return (
            <div
              className={`inline-flex items-center gap-1.5 px-2 py-1 rounded-lg border ${
                isActive
                  ? "bg-green-50 text-green-600 border-green-200/60"
                  : "bg-slate-50 text-red-400 border-slate-200/50"
              }`}
            >
              <div className={`w-1.5 h-1.5 rounded-full ${isActive ? "bg-green-500" : "bg-red-400"}`} />
              <span className="text-[10px] font-bold uppercase tracking-wider">
                {isActive ? "Activo" : "Inactivo"}
              </span>
            </div>
          );
        },
      };
    }

    return column;
  });
}, [modelColumns]);
```

Patrones comunes:

| Necesidad | Qué devolver |
|---|---|
| Fijar ancho / alinear | `{ ...column, width: 60, align: "center", fixed: "left" }` |
| Render propio | `{ ...column, render: (valor, registro) => ... }` |
| Ocultar columna (se usa solo dentro de otro render) | `{ ...column, width: 0, className: "hidden", hidden: true }` |
| Cambiar el título | `{ ...column, title: "Categoría" }` |

Si los renders crecen mucho, moverlos a `-components/` (ej. `-components/columnas.tsx`).
Si no se personaliza nada, pasar `modelColumns` directo a `TablaQuery`.

## 5. JSX

```tsx
return (
  <DefaultContainer className="h-full p-0!">
    <Row gutter={[24, 16]}>
      <Col span={24}>
        <CrudHeader
          titulo={ModelClass.CLASS_NAME}
          agregarLabel={`Agregar ${ModelClass.CLASS_NAME}`}
          onAgregar={handleAgregar}
        />
      </Col>

      <Col span={24}>
        <TablaQuery
          columns={columnsEdit}
          queryProps={{
            endpoint: ModelClass.ENDPOINTS.DEFAULT,
            extraParams: {
              expand: ModelClass.EXPAND.DEFAULT,
              ...requestParams,
            },
          }}
          setRequestParams={setRequestParams}
          showSearch
          searchProps={{
            inputProps: {
              placeholder: `Buscar por ${ModelClass.CLASS_NAME.toLowerCase()}...`,
            },
          }}
        />
      </Col>
    </Row>
  </DefaultContainer>
);
```

Props útiles:

| Componente | Prop | Uso |
|---|---|---|
| `CrudHeader` | `children` | Botones o filtros extra, alineados a la derecha junto a "Agregar". |
| `CrudHeader` | `subtitulo`, `agregarIcon` | Texto secundario / icono del botón. |
| `TablaQuery` | `ordenar` | Orden inicial, por defecto `"id-desc"`. |
| `TablaQuery` | `disableOrdenar` | Quita el orden por columnas. |
| `TablaQuery` | `topContent` | Contenido arriba de la tabla (filtros). |
| `TablaQuery` | `dashboardStats` | Tarjetas de estadísticas (`CardTablero`) arriba de la tabla. |
| `TablaQuery` | `wrapInCard` | `false` para no envolver en `Card`. |
| `TablaQuery` | `tableProps` | Props directas de `Table` de antd (ej. `scroll`). |

## Checklist

- [ ] Ruta `createFileRoute("/(auth)/modulo/")` con `PaginaProvider`.
- [ ] Modelo importado como `ModelClass`; sin strings de rutas o endpoints escritos a mano.
- [ ] Columnas desde `ModelClass.COLUMNS` + `useModelColumns`.
- [ ] Eliminar refresca la tabla con `timestamp`.
- [ ] `expand` y `...requestParams` en `extraParams`.
- [ ] Menos de 600 líneas.
