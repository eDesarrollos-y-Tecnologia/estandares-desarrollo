# Reglas generales

## Estructura del proyecto

La ubicación de cada tipo de archivo (qué va en cada carpeta y qué no) está en [08-archivos.md](08-archivos.md). Consultarla antes de crear un archivo nuevo.

Aliases: `@/` = `src/`, `@base/` = `base/`. Usarlos siempre en lugar de rutas relativas largas.

## Estilos

- Solo existe un archivo CSS: `src/global.css`. No crear otros.
- Estilos de componentes con Tailwind; usar CSS en `global.css` solo si Tailwind no alcanza.
- Colores y tema de antd: `src/config/settings/antd-config-provider.tsx`. Para personalización mayor, `src/global.css`.

## Tamaño y organización de archivos

- Ningún archivo debe pasar de 600 líneas. Para lograrlo:
  - separar secciones de la vista en `-components/` del módulo;
  - usar los modelos existentes en lugar de crear interfaces dentro de los componentes;
  - antes de crear un componente, buscar si ya existe en `base/components`, `src/components` o `@e-desarrollos/react-core` y reutilizarlo.

## Código

- Evitar código comentado y comentarios innecesarios.
- Orden dentro de los componentes: hooks/estado/variables → funciones → `useMemo` → `useEffect` → `return`.
- No escribir rutas ni endpoints a mano: usar `ModelClass.BASE_ROUTE` y `ModelClass.ENDPOINTS`.

## Formato del código

El código debe ser fácil de leer. Un código más largo pero claro es correcto; un código compacto que obliga a descifrarlo no lo es. Nunca compactar el código para reducir el número de líneas.

### Indentación

- Usar **2 espacios** por nivel de indentación. No usar tabulaciones ni 4 espacios.
- Cada bloque anidado suma 2 espacios: funciones, condiciones, ciclos, objetos, arreglos y JSX anidado.

### Reglas

1. **Ningún bloque en una sola línea.** Funciones, condiciones, ciclos, objetos con más de una propiedad y JSX con atributos ocupan varias líneas.
2. **Una sentencia por línea.** No separar instrucciones con `;` en la misma línea.
3. **Propiedades y elementos en líneas separadas** cuando hay más de uno.
4. **Siempre usar llaves** en condicionales, incluso si el bloque tiene una sola instrucción.
5. **Encadenamiento en varias líneas.** Un método por línea, con la misma indentación.
6. **Argumentos largos en líneas separadas.** Si una llamada o firma no cabe en una línea clara, cada argumento va en su propia línea.
7. **JSX ordenado.** Cada atributo en su línea cuando hay más de uno, y los hijos en líneas separadas.
8. **Sin ternarios anidados.** Usar `if`, variables intermedias o un objeto de búsqueda.
9. **Condiciones complejas en variables** con nombre descriptivo, en lugar de encadenarlas en una línea.
10. **Líneas en blanco entre bloques lógicos** dentro de una función.

Excepción: expresiones simples que se leen de inmediato, como `const total = precio * cantidad`, y los `import` o `export` de una línea.

### Ejemplos

Mal: todo en una línea.

```tsx
const opciones = marcas.filter((m) => m.activo).map((m) => ({ value: m.id, label: m.nombre })); if (!opciones.length) return null;
```

Bien:

```tsx
const opciones = marcas
  .filter((marca) => marca.activo)
  .map((marca) => ({
    value: marca.id,
    label: marca.nombre,
  }));

if (opciones.length === 0) {
  return null;
}
```

Mal: JSX con atributos en una línea.

```tsx
<Button type="primary" onClick={() => guardar(valores)} loading={cargando} disabled={!valido}>Guardar</Button>
```

Bien:

```tsx
<Button
  type="primary"
  loading={cargando}
  disabled={!valido}
  onClick={() => guardar(valores)}
>
  Guardar
</Button>
```

Mal: ternarios anidados.

```ts
const etiqueta = estado === "A" ? "Activo" : estado === "B" ? "Baja" : "Otro";
```

Bien: objeto de búsqueda.

```ts
const etiquetas: Record<string, string> = {
  A: "Activo",
  B: "Baja",
};

const etiqueta = etiquetas[estado] ?? "Otro";
```

### Verificación

Antes de terminar un cambio, comprobar que:

- [ ] Todo el archivo usa indentación de 2 espacios.
- [ ] Ninguna función, condición u objeto está escrito en una sola línea.
- [ ] Cada sentencia termina en su propia línea.
- [ ] No hay ternarios anidados ni condiciones largas en una sola línea.
- [ ] Los atributos JSX y los argumentos largos están uno por línea.
- [ ] El código se entiende sin necesidad de ejecutarlo.

Los archivos generados, como `src/routeTree.gen.ts`, no se editan a mano.

## Menú (`src/localMenuItems.gen.ts`)

- Define las rutas que aparecen en el sidebar; el `label` también se usa en los breadcrumbs.
- Toda entrada lleva un icono acorde al nombre (iconify, preferentemente `lucide:*`).
- El `key` es la ruta (igual al `BASE_ROUTE` del modelo). Los submódulos van en `children`.

## Otros

- Las plantillas de `base/scripts/templates/modelo/` (interface + hook) son de una versión anterior y no se usan. Para modelos seguir [01-modelos.md](01-modelos.md).
