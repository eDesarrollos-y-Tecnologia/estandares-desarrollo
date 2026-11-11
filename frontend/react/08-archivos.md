# Ubicación de archivos

Esta guía define en qué carpeta va cada tipo de archivo del proyecto y qué no debe ir en cada una. Sirve para decidir dónde crear un archivo nuevo y para reconocer un archivo que está fuera de lugar.

Aplica a todo archivo nuevo. Los archivos existentes que no cumplen la regla están listados en [Legado](#legado-archivos-existentes-fuera-de-la-regla). No se mueven como parte de otra tarea: se corrigen en una tarea específica.

## Regla principal

Un archivo va en la carpeta que corresponde a su **función**, no a la que resulte más cómoda.

Si un archivo podría ir en dos carpetas, usar el [árbol de decisión](#árbol-de-decisión).

## Estructura de referencia

```
src/
├── assets/              Imágenes y recursos que se importan desde el código
├── components/          Componentes React compartidos por varias vistas
│   ├── utils/           Controles pequeños reutilizables (botones, buscadores, selectores)
│   ├── validators/      Componentes que protegen rutas
│   └── <Nombre>/        Componente compuesto con subcomponentes propios
├── config/
│   └── settings/        Configuración de librerías (antd, axios, react-query, router)
├── hooks/               Hooks personalizados (use<Nombre>.tsx), en una carpeta plana
├── layout/              Layouts de página completa (estructura de la pantalla)
├── lib/
│   ├── api/             Endpoints globales y cliente HTTP
│   ├── constants/       Valores fijos reutilizables
│   ├── types/           Interfaces, tipos, enums y declaraciones de tipos
│   └── utilities/       Funciones puras de uso general (formatos, conversiones)
├── models/              Modelos de entidad (<Nombre>.model.ts)
├── providers/           Proveedores de contexto de React
├── routes/              Rutas de TanStack Router: cada archivo es una URL
│   ├── (auth)/          Vistas privadas; una carpeta por módulo
│   │   └── <modulo>/
│   │       ├── index.tsx        Listado
│   │       ├── detalle.tsx      Alta / edición
│   │       └── -components/     Piezas visuales exclusivas del módulo
│   ├── (public)/        Vistas públicas
│   └── -pages/          Vistas generales no ligadas a un módulo (inicio, errores)
├── global.css           Único archivo de estilos
├── localMenuItems.gen.ts  Menú del sidebar (ver 06-general.md)
├── main.tsx             Punto de entrada
└── routeTree.gen.ts     GENERADO. No editar.

base/                    Código genérico y reutilizable (layouts CRUD, tablas, hooks base)
public/                  Archivos estáticos que se sirven tal cual (favicon)
```

Aliases: `@/` apunta a `src/` y `@base/` apunta a `base/`. Usarlos en lugar de rutas relativas largas.

## Reglas por carpeta

### `src/components/`

**Sí va aquí:**
- Componentes React (`.tsx`) usados por más de una vista o módulo.
- Subcomponentes de un componente compartido grande, en su propia subcarpeta.

**No va aquí:**
- Tipos, interfaces, `type` o `enum`. Van en `src/lib/types/`. Excepción: las props de un componente que solo se usan en su mismo archivo pueden declararse en ese archivo.
- Modelos de entidad. Van en `src/models/`.
- Componentes exclusivos de un módulo. Van en `-components/` del módulo.
- Hooks. Van en `src/hooks/`, aunque el componente los use.
- Funciones puras o formateadores. Van en `src/lib/utilities/`.
- Llamadas HTTP Van en `src/lib/api/`.
- Endpoints van en el modelo.
- Constantes. Van en `src/lib/constants/`.
- Hojas de estilo nuevas. Solo existe `src/global.css`.

**Ejemplo mal ubicado:** `src/components/utils/usuario.types.ts`. Es un archivo de tipos y debe ir en `src/lib/types/`.

### `src/routes/`

**Sí va aquí:**
- Archivos de ruta: `index.tsx`, `detalle.tsx`, `route.tsx`, `__root.tsx`.
- Dentro de `-components/`, las piezas visuales del módulo (formularios, secciones, modales).

**No va aquí:**
- Modelos, tipos o interfaces, aunque sean del módulo. Van en `src/models/` o `src/lib/types/`.
- Hooks. Van en `src/hooks/`.
- Componentes usados fuera del módulo. Van en `src/components/`.
- Funciones de utilidad, constantes o llamadas HTTP.
- Componentes sin el prefijo `-`. TanStack Router los registra como rutas.

**Ejemplo mal ubicado:** `src/routes/(auth)/marcas/formulario.tsx`. Sin guion, este archivo se convierte en la URL `/marcas/formulario`. Debe estar en `src/routes/(auth)/marcas/-components/formulario.tsx`.

### `src/models/`

**Sí va aquí:**
- Una clase por archivo, con el nombre `<Nombre>.model.ts`, en singular y PascalCase (ver [01-modelos.md](01-modelos.md)).

**No va aquí:**
- Componentes, hooks o funciones.
- Interfaces de respuesta HTTP. Van en `src/lib/types/`.
- Constantes generales o utilidades.
- Archivos sin el sufijo `.model.ts`.

**Ejemplos mal ubicados:**
- `src/models/Marca.tsx`: es un componente, no un modelo.
- `src/models/marca-form.ts`: no sigue el nombre de modelo y probablemente es un formulario.

### `src/hooks/`

**Sí va aquí:**
- Hooks personalizados, un archivo por hook, con nombre `use<Nombre>.tsx`.
- Si el hook solo tiene codigo en typescript el archivo deberia tener la extension .ts, en caso de que retornara un jsx o un componente debe de ser .tsx

**No va aquí:**
- Componentes. Un hook no devuelve JSX.
- Tipos o interfaces. Van en `src/lib/types/`.
- Constantes o funciones puras que no usan hooks. Van en `src/lib/constants/` o `src/lib/utilities/`.
- Subcarpetas por hook. Ver [Legado](#legado-archivos-existentes-fuera-de-la-regla).

**Ejemplo mal ubicado:** `src/hooks/useFormularioMarca.tsx` que devuelve `<Form>...</Form>`. Es un componente y debe ir en `src/routes/(auth)/marcas/-components/formulario.tsx`.

### `src/lib/types/`

**Sí va aquí:**
- Interfaces (`interface`), tipos (`type`) y enums que no son modelos de entidad.
- Tipos de respuestas HTTP y tipos compartidos entre archivos.
- Declaraciones globales (`.d.ts`), como las variables de entorno.

**No va aquí:**
- Componentes o funciones.
- Valores constantes. Van en `src/lib/constants/`.
- Modelos de entidad. Si una entidad tiene clase en `src/models/`, no se duplica su interfaz aquí.

**Ejemplo mal ubicado:** `src/routes/(auth)/marcas/types.ts`. Debe ir en `src/lib/types/marca.interface.ts`.

### `src/lib/constants/`

**Sí va aquí:**
- Valores fijos reutilizables en varias partes de la aplicación: catálogos, estilos compartidos, mensajes, íconos de carga.

**No va aquí:**
- Configuración del entorno o de librerías. Va en `src/config/`.
- Tipos. Van en `src/lib/types/`.
- Funciones. Van en `src/lib/utilities/`.
- Endpoints. Van en `src/lib/api/` o en el modelo.
- Valores usados en una sola vista. Se quedan dentro de su archivo.

### `src/lib/api/`

**Sí va aquí:**
- Endpoints globales de la aplicación (por ejemplo, autenticación) y el cliente HTTP.

**No va aquí:**
- Llamadas HTTP dentro de componentes.
- Archivos de servicio por módulo. Los endpoints de un módulo se definen en su modelo (`ModelClass.ENDPOINTS`) y se consumen con `useHttp` (ver [06-general.md](06-general.md)).

### `src/lib/utilities/`

**Sí va aquí:**
- Funciones puras que no usan React: formatear dinero o fechas, convertir números a letras.

**No va aquí:**
- Hooks. Van en `src/hooks/`.
- Componentes. Van en `src/components/`.
- Llamadas HTTP o tipos.

### `src/config/`

**Sí va aquí:**
- Configuración de librerías en `settings/` (antd, axios, react-query, router).
- Lectura de variables de entorno.

**No va aquí:**
- Constantes de negocio. Van en `src/lib/constants/`.
- Tipos, componentes o lógica de la aplicación.

### `src/layout/`

**Sí va aquí:**
- Layouts de página completa: estructura de la pantalla con sidebar, encabezado y área de contenido.

**No va aquí:**
- Componentes pequeños reutilizables. Van en `src/components/`.
- Componentes de un módulo. Van en su `-components/`.

### `src/providers/`

**Sí va aquí:**
- Proveedores de contexto de React (tema, sesión).

**No va aquí:**
- Hooks que consumen el contexto. Van en `src/hooks/`.
- Componentes de interfaz.

### `src/assets/` y `public/`

**Sí va aquí:**
- En `src/assets/`: imágenes y logos que se importan desde el código.
- En `public/`: archivos que se sirven tal cual (favicon, archivos estáticos).

**No va aquí:**
- Copias de archivos. Un nombre con `copy` es señal de copia accidental.
- Nombres con espacios.
- Imágenes de prueba o archivos sin uso.

### Raíz del proyecto

**Sí va aquí:**
- Solo archivos de configuración de herramientas: `package.json`, `vite.config.ts`, `tsconfig*.json`, `eslint.config.js`, `tailwind.config.js`, `components.json`, `firebase.json`.

**No va aquí:**
- Archivos de código de la aplicación.

### `src/global.css`

**Sí va aquí:**
- Estilos globales que Tailwind no cubre (ver [06-general.md](06-general.md)).

**No va aquí:**
- Otros archivos `.css`, `.scss` o `.module.css`.

### `base/`

**Sí va aquí:**
- Código genérico reutilizable en cualquier proyecto: layouts CRUD, `TablaQuery`, `SelectorQuery`, hooks base (`usePagina`, `useHttp`, `useDetalle`).
- Dentro de `base/hooks/`, cada hook va en su propia carpeta, como en el resto de la carpeta.

**No va aquí:**
- Código específico de un módulo o del negocio: modelos, rutas, menú o llamadas a endpoints de una entidad. Ese código va en `src/`.

## Árbol de decisión

Recorrer las preguntas en orden y usar la primera respuesta afirmativa.

1. **¿Es un valor fijo que se reutiliza?** → `src/lib/constants/`
2. **¿Es una clase que representa una entidad del negocio?** → `src/models/<Nombre>.model.ts`
3. **¿Es una interfaz, tipo o enum?** → `src/lib/types/`
4. **¿Es una URL o página de la aplicación?** → `src/routes/` (privada en `(auth)/`, pública en `(public)/`, general en `-pages/`)
5. **¿Es un componente React?**
   - Solo lo usa un módulo → `-components/` de ese módulo.
   - Lo usan varias vistas o módulos → `src/components/`.
   - Es genérico para cualquier proyecto → `base/components/`.
6. **¿Es un hook que usa estado, efectos o contexto de React?** → `src/hooks/use<Nombre>.tsx`
7. **¿Es una función pura sin React?** → `src/lib/utilities/`
8. **¿Define cliente HTTP?** → `src/lib/api/`
9. **¿Configura una librería o las variables de entorno?** → `src/config/`
10. **¿Es un layout de página completa?** → `src/layout/`
11. **¿Es un proveedor de contexto?** → `src/providers/`
12. **¿Es una imagen o un archivo estático?** → `src/assets/` o `public/`
13. **¿Son estilos?** → `src/global.css` (o Tailwind en el componente)

Si ninguna pregunta aplica, no crear el archivo: revisar primero si la necesidad ya está cubierta.

## Nombres de archivo

Esta guía no impone una convención de nombres para todo el proyecto. Al crear un archivo, usar el mismo estilo que los archivos vecinos de su carpeta.

Estos sufijos y prefijos sí son obligatorios:

| Elemento | Regla | Ejemplo |
|---|---|---|
| Modelo | `<Nombre>.model.ts` | `Marca.model.ts` |
| Hook | `use<Nombre>.tsx` | `useModalState.tsx` |
| Carpeta o archivo que no es ruta | Prefijo `-` | `-components/`, `-pages/` |
| Ruta de módulo | Nombre = segmento de la URL | `detalle.tsx` → `/marcas/detalle` |
| Declaración global | `.d.ts` | `env.d.ts` |

## Errores comunes

| Archivo mal ubicado | Problema | Ubicación correcta |
|---|---|---|
| `src/components/usuario.types.ts` | Tipos dentro de componentes | `src/lib/types/usuario.interface.ts` |
| `src/components/Marca.model.ts` | Modelo fuera de `models/` | `src/models/Marca.model.ts` |
| `src/routes/(auth)/marcas/formulario.tsx` | Sin guion, se registra como ruta | `src/routes/(auth)/marcas/-components/formulario.tsx` |
| `src/hooks/useFormularioMarca.tsx` (devuelve JSX) | Un hook que devuelve interfaz | `src/routes/(auth)/marcas/-components/formulario.tsx` |
| `src/lib/utilities/usuario.ts` con `interface IUsuario` | Tipos dentro de utilidades | `src/lib/types/usuario.interface.ts` |
| `src/components/formatear-fecha.ts` | Función pura dentro de componentes | `src/lib/utilities/formatear-fecha.ts` |
| `src/models/MarcaForm.tsx` | Componente dentro de `models/` | `src/routes/(auth)/marcas/-components/formulario.tsx` |
| `src/routes/(auth)/marcas/-components/marca-api.ts` | Servicio HTTP por módulo | Usar `Marca.ENDPOINTS` con `useHttp` |
| `src/components/estilos.css` | CSS fuera de `global.css` | Tailwind o `src/global.css` |
| `src/assets/imagen copy.png` | Copia accidental con espacio | Eliminar o renombrar |
| `base/.../marcas.tsx` | Código de negocio en `base/` | `src/routes/(auth)/marcas/` |
| Nuevo archivo en `src/config/constants/` | Carpeta legada | `src/lib/constants/` |

## Antes de crear un archivo

- [ ] Recorrí el árbol de decisión y la carpeta elegida corresponde a la función del archivo.
- [ ] Busqué si ya existe un componente, hook o utilidad que pueda reutilizar (ver [06-general.md](06-general.md)).
- [ ] El archivo tiene una sola responsabilidad (ver [07-organizacion-componentes.md](07-organizacion-componentes.md)).
- [ ] Si está en `components/`, `hooks/`, `lib/utilities/` o `lib/constants/`, no contiene tipos que deban ir en `lib/types/`.
- [ ] Si está en `routes/`, las piezas que no son ruta tienen el prefijo `-`.
- [ ] El nombre sigue el estilo de los archivos vecinos y los sufijos obligatorios.
- [ ] No creé una carpeta nueva. Si fue necesaria, la agregué a la estructura de esta guía.

## Carpetas nuevas

Crear una carpeta solo cuando ninguna de las existentes aplica. En ese caso:

1. Agregarla a la estructura de referencia de esta guía, con una descripción de su contenido.
2. Explicar el motivo en el pull request o commit.

No crear carpetas vacías ni carpetas "por si acaso".

## Legado: archivos existentes fuera de la regla

Estos archivos ya existen en `base-react-vite-ts` y no cumplen la regla. No se deben usar como ejemplo ni ampliar. Corregirlos en una tarea específica.

| Ubicación actual | Situación | Para archivos nuevos |
|---|---|---|
| `src/hooks/useAuth.tsx` y `src/hooks/useAuth/useAuth.tsx` | Hook duplicado; uno está en subcarpeta | Usar `src/hooks/` plano. Revisar cuál usa la aplicación antes de tocar cualquiera. |
| `src/utils/` | Carpeta legada | `src/lib/utilities/` |
| `src/constants/` | Carpeta legada | `src/lib/constants/` |
| `src/config/constants/` | Constantes y variables de entorno mezcladas | Constantes en `src/lib/constants/`, entorno en `src/config/` |
| `src/components/layout/` | Layouts dentro de componentes | `src/layout/` |
| `src/routes/-pages/NotFoundPage.tsx` y `src/routes/-pages/utils/not-found-page.tsx` | Dos páginas "no encontrado", ambas en uso | Unificar en una tarea específica |
| `src/routes/-pages/Login.tsx` y `src/routes/-pages/home/login-form.tsx` | Dos piezas de login, ambas en uso | Separar responsabilidades en una tarea específica |
| `ILoginForm` en `src/routes/-pages/home/login-form.tsx`, importado por `src/hooks/useAuth.tsx` | Tipo definido en una vista | Mover a `src/lib/types/` |
| `src/assets/imagen-no-disponible copy.png` | Copia accidental | Eliminar en una tarea específica |
| `src/lib/types/usuario.interface.ts` y `src/models/Usuario.model.ts` | Interfaz duplicada del modelo | Usar el modelo como fuente de verdad |
