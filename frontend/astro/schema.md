# Schema de validación (Zod)

**Cuándo usarlo:** para cada formulario, en `src/schemas/<Nombre>Schema.ts`.

## Reglas

- Zod v4 (`import { z } from "zod"`). El mensaje de tipo se pasa con
  `{ error: "..." }`.
- Para un campo obligatorio:
  `.string({ error }).min(1, "X es obligatorio").trim()`.
- Para un campo opcional: `.string().trim().optional()`. Si el backend manda
  `null`, se agrega `.nullable()`.
- Un id de relación (FK desde `QuerySelector`) es `z.string(...).min(1, ...)`.
- Validaciones entre campos con `.refine(fn, { message, path: ["campo"] })`.
- Para formatos (correo, teléfono, CP) se reutiliza `REGEX` de
  `src/constants/index.ts` o `z.email()`.
- Exporta `<Nombre>Schema` y
  `type <Nombre>FormData = z.output<typeof <Nombre>Schema>`.

## Plantilla

```ts
import { z } from "zod";

export const ServicioSchema = z.object({
  nombre: z
    .string({ error: "Por favor, ingresa el nombre" })
    .min(1, "El nombre es obligatorio")
    .trim(),
  idCategoria: z
    .string({ error: "La categoría es obligatoria" })
    .min(1, "La categoría es obligatoria"),
  descripcion: z.string().trim().optional(),
});

export type ServicioFormData = z.output<typeof ServicioSchema>;
```

## Evitar

- `{ message: "..." }` en el constructor del tipo (forma vieja; `ModuloSchema` y
  `PermisoSchema` aún la usan).
- Exigir campos que el formulario no muestra en algún modo. Por ejemplo,
  `UsuarioSchema` pide `pwd`/`pwdConfirm` siempre, pero en edición esos campos
  no se muestran. Para eso hay que usar `.optional()` o un schema distinto para
  editar.

## Referencias

- `src/schemas/UsuarioSchema.ts` (`{ error }`, `.refine`)
- `src/schemas/PermisoSchema.ts`
