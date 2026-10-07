# Formulario (`-components/formulario.tsx`)

Archivo: `src/routes/(auth)/[modulo]/-components/formulario.tsx`. Lo renderiza `DetalleLayout` (ver [detalle.md](detalle.md)), que le pasa los datos y la función de guardado.

Referencias: `categorias/-components/formulario.tsx` (secciones en `Card`, switch, imagen), `administracion/permisos/-components/formulario.tsx` (`SelectorQuery`).

## Reglas

- Aprovechar al máximo el espacio en blanco; botones alineados de forma limpia.
- Orden dentro del componente: hooks/`useState`/`useRef`/variables → funciones → `useEffect` → `return`.
- Raíz siempre `EdForm` de `@e-desarrollos/react-core` con `{...formProps}`, `form={form}` e `isLoading={isLoading}`.
- Layout con `Row` + `Col` responsivo (`span={24} md={12}`). Agrupar campos relacionados en `Card` por sección.
- Cada campo dentro de `Form.Item` de antd con `name` igual a la propiedad del modelo.
- Validaciones con `AntdFormValidation`; regex propio solo si no existe la regla.
- Campos FK (relación con otro modelo) → `SelectorQuery`.
- Si pasa de ~600 líneas, separar secciones en otros archivos de `-components/`.

## Props que recibe (`IFormularioBaseProps`)

| Prop | Descripción |
|---|---|
| `formProps` | `{ initialValues: modelo, onFinish: guardar }`. Se esparce en `EdForm`. |
| `modelo` | Registro cargado (o `null` al agregar). |
| `isLoading` | `true` mientras carga el registro. |
| `isGuardando` | `true` mientras guarda. Usar en el `loading` del botón. |
| `isEditando` | `true` si hay `id`. |
| `submitLabel` | "Guardar X" / "Actualizar X". |
| `mostrarSubmit` | Mostrar u ocultar el botón (formularios embebidos). |
| `queryProps` | Lo que se pase como `formQueryProps` en detalle. |

## Firma

```tsx
export default function Formulario({
  mostrarSubmit = true,
  submitLabel = "Guardar",
  isLoading = false,
  isGuardando = false,
  formProps,
  modelo,
}: IFormularioBaseProps) {
```

## Cargar valores y transformar al guardar

- `initialValues` solo aplica en el primer render y el `modelo` llega después, por eso se cargan con `useEffect` + `form.setFieldsValue(modelo)`.
- Si hay que ajustar valores antes de guardar (convertir relaciones a ids, agregar campos calculados), se hace en `onFinish` y se llama `formProps.onFinish?.(valores)`.

```tsx
useEffect(() => {
  if (modelo) form.setFieldsValue(modelo);
}, [modelo, form]);
```

```tsx
<EdForm
  {...formProps}
  form={form}
  isLoading={isLoading}
  onFinish={(v) => formProps.onFinish?.({ ...v, activo: !!v.activo })}
>
```

## Validaciones (`AntdFormValidation`)

| Regla | Ejemplo |
|---|---|
| `Requerido(mensaje?)` | `AntdFormValidation.Requerido("El nombre es obligatorio")` |
| `Correo`, `Telefono`, `CURP`, `RFC`, `Divisa` | `AntdFormValidation.Correo()` |
| `Minimo(n)`, `Maximo(n)`, `Rango(min, max)` | valores numéricos |
| `LongitudMinima(n)`, `LongitudMaxima(n)` | largo del texto |
| `ConfirmarContrasena(form, "password")` | igualdad con otro campo |
| `AntdFormValidation.REGEX.*` | `NUMEROS_ENTEROS`, `NUMEROS_DECIMALES`, `CORREO`, `TELEFONO`, `CURP`, `RFC`... para `{ pattern, message }` propios |

Alternativa: `FormItem` de react-core acepta `validate={[{ type: "requerido" }]}` en lugar de `rules`.

## `SelectorQuery` (relaciones FK)

```tsx
<Form.Item label="Categoría" name="idCategoria" rules={[AntdFormValidation.Requerido()]}>
  <SelectorQuery
    queryProps={{
      endpoint: Categoria.ENDPOINTS.DEFAULT,
      enabled: true,
      extraParams: { sort: "nombre-asc" },
    }}
    selectProps={{ placeholder: "Selecciona una categoría" }}
  />
</Form.Item>
```

| Prop | Uso |
|---|---|
| `labelProp` / `valueProp` | Propiedad a mostrar / guardar. Default `"nombre"` / `"id"`. |
| `selectProps` | Props de `Select` de antd (`mode: "multiple"`, `allowClear`...). |
| `append` / `blacklist` | Agregar / excluir opciones. |
| `onSelect(data, value)` | Acceso al registro completo seleccionado. |
| `labelRender` / `optionRender` | Render personalizado. |

## Ejemplo completo

```tsx
import { Button, Card, Col, Form, Input, Row, Switch } from "antd";
import { useForm } from "antd/lib/form/Form";
import { Icon } from "@iconify/react";
import { useEffect } from "react";
import {
  AntdFormValidation,
  EdForm,
  IFormularioBaseProps,
} from "@e-desarrollos/react-core";
import { SelectorQuery } from "@base/components/form/SelectorQuery/SelectorQuery";
import { Categoria } from "@/models/Categoria.model";

export default function Formulario({
  mostrarSubmit = true,
  submitLabel = "Guardar",
  isLoading = false,
  isGuardando = false,
  formProps,
  modelo,
}: IFormularioBaseProps) {
  const [form] = useForm();

  useEffect(() => {
    if (modelo) form.setFieldsValue(modelo);
  }, [modelo, form]);

  return (
    <EdForm
      {...formProps}
      form={form}
      isLoading={isLoading}
      className="bg-transparent border-none p-0"
    >
      <Row gutter={[24, 24]}>
        <Col span={24}>
          <Card
            title={
              <div className="flex items-center gap-3 py-1">
                <Icon icon="lucide:badge-check" className="text-blue-600 text-xl" />
                <span className="text-lg tracking-tight">Datos generales</span>
              </div>
            }
            className="border-l-4 border-l-blue-600"
          >
            <Row gutter={[24, 0]}>
              <Col span={24} md={12}>
                <Form.Item
                  label="Nombre"
                  name="nombre"
                  rules={[AntdFormValidation.Requerido("El nombre es obligatorio")]}
                >
                  <Input placeholder="Nombre" />
                </Form.Item>
              </Col>

              <Col span={24} md={12}>
                <Form.Item
                  label="Categoría"
                  name="idCategoria"
                  rules={[AntdFormValidation.Requerido("La categoría es obligatoria")]}
                >
                  <SelectorQuery
                    queryProps={{ endpoint: Categoria.ENDPOINTS.DEFAULT, enabled: true }}
                    selectProps={{ placeholder: "Selecciona una categoría" }}
                  />
                </Form.Item>
              </Col>

              <Col span={24}>
                <Form.Item label="Descripción" name="descripcion">
                  <Input.TextArea rows={3} placeholder="Descripción" />
                </Form.Item>
              </Col>

              <Col span={24} md={6}>
                <Form.Item label="Activo" name="activo" valuePropName="checked">
                  <Switch />
                </Form.Item>
              </Col>
            </Row>
          </Card>
        </Col>

        {mostrarSubmit && (
          <Col span={24}>
            <Button type="primary" htmlType="submit" loading={isGuardando} className="h-12 px-20">
              <Icon icon="lucide:check-circle" className="text-xl mr-2" />
              {submitLabel}
            </Button>
          </Col>
        )}
      </Row>
    </EdForm>
  );
}
```

## Checklist

- [ ] `export default function Formulario` tipado con `IFormularioBaseProps`.
- [ ] `EdForm` con `{...formProps}`, `form` e `isLoading`.
- [ ] `useEffect` que carga `modelo` en el form.
- [ ] Cada `name` coincide con una propiedad del modelo.
- [ ] Obligatorios con `AntdFormValidation.Requerido`.
- [ ] FK con `SelectorQuery` usando `Modelo.ENDPOINTS.DEFAULT`.
- [ ] Botón submit con `loading={isGuardando}` y `submitLabel`, respetando `mostrarSubmit`.
- [ ] Menos de 600 líneas.
