# Uso de directivas client: ,script y style dentro de astro

**client**

1.client:idle
Prioridad: Media
Útil para: Elementos de UI de menor prioridad que no necesitan ser interactivos inmediatamente.
Carga e hidrata el componente JavaScript una vez que la página haya terminado con su carga inicial y se haya activado el evento requestIdleCallback. Si está en un navegador que no es compatible con requestIdleCallback, entonces se usará el evento load.

<ShowHideButton client:idle />

2.client:load
Prioridad: Alta
Útil para: Elementos de la UI inmediatamente visibles que deben ser interactivos lo antes posible.
Carga e hidrata el JavaScript del componente inmediatamente al cargar la página.

<BuyButton client:load />

3.client:only="framework"
client:only={string} evita la renderización del servidor HTML y solo se renderiza en el cliente. Actúa de manera similar a client:load en el sentido de que carga, procesa e hidrata el componente inmediatamente al cargar la página.

¡Debes pasar el framework correcto al componente! Debido a que Astro no ejecuta el componente durante su compilación/en el servidor, Astro no sabe qué framework usa el componente a menos que se lo indiques explícitamente.

<SomeReactComponent client:only="react" />
<SomePreactComponent client:only="preact" />
<SomeSvelteComponent client:only="svelte" />
<SomeVueComponent client:only="vue" />
<SomeSolidComponent client:only="solid-js" />
<SomeLitComponent client:only="lit" />

Mostrar contenido de carga
Para los componentes que se renderizan solo en el cliente, también es posible mostrar contenido alternativo mientras se están cargando. Usa slot="fallback" en cualquier elemento hijo para crear contenido que se mostrará solo hasta que tu componente en el cliente esté disponible:

<ClientComponent client:only="vue">
  <div slot="fallback">Cargando</div>
</ClientComponent>

4.client:visible
Prioridad: Baja
Útil para: Elementos de la interfaz de usuario de baja prioridad que se encuentran en la parte inferior de la página (“que no son visibles al usuario”) o que requieren tantos recursos para cargar que preferiría no cargarlos en absoluto si el usuario nunca vio el elemento.
Carga e hidrata el JavaScript del componente una vez que haya ingresado al viewport del usuario. Esto utiliza un IntersectionObserver internamente para realizar un seguimiento de la visibilidad.

<HeavyImageCarousel client:visible />

5.client:media
Prioridad: Baja
Útil para: Alternar las barras de navegación u otros elementos que solo deben mostrarse en ciertos tamaños de pantalla.
client:media={string} carga e hidrata el componente JavaScript una vez que se cumple una determinada media query de CSS.

Nota

Si el componente ya está oculto y se muestra mediante media queries de CSS, entonces puede ser más fácil simplemente usar client:visible y no pasar la misma media query a la directiva.

<SidebarToggle client:media="(max-width: 50em)" />


**script y style**

1.is:global
De forma predeterminada, Astro aplica automáticamente las reglas CSS <style> localmente al componente. Puedes optar por no participar con este comportamiento con la directiva is:global.

is:global hace que el contenido de una etiqueta <style> se aplique globalmente en la página cuando el componente es incluido. Esto deshabilita el alcance local de CSS de Astro. Esto es equivalente a envolver todos los selectores dentro de una etiqueta <style> con :global().

Puedes combinar <style> y <style is:global> juntos en el mismo componente para crear algunas reglas de estilo globales mientras mantienes la mayor parte del CSS con un alcance local dentro del componente de Astro.

Consulta la página Estilos & CSS para obtener más detalles sobre cómo funcionan los estilos globales.
<style is:global>
  body a { color: red; }
</style>

2.is:inline
De forma predeterminada, Astro procesará, optimizará y empaquetará cualquier etiqueta <script> y <style> que vea en la página. Puedes optar por evitar este comportamiento con la directiva is:inline.

is:inline le indica a Astro que deje la etiqueta <script> o <style> tal como está en el HTML final. Los contenidos no serán procesados, optimizados o agrupados. Esto limita algunas características de Astro, como importar un paquete npm o usar un lenguaje de compilación a CSS como Sass.

La directiva is:inline significa que las etiquetas <style> y <script>:

No se empaquetarán como un archivo externo. Esto significa que atributos tales como defer que controlan la carga de archivos externos, no tendrán efecto.
No se deduplicarán: el elemento aparecerá tantas veces como se represente.
No se resolverán sus referencias import/@import/url() en relación con el archivo .astro.
Se renderizarán en el HTML final exactamente donde se crearon.
Los estilos serán globales y no tendrán alcance local en el componente.
Precaución

La directiva is:inline está implícita cada vez que se usa cualquier atributo que no sea src en una etiqueta <script> o <style>.

<style is:inline>
  /* inline: Las importaciones de paquetes relativos y npm no son compatibles. */
  @import '/assets/some-public-styles.css';
  span { color: green; }
</style>

<script is:inline>
  /* inline: Las importaciones de paquetes relativos y npm no son compatibles. */
  console.log('Estoy inline aquí en el HTML generado.');
</script>
