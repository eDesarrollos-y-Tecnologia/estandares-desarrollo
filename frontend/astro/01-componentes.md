# Componentes React

**Cuándo usarlo:** en cualquier componente `.tsx` (islas del admin, bloques del
sitio público). Para listados y formularios CRUD, ver además
[03-listado.md](./03-listado.md) y [04-formulario.md](./04-formulario.md).

## Reglas

- Usa un componente Astro si solo es markup estático. React solo cuando hace
  falta interactividad.
- Tipado `React.FC<Props>`, con la interfaz `Props` definida arriba del
  componente.
- Orden interno:
  1. `useState`, variables derivadas (`id`, constantes del modelo) y hooks
     (`useForm`, `useRef`, `useStore`).
  2. Funciones y handlers con `useCallback` (las llamadas a la API van aquí).
  3. `useEffect` justo antes del `return`.
- `export default` al final del archivo.
- **La API va por `useRecurso`** (`src/hooks/useRecurso.ts`). Recibe la clase
  del modelo (y opcionalmente otro endpoint) y devuelve
  `{ cargando, obtener, obtenerPorId, guardar, eliminar }`. Cada función ya trae
  el `try/catch/finally`, el `toast.error` con `HttpService.getMensajeError` y
  el `toast.success`. Los mensajes por defecto son neutros ("Registro guardado
  correctamente"); se cambian con el último parámetro opcional
  `{ exito, error }`.
  - `obtener(params?)` → `T[]` · `obtenerPorId(id, expand?)` → `T | null` ·
    `guardar(body)` → `T | null` · `eliminar(id)` → `boolean`.
  - El componente decide qué hacer con el resultado (navegar, `form.reset`,
    recargar la tabla). El hook no navega ni toca el formulario.

```tsx
const { cargando, obtener } = useRecurso(ModelClass);

const get = useCallback(async () => {
  setDatos(await obtener({ expand: ModelClass.EXPAND.DEFAULT }));
}, [obtener]);
```

- No se crea un hook por recurso (`useServicio`, `usePermiso`…). Solo si un
  recurso tiene lógica propia (ej. cargar datos relacionados) se crea un hook
  específico, y ese hook usa `useRecurso` por dentro.
- Para llamadas que no encajan en el CRUD (`postFormData`, `getBlob`, endpoints
  especiales) se usa el singleton `http` (`@/services`) con este esqueleto:

```tsx
const get = useCallback(async () => {
  setCargando(true);
  try {
    const res = await http.get(ModelClass.ENDPOINTS.DEFAULT, {
      expand: ModelClass.EXPAND.DEFAULT,
    });

    if (res.isError) {
      toast.error(
        HttpService.getMensajeError(res, "Ocurrió un problema al obtener ..."),
      );
      return;
    }

    setDatos(res.resultado as ModelClass[]);
  } catch {
    toast.error("Ocurrió un problema al obtener ...");
  } finally {
    setCargando(false);
  }
}, []);
```

- Notificaciones con `toast` de `sonner`. Navegación con `navigate` de
  `astro:transitions/client`.
- La UI se arma con componentes shadcn de `@/components/ui/*` e iconos de
  `lucide-react`.
- Nombres, textos y variables en **español** (`cargando`, `editando`,
  `onFinish`).

## Evitar

- Usar `fetch` directo o leer el token a mano: `http` ya agrega el
  `Authorization`.
- Repetir el bloque `setCargando → try → http.X → toast` para
  get/guardar/eliminar: eso lo hace `useRecurso`.
- Leer `import.meta.env`: se usan las constantes de `src/constants/index.ts`.

## Referencias

- `src/hooks/useRecurso.ts`
- `src/services/httpService.ts` (`http`, `HttpService.getMensajeError`)
- `src/pages/freshii-admin/permisos/_components/`
