# Listado (tabla CRUD)

**Cuándo usarlo:** para la vista índice de un recurso, en
`src/pages/freshii-admin/<carpeta>/_components/index.tsx`, montado desde
`index.astro`.

## Reglas

- La tabla es `<Tabla ref model columns params />` (`src/components/Tabla.tsx`).
  Se encarga del fetch a `model.ENDPOINTS.DEFAULT`, la paginación y el orden
  (por defecto `ordenar: "nombre-asc"`, `limite: 20`). Al hacer clic en una fila
  navega a `${model.BASE_ROUTE}/detalle?id=<id>`; con la prop `enlace` se puede
  cambiar el destino.
- `tablaRef = useRef<TablaHandle>(null)`:
  - `tablaRef.current?.fetch({ ...params })` para cambiar filtros o búsqueda.
  - `tablaRef.current?.refetch()` para recargar después de eliminar.
- Eliminar: `const { cargando, eliminar } = useRecurso(Modelo)` y luego
  `if (await eliminar(id)) tablaRef.current?.refetch()`. Los toasts los muestra
  el hook.
- Las columnas salen de `useMemo` a partir de `[...Modelo.COLUMNS]`, con
  `unshift` de una columna `acciones` (botón eliminar con `e.stopPropagation()`
  para que no se abra la fila).
- `params` va memorizado con `useMemo` e incluye `expand: Modelo.EXPAND.DEFAULT`
  si las columnas muestran relaciones.
- El encabezado lleva un título con `{Modelo.CLASS_NAME}s` + icono, y un botón
  "Añadir" que navega a `${Modelo.BASE_ROUTE}/detalle`.
- Filtros: `Buscador` (con debounce de 400 ms) → `fetch({ buscar })`. Los
  filtros por relación usan `QuerySelector` y hay un botón para limpiarlos.

## Plantilla

```tsx
import { useCallback, useMemo, useRef } from "react";
import type React from "react";
import { navigate } from "astro:transitions/client";
import { Servicio } from "@/models/Servicio.model";
import { useRecurso } from "@/hooks/useRecurso";
import { Tabla, type TablaHandle } from "@/components/Tabla";
import type { ModelColumnsType } from "@/types/ModeloBase.model";
import { Plus, Scissors, Trash } from "lucide-react";
import { Button } from "@/components/ui/button";
import Buscador from "@/components/Buscador";

const ServicioIndex: React.FC = () => {
  const { cargando, eliminar } = useRecurso(Servicio);
  const tablaRef = useRef<TablaHandle>(null);

  const params = useMemo(
    () => ({
      ordenar: "nombre-asc",
      expand: Servicio.EXPAND.DEFAULT,
    }),
    [],
  );

  const deleteServicio = useCallback(async (id?: string) => {
    if (await eliminar(id)) {
      await tablaRef.current?.refetch();
    }
  }, [eliminar]);

  const _columns = useMemo(() => {
    const c: ModelColumnsType<Servicio>[] = [...Servicio.COLUMNS];
    c.unshift({
      id: "acciones",
      header: "",
      size: 1,
      meta: {
        cellClassName: "px-1",
        headerClassName: "px-1",
      },
      cell: (params) => (
        <Button
          size={"xs"}
          variant={"outline"}
          disabled={cargando}
          className="z-10 border border-red-500 text-red-500 hover:text-red-700 transition-colors duration-300"
          onClick={(e) => {
            e.stopPropagation();
            deleteServicio(params.row.original.id);
          }}
        >
          <Trash />
        </Button>
      ),
    });

    return c;
  }, [deleteServicio, cargando]);

  return (
    <div>
      <div className="flex justify-between items-center gap-4 mb-4">
        <h1 className="flex items-center gap-2 text-2xl font-bold text-gray-900">
          <Scissors /> {Servicio.CLASS_NAME}s
        </h1>
        <Button
          className="cursor-pointer"
          onClick={() => navigate(`${Servicio.BASE_ROUTE}/detalle`)}
        >
          <Plus className="size-4" /> Añadir {Servicio.CLASS_NAME}
        </Button>
      </div>

      <div className="flex items-center justify-between mb-3 text-sm">
        <Buscador get={(q) => tablaRef.current?.fetch({ buscar: q })} />
      </div>

      <div className="overflow-hidden">
        <Tabla
          ref={tablaRef}
          columns={_columns}
          model={Servicio}
          params={params}
        />
      </div>
    </div>
  );
};

export default ServicioIndex;
```

### Filtro por relación (opcional)

Ejemplo tomado de `permisos` (filtra por módulo con el parámetro `m`):

```tsx
const [modulo, setModulo] = useState<string>("");

<QuerySelector<Modulo>
  value={modulo}
  placeholder="Selecciona un módulo"
  endpoint={Modulo.ENDPOINTS.DEFAULT}
  queryProps={{ ordenar: "nombre-asc" }}
  onValueChange={(v) => {
    setModulo(v);
    tablaRef.current?.fetch({ m: v });
  }}
/>;
```

### Página `index.astro`

```astro
---
import AdminLayout from "@/layouts/AdminLayout.astro";
import ServicioIndex from "./_components/index";

const breadcrumbs = [
  { label: "Inicio", href: "/freshii-admin/" },
  { label: "Servicios", href: "/freshii-admin/servicio" },
];
---

<AdminLayout title="Servicios | Freshii" breadcrumbs={breadcrumbs}>
  <ServicioIndex client:only="react" />
</AdminLayout>
```

Para que aparezca en el menú hay que agregar la entrada
`{ label, href: Modelo.BASE_ROUTE, icon }` en el arreglo de navegación de
`src/components/base/app-sidebar.tsx`.

## Evitar

- Hacer fetch o paginar a mano: eso lo hace `Tabla`.
- Escribir a mano el `try/catch` con `http.delete`: se usa `eliminar` de
  `useRecurso`.
- Mutar `Modelo.COLUMNS` directamente: siempre se copia con `[...]` antes del
  `unshift`.
- Ruta escrita a mano en el botón "Añadir" (`modulos` lo hace): se usa
  `BASE_ROUTE`.

## Referencias

- `src/pages/freshii-admin/permisos/_components/index.tsx` (patrón base con
  filtro)
- `src/hooks/useRecurso.ts`
- `src/components/Tabla.tsx`, `src/components/Buscador.tsx`,
  `src/components/QuerySelector.tsx`
