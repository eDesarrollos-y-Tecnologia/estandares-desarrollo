# Modelo

**Cuándo usarlo:** al agregar una entidad del backend en
`src/models/<Nombre>.model.ts`.

## Reglas

- Extiende de `ModeloBase` (`src/types/ModeloBase.model.ts`). `id`, `creado` y
  `modificado` ya vienen heredados.
- Todas las propiedades son **opcionales** (`?`). Primero van los ids (`idX`),
  luego los campos y al final las relaciones tipadas con `import type`.
- `CLASS_NAME`: nombre legible de la clase, separado con espacios. Se usa en
  títulos de la UI (`"Servicio Media"`).
- `BASE_ROUTE`: ruta de la carpeta del admin, `"/freshii-admin/<carpeta>"` (ej.
  `"/freshii-admin/permisos"`). `Tabla` y los formularios navegan con este
  valor.
- `ENDPOINTS.DEFAULT`: `"v1/<kebab-case>.json"` (ej. `v1/servicio-media.json`).
  Si el sitio público consulta la entidad, se agrega
  `PUBLICO: "web/<kebab-case>.json"`.
- `EXPAND.DEFAULT`: vacío (`""`) salvo que el listado o el formulario necesiten
  relaciones; en ese caso, nombres separados por coma
  (`"categoria,imagenDestacada"`). Para otros casos se agregan llaves extra
  (`CATEGORIA`, `PERMISOS`).
- `COLUMNS`: una columna por propiedad que se muestra en la tabla, **sin ids**.
  Las relaciones se muestran con `accessorFn` y fallback `"-"`, y las fechas se
  formatean con `dayjs`.
- Los enums del dominio van como `static` en el modelo (ej.
  `Usuario.ROLES = [{ value, label }]`).
- `fromJson`, `fromJsonList` y el `constructor` son obligatorios.

## Plantilla

```ts
import { type ModelColumnsType, ModeloBase } from "@/types/ModeloBase.model";
import dayjs from "dayjs";
import type { Servicio } from "./Servicio.model";
import type { Empleado } from "./Empleado.model";

export class Cita extends ModeloBase {
  idServicio?: string;
  idEmpleado?: string;

  fecha?: string;
  horaInicio?: string;
  precioTotal?: number;
  estatus?: string;

  servicio?: Servicio;
  empleado?: Empleado;

  static CLASS_NAME = "Cita";
  static BASE_ROUTE = "/freshii-admin/cita";

  static ENDPOINTS = {
    DEFAULT: "v1/cita.json",
  };

  static EXPAND = {
    DEFAULT: "servicio,empleado",
  };

  static COLUMNS: ModelColumnsType<Cita>[] = [
    {
      accessorKey: "fecha",
      header: "Fecha",
      accessorFn: (
        row,
      ) => (row.fecha ? dayjs(row.fecha).format("DD/MM/YYYY") : "-"),
    },
    {
      accessorKey: "servicio",
      header: "Servicio",
      accessorFn: (row) => row.servicio?.nombre ?? "-",
    },
    {
      accessorKey: "estatus",
      header: "Estatus",
    },
  ];

  static fromJson(data: Partial<Cita>) {
    return new Cita(data);
  }

  static fromJsonList(data: Partial<Cita>[]) {
    return data.map((_data) => new Cita(_data));
  }

  constructor(data: Partial<Cita> = {}) {
    super();
    Object.assign(this, data);
  }
}
```

## Evitar

- Escribir endpoints o rutas directamente en los componentes. Siempre se usan
  `Modelo.ENDPOINTS` / `Modelo.BASE_ROUTE`.
- Poner `BASE_ROUTE = "/<modelo>"` sin el prefijo `/freshii-admin`.
- Propiedades obligatorias (sin `?`): rompen `Partial<T>` y `form.reset`.

## Referencias

- `src/types/ModeloBase.model.ts`
- `src/models/Servicio.model.ts` (relaciones + `PUBLICO` + `EXPAND` múltiple)
- `src/models/Usuario.model.ts` (enum estático `ROLES`)
