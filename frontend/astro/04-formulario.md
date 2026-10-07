# Formulario (detalle / alta)

**Cuándo usarlo:** para crear o editar un registro en
`src/pages/freshii-admin/<carpeta>/_components/formulario.tsx`, montado desde
`detalle.astro`. Antes hay que tener el [modelo](./02-modelo.md) y el
[schema](./05-schema.md).

## Reglas

- Importa el modelo como `ModelClass` (`import { Servicio as ModelClass } ...`),
  así el archivo se copia entre recursos cambiando solo esa línea.
- `id` sale de `?id=` en la URL. Si no hay `id`, es un alta; si hay, es una
  edición y se llama a `get(id)`.
- Estado: `editando`. `cargando`, `obtenerPorId` y `guardar` salen de
  `useRecurso(ModelClass)` (ver [01-componentes.md](./01-componentes.md)).
- `useForm<XFormData>` con `standardSchemaResolver(XSchema)`, y `defaultValues`
  con **todos** los campos (`""` para strings).
- `get(id)`: `obtenerPorId(id, ModelClass.EXPAND.DEFAULT)` devuelve el registro
  o `null` (el toast de error lo muestra el hook). Si hay datos, se pasan a
  `form.reset({...})` campo por campo con `?? ""`.
- `onFinish`: `guardar({ ...values, id: id ?? undefined })`. El POST crea o
  actualiza según venga `id`, y el hook muestra el toast. Si no devuelve `null`:
  `navigate(ModelClass.BASE_ROUTE)`.
- Cada campo va dentro de un `Controller` → `Field data-invalid` →
  `FieldLabel htmlFor required?` → control con `{...field}` y `aria-invalid` →
  `FieldError` condicional.
- Layout: `form > Card > FieldSet > FieldGroup` con `grid sm:grid-cols-2 gap-4`.
  Los campos anchos (textarea) usan `col-span-full`.
- El botón submit lleva `disabled={cargando}` y el texto cambia según el modo
  (`Actualizar` / `Guardar`).
- Si el formulario es largo, cada sección va en su propio `Card` con un
  encabezado `h2` (ver `usuarios`).

## Controles según el tipo de campo

| Campo           | Control                           | Notas                                                                                                                                    |
| --------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Texto corto     | `Input`                           | `placeholder="Ej. ..."`                                                                                                                  |
| Texto largo     | `Textarea`                        | `className="w-auto h-24"`, `Field` con `col-span-full`                                                                                   |
| FK / relación   | `QuerySelector<Modelo>`           | `endpoint={Modelo.ENDPOINTS.DEFAULT}`, `queryProps={{ ordenar: "nombre-asc" }}`, `value={field.value}`, `onValueChange={field.onChange}` |
| Enum del modelo | `Select` + `SelectItem`           | Opciones de `ModelClass.<ENUM>` (ej. `Usuario.ROLES`)                                                                                    |
| Activo/inactivo | `Switch`                          | `inactivo` es `null` cuando está activo, o la fecha ISO en que se desactivó                                                              |
| Contraseña      | `Input type=password` + botón ojo | Se muestra solo en alta (`!id`)                                                                                                          |

## Plantilla

```tsx
import { Button } from "@/components/ui/button";
import { Card } from "@/components/ui/card";
import {
  Field,
  FieldError,
  FieldGroup,
  FieldLabel,
  FieldSet,
} from "@/components/ui/field";
import { Input } from "@/components/ui/input";
import { Textarea } from "@/components/ui/textarea";
import QuerySelector from "@/components/QuerySelector";
import { useRecurso } from "@/hooks/useRecurso";
import { standardSchemaResolver } from "@hookform/resolvers/standard-schema";
import { navigate } from "astro:transitions/client";
import { ArrowLeftIcon, SaveIcon } from "lucide-react";
import React, { useCallback, useEffect, useState } from "react";
import { Controller, useForm } from "react-hook-form";
import { Servicio as ModelClass } from "@/models/Servicio.model";
import { Categoria } from "@/models/Categoria.model";
import {
  type ServicioFormData,
  ServicioSchema,
} from "@/schemas/ServicioSchema";

const DetalleServicioView: React.FC = () => {
  const [editando, setEditando] = useState<boolean>(false);
  const id = new URLSearchParams(window.location.search).get("id");
  const { cargando, obtenerPorId, guardar } = useRecurso(ModelClass);

  const form = useForm<ServicioFormData>({
    resolver: standardSchemaResolver(ServicioSchema),
    defaultValues: {
      nombre: "",
      idCategoria: "",
      descripcion: "",
    },
  });

  const get = useCallback(async (id: string) => {
    const data = await obtenerPorId(id, ModelClass.EXPAND.DEFAULT);
    if (data) {
      form.reset({
        nombre: data.nombre ?? "",
        idCategoria: data.idCategoria ?? "",
        descripcion: data.descripcion ?? "",
      });
    }
  }, [obtenerPorId]);

  const onFinish = useCallback(async (values: ServicioFormData) => {
    if (await guardar({ ...values, id: id ?? undefined })) {
      navigate(ModelClass.BASE_ROUTE);
    }
  }, [id, guardar]);

  useEffect(() => {
    if (id) {
      get(id);
      setEditando(true);
    }
  }, [id, get]);

  return (
    <div className="flex flex-col justify-center w-full h-auto">
      <div className="flex justify-between items-center mb-6 w-full h-auto">
        <h1 className="scroll-m-20 text-2xl font-semibold tracking-tight">
          {editando
            ? `Detalle ${ModelClass.CLASS_NAME}`
            : `Agregar ${ModelClass.CLASS_NAME}`}
        </h1>
        <Button
          onClick={() => navigate(ModelClass.BASE_ROUTE)}
          variant="ghost"
          size="sm"
          className="text-muted-foreground hover:text-primary cursor-pointer"
        >
          <ArrowLeftIcon className="size-4" /> Volver
        </Button>
      </div>

      <form onSubmit={form.handleSubmit(onFinish)}>
        <Card className="px-4 py-6">
          <FieldSet>
            <FieldGroup className="grid sm:grid-cols-2 gap-4 w-full h-auto">
              <Controller
                name="nombre"
                control={form.control}
                render={({ field, fieldState }) => (
                  <Field className="gap-2" data-invalid={fieldState.invalid}>
                    <FieldLabel htmlFor="nombre" required>Nombre</FieldLabel>
                    <Input
                      {...field}
                      name="nombre"
                      placeholder="Ej. Corte clásico"
                      aria-invalid={fieldState.invalid}
                    />
                    {fieldState.invalid && (
                      <FieldError>{fieldState.error?.message}</FieldError>
                    )}
                  </Field>
                )}
              />

              <Controller
                name="idCategoria"
                control={form.control}
                render={({ field, fieldState }) => (
                  <Field className="gap-2" data-invalid={fieldState.invalid}>
                    <FieldLabel htmlFor="idCategoria" required>
                      Categoría
                    </FieldLabel>
                    <QuerySelector<Categoria>
                      value={field.value}
                      endpoint={Categoria.ENDPOINTS.DEFAULT}
                      placeholder="Selecciona una categoría"
                      queryProps={{ ordenar: "nombre-asc" }}
                      onValueChange={field.onChange}
                    />
                    {fieldState.invalid && (
                      <FieldError>{fieldState.error?.message}</FieldError>
                    )}
                  </Field>
                )}
              />

              <Controller
                name="descripcion"
                control={form.control}
                render={({ field, fieldState }) => (
                  <Field
                    className="col-span-full gap-2"
                    data-invalid={fieldState.invalid}
                  >
                    <FieldLabel htmlFor="descripcion">Descripción</FieldLabel>
                    <Textarea
                      {...field}
                      name="descripcion"
                      className="w-auto h-24"
                      aria-invalid={fieldState.invalid}
                    />
                    {fieldState.invalid && (
                      <FieldError>{fieldState.error?.message}</FieldError>
                    )}
                  </Field>
                )}
              />

              <Field className="col-span-full flex justify-start items-center h-auto w-26">
                <Button
                  type="submit"
                  disabled={cargando}
                  className="cursor-pointer"
                >
                  <SaveIcon /> {editando ? "Actualizar" : "Guardar"}
                </Button>
              </Field>
            </FieldGroup>
          </FieldSet>
        </Card>
      </form>
    </div>
  );
};

export default DetalleServicioView;
```

### Página `detalle.astro`

```astro
---
import AdminLayout from "@/layouts/AdminLayout.astro";
import DetalleServicio from "./_components/formulario";

const breadcrumbs = [
  { label: "Inicio", href: "/freshii-admin/" },
  { label: "Servicios", href: "/freshii-admin/servicio" },
  { label: "Detalle" },
];
---

<AdminLayout title="Freshii | Detalle del servicio" breadcrumbs={breadcrumbs}>
  <DetalleServicio client:only="react" />
</AdminLayout>
```

`client:only="react"` es obligatorio porque el componente lee `window.location`
al renderizar.

## Evitar

- Rutas escritas a mano (`navigate("/freshii-admin/modulos")`): se usa
  `ModelClass.BASE_ROUTE`.
- Comparar con `id !== "nuevo"` (lo hace `modulos`): cuando es alta, simplemente
  no hay `id`.
- Dejar el submit habilitado mientras `cargando` (se duplican envíos).
- Escribir a mano el `try/catch` con `http.get`/`http.post`: se usa
  `useRecurso`.
- Omitir `defaultValues`: el input pasa de no controlado a controlado y React
  avisa en consola.
- Exigir en el schema campos que se ocultan al editar. Ver
  [05-schema.md](./05-schema.md#evitar).

## Referencias

- `src/hooks/useRecurso.ts`
- `src/pages/freshii-admin/permisos/_components/formulario.tsx` (patrón base,
  `QuerySelector`)
- `src/pages/freshii-admin/usuarios/_components/formulario.tsx` (secciones en
  `Card`, `Select`, `Switch`, contraseña)
- `src/components/QuerySelector.tsx`, `src/components/ui/field.tsx`
