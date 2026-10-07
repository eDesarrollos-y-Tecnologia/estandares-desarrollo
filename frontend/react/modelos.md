# Modelos

Cada entidad de la API tiene una clase en `src/models/Nombre.model.ts` (PascalCase, singular).
El modelo concentra todo lo que index, detalle y formulario necesitan: nombre, ruta, endpoints, columnas y relaciones a expandir. Así no se crean interfaces sueltas dentro de los componentes.

## Orden del archivo

1. Imports: `ModeloBase` y `ModelColumnsType` de `@e-desarrollos/react-core`, y los modelos de las relaciones.
2. `export class Nombre extends ModeloBase` (hereda `id`, `creado`, `modificado`).
3. **Atributos**: opcionales (`?`). Los campos obligatorios pueden llevar valor por defecto (`nombre: string = ""`).
4. **Relaciones**: tipadas con su modelo (`imagenDestacada?: Media`, `direcciones: ClienteDireccion[] = []`).
5. Dentro de `//#region STATICS DEFAULT PARA CRUDS`:
   1. `CLASS_NAME`
   2. `BASE_ROUTE`
   3. `ENDPOINTS`
   4. `COLUMNS`
   5. `EXPAND`
   6. helpers estáticos opcionales (ej. `Cotizacion.formatearNombreCliente`)
   7. `fromJson`
   8. `fromJsonList`
6. `constructor`.

## Statics

| Static | Descripción |
|---|---|
| `CLASS_NAME` | Nombre singular legible. Se usa en títulos, botones y mensajes (`Agregar ${CLASS_NAME}`). |
| `BASE_ROUTE` | Ruta del módulo **igual** a la carpeta sin `(auth)`: `/categorias`, `/catalogos/clientes`. |
| `ENDPOINTS` | Objeto clave → endpoint. `DEFAULT` es obligatorio (listar, obtener, guardar y eliminar). Se pueden agregar otros, ej. `GUARDAR`. Siempre empiezan con `/`. |
| `COLUMNS` | Columnas que muestra `TablaQuery` en el index (ver abajo). |
| `EXPAND` | Relaciones que la API debe incluir, separadas por coma. `DEFAULT` es obligatorio (puede ser `""`). Puede haber otros, ej. `EXPAND.PERMISOS`. |

## `ModelColumnsType`

Lo que acepta cada columna (lo interpreta `base/components/tables/column-renderer/column-renderer.tsx`):

| Propiedad | Uso |
|---|---|
| `key` | Propiedad a mostrar. Admite anidadas: `"cliente.nombre"`, `"modulo.nombre"`. |
| `title` | Encabezado de la columna. `""` si solo se usa para un render personalizado. |
| `type` | `"date"` → `DD-MM-YYYY`; `"currency"` → formato moneda; `"boolean"` → usa `options`. |
| `options` | `Map` para `type: "boolean"`, ej. `new Map([["true", "Sí"], ["false", "No"]])`. |
| `default` | Texto si el valor viene vacío (si no, muestra `-`). |
| `dataIndex` | Solo si difiere de `key`. |

Por defecto cada celda es un link a `detalle?id=...`.

## Ejemplo completo

```ts
import { ModelColumnsType, ModeloBase } from "@e-desarrollos/react-core";
import { Media } from "./Media.model";

export class Categoria extends ModeloBase {
  nombre: string = "";
  descripcion?: string = "";
  activo?: boolean;
  idImagenDestacada?: string;

  imagenDestacada?: Media;

  //#region STATICS DEFAULT PARA CRUDS
  static CLASS_NAME = "Categoria";
  static BASE_ROUTE = "/categorias";

  static ENDPOINTS = {
    DEFAULT: "/v1/categoria.json",
  };

  static COLUMNS: ModelColumnsType[] = [
    { key: "idImagenDestacada", title: "" },
    { key: "nombre", title: "Nombre" },
    { key: "descripcion", title: "Descripción" },
    { key: "activo", title: "Activo" },
  ];

  static EXPAND = {
    DEFAULT: "media, imagenDestacada",
  };

  static fromJson(data: Partial<Categoria>) {
    return new Categoria(data);
  }

  static fromJsonList(data: Partial<Categoria>[]) {
    return data.map((_data) => new Categoria(_data));
  }
  //#endregion

  constructor(data: Partial<Categoria> = {}) {
    super();
    Object.assign(this, data);
  }
}
```

Ejemplo de columnas con tipos y relación anidada (`Cotizacion.model.ts`):

```ts
static COLUMNS: ModelColumnsType[] = [
  { key: "cliente.nombre", title: "Cliente" },
  { key: "folio", title: "Folio" },
  { key: "monto", title: "Monto", type: "currency" },
  { key: "estatus", title: "Estatus" },
];
```

## Checklist

- [ ] Archivo `src/models/Nombre.model.ts` y clase que extiende `ModeloBase`.
- [ ] Antes de crear el modelo, revisar que no exista ya en `src/models/`.
- [ ] Relaciones tipadas con el modelo correspondiente (no `any` si el modelo existe).
- [ ] `BASE_ROUTE` igual a la ruta de la carpeta.
- [ ] Todo endpoint empieza con `/`.
- [ ] Cada relación usada en columnas (`cliente.nombre`) o formulario está en `EXPAND`.
- [ ] `fromJson`, `fromJsonList` y constructor con `super()` + `Object.assign(this, data)`.
