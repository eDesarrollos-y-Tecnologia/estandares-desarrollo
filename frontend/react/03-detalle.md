# Vista de detalle (`detalle.tsx`)

Archivo: `src/routes/(auth)/[modulo]/detalle.tsx`. Una sola vista sirve para **agregar** y **editar**:

- `/categorias/detalle` → agregar (formulario vacío).
- `/categorias/detalle?id=123` → editar (carga el registro).

Referencia: `src/routes/(auth)/categorias/detalle.tsx`.

El archivo solo conecta el modelo con `DetalleLayout`; los campos van en `-components/formulario.tsx` (ver [04-formulario.md](04-formulario.md)).

## Ejemplo completo

```tsx
import Formulario from "./-components/formulario";
import { Categoria as ModelClass } from "@/models/Categoria.model";
import { PaginaProvider, usePagina } from "@base/hooks/usePagina/usePagina";
import { createFileRoute } from "@tanstack/react-router";
import { DetalleLayout } from "@base/components/layout/crud/DetalleLayout";

export const Route = createFileRoute("/(auth)/categorias/detalle")({
  component: () => (
    <PaginaProvider titulo={`Detalle de ${ModelClass.CLASS_NAME}`}>
      <RouteComponent />
    </PaginaProvider>
  ),
});

function RouteComponent() {
  const { handleVolver, queryParams, isEditando } = usePagina();

  return (
    <DetalleLayout
      handleVolver={handleVolver}
      queryParams={queryParams}
      isEditando={isEditando}
      nombrePlural={`${ModelClass.CLASS_NAME}s`}
      nombreSingular={ModelClass.CLASS_NAME}
      queryProps={{
        endpoint: ModelClass.ENDPOINTS.DEFAULT,
        extraParams: {
          id: queryParams?.id,
          expand: ModelClass.EXPAND.DEFAULT,
        },
        enabled: !!queryParams?.id,
      }}
      Formulario={Formulario}
    />
  );
}
```

## `usePagina()`

| Valor | Descripción |
|---|---|
| `handleVolver` | Navega a `..` (el listado). |
| `queryParams` | Search params de la URL (`queryParams.id`). |
| `isEditando` | `true` si la URL trae `?id=`. |

## Props de `DetalleLayout`

| Prop | Requerida | Descripción |
|---|---|---|
| `handleVolver`, `queryParams`, `isEditando` | sí | Vienen de `usePagina()`. |
| `nombreSingular`, `nombrePlural` | sí | Para títulos y botones. |
| `queryProps` | sí | Consulta del registro: `endpoint`, `extraParams: { id, expand }`, `enabled: !!queryParams?.id` (solo consulta al editar). |
| `Formulario` | normalmente | Componente del formulario. Recibe las props descritas en 04-formulario.md. |
| `saveEndpoint` | no | Endpoint de guardado si es distinto de `queryProps.endpoint` (ej. `ModelClass.ENDPOINTS.GUARDAR`). |
| `formQueryProps` | no | Se pasa al formulario como `queryProps`. |
| `headerExtra` | no | Botones extra en el header, junto a "Volver". |
| `showHeader` | no | `false` para ocultar el header. Default `true`. |
| `children` | no | Contenido debajo del formulario (tablas relacionadas, historial, etc.). |

## Qué hace por dentro (`base/hooks/useDetalle/useDetalle.tsx`)

1. Si `enabled`, hace GET a `queryProps.endpoint` con `extraParams` y toma `resultado[0]` como `modelo`.
2. Título: `Agregar {nombreSingular}` o `Detalle de {nombreSingular}`.
3. Botón: `Guardar {nombreSingular}` o `Actualizar {nombreSingular}`.
4. Al enviar el formulario hace POST a `saveEndpoint ?? queryProps.endpoint` con `{ id (si edita), ...valores }`.
5. Si guarda bien, regresa al listado (`..`).

No hay que programar carga, guardado ni navegación en `detalle.tsx`.

## Variantes

Endpoint de guardado distinto (`cotizaciones/detalle.tsx`):

```tsx
<DetalleLayout
  {...}
  saveEndpoint={ModelClass.ENDPOINTS.GUARDAR}
  Formulario={Formulario}
/>
```

Varias relaciones en `expand` (`administracion/usuarios/detalle.tsx`):

```tsx
extraParams: {
  id: queryParams?.id,
  expand: `${ModelClass.EXPAND.DEFAULT},${ModelClass.EXPAND.PERMISOS}`,
},
```

Contenido adicional debajo del formulario solo al editar:

```tsx
<DetalleLayout {...} Formulario={Formulario}>
  {isEditando && <TablaRelacionada idPadre={queryParams.id} />}
</DetalleLayout>
```

`TablaRelacionada` va en `-components/`.

## Checklist

- [ ] Ruta `createFileRoute("/(auth)/modulo/detalle")` con `PaginaProvider`.
- [ ] `enabled: !!queryParams?.id`.
- [ ] `expand` incluye todo lo que el formulario necesita mostrar.
- [ ] Sin lógica de carga/guardado propia, salvo que `DetalleLayout` no alcance.
