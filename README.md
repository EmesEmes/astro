# Astro

Astro es un marco de trabajo moderno para la construcción de sitios web rápidos y eficientes. Se centra en la entrega de una experiencia óptima tanto para desarrolladores como para usuarios finales, permitiendo la creación de sitios web estáticos con componentes reactivos donde sea necesario. 

Astro permite escribir componentes usando Vue, React, Svelte, y otros frameworks populares, pero sirve estos componentes como HTML estático siempre que sea posible, cargando JavaScript solo cuando es necesario. Esto resulta en tiempos de carga más rápidos y una mejor eficiencia energética, lo que contribuye a una web más sostenible.

Algunas de las características importantes de Astro incluyen:

1. Integración con múltiples frameworks: Astro permite utilizar varios frameworks de frontend como React, Vue, Svelte, y Preact en un mismo proyecto, facilitando el uso de los mejores aspectos de cada uno.

2. Renderizado estático y carga diferida: Astro genera sitios como HTML estático por defecto, lo que mejora la velocidad de carga. Además, carga JavaScript solo cuando es necesario, optimizando aún más el rendimiento.

3. Soporte para TypeScript: Astro ofrece soporte completo para TypeScript, permitiendo a los desarrolladores aprovechar las ventajas de la tipificación estática para escribir código más seguro y mantenible.

4. Optimización de imágenes: Incluye herramientas para optimizar automáticamente las imágenes, reduciendo su tamaño sin comprometer la calidad, lo que mejora la velocidad de carga de las páginas.

5. Componentes de islas: Permite a los desarrolladores crear "islas" interactivas de JavaScript dentro de páginas estáticas, combinando la eficiencia del contenido estático con la interactividad del JavaScript cuando es necesario.

6. Soporte para CSS y Sass: Astro tiene un soporte integrado para CSS y Sass, permitiendo a los desarrolladores estilizar sus aplicaciones de manera eficiente.

7. Construcción y despliegue eficientes: Astro optimiza el proceso de construcción y despliegue, asegurando que los sitios web sean ligeros y rápidos, con tiempos de carga mínimos.

8. Desarrollo basado en componentes: Fomenta un enfoque de desarrollo basado en componentes, lo que facilita la reutilización de código y mejora la organización del proyecto.

9. Orientado a contenido y SEO Friendly: Astro está diseñado para crear sitios web que son amigables con los motores de búsqueda y se centran en el contenido, lo que es crucial para la visibilidad en línea.

> Estas características hacen de Astro una herramienta poderosa y flexible para el desarrollo web moderno, enfocándose en la eficiencia, la velocidad y la flexibilidad.

# Iniciar un proyecto con Astro
`npm create astro@latest`

# Componentes
Los componentes Astro son los componentes básicos de cualquier proyecto Astro. Son componentes de plantilla HTML únicamente sin entorno de ejecución del lado del cliente. Se puede identificar un componente Astro por su extensión de archivo: .astro.

Los componentes Astro son extremadamente flexibles. A menudo, un componente Astro contendrá alguna interfaz de usuario reutilizable en la página , como un encabezado o una tarjeta de perfil. En otras ocasiones, un componente Astro puede contener un fragmento más pequeño de HTML, como una colección de `<meta>` etiquetas comunes que facilitan el trabajo con SEO. Los componentes Astro pueden incluso contener un diseño de página completo.

Lo más importante que hay que saber sobre los componentes Astro es que no se renderizan en el cliente. Se renderizan en HTML en el momento de la compilación o a pedido mediante renderización del lado del servidor (SSR). Se puedes incluir código JavaScript dentro de la portada de un componente y todo se eliminará de la página final enviada a los navegadores de los usuarios. El resultado es un sitio más rápido, sin huella de JavaScript añadida de forma predeterminada.

Cuando un componente Astro necesita interactividad del lado del cliente, se puede agregar etiquetas HTML estándar `<script>` o componentes de UI Framework .

## Estructura del componente
Un componente Astro se compone de dos partes principales: el script del componente y la plantilla del componente . Cada parte realiza una tarea diferente, pero juntas proporcionan un marco que es fácil de usar y lo suficientemente expresivo para manejar cualquier cosa que se desee crear.

```code
---
// Component Script (JavaScript)
---
<!-- Component Template (HTML + JS Expressions) -->
```

## El script del componente/Component Script
Astro utiliza una valla de código ( --- ) para identificar el script del componente en su componente Astro. Si alguna vez ha escrito en Markdown, es posible que ya esté familiarizado con un concepto similar llamado frontmatter. La idea de Astro de un script de componente se inspiró directamente en este concepto.

Se puede utilizar el script del componente para escribir cualquier código JavaScript/TypeScript que se necesite para representar una plantilla. Esto puede incluir:

* Importar otros componentes Astro
* Importar otros componentes del framework, como React
* Importar datos, como un archivo JSON
* Obtener contenido de una API o base de datos
* Creando variables a las que harás referencia en tu plantilla

```javascript
---
import SomeAstroComponent from '../components/SomeAstroComponent.astro';
import SomeReactComponent from '../components/SomeReactComponent.jsx';
import someData from '../data/pokemon.json';

// Access passed-in component props, like `<X title="Hello, World" />`
const { title } = Astro.props;
// Fetch external data, even from a private API or database
const data = await fetch('SOME_SECRET_API_URL/users').then(r => r.json());
---
<!-- Your template here! -->
```

> La cerca de código está diseñada para garantizar que el código JavaScript/TypeScript que se escriba en ella esté "cercado". No se escapará a la aplicación frontend ni caerá en manos de los usuarios. Se puedes escribir aquí de forma segura código costoso o confidencial (como una llamada a tu base de datos privada) sin preocuparse de que termine en el navegador de los usuarios.

## La plantilla de componentes/Component Template
La plantilla del componente se encuentra debajo del límite del código y determina la salida HTML de un componente.

Si se escribe HTML simple aquí, el componente representará ese HTML en cualquier página de Astro en la que se importe y utilice.

Sin embargo, la sintaxis de la plantilla de componentes de Astro también admite expresiones de JavaScript, etiquetas Astro `<style>` y `<script>`, componentes importados y directivas especiales de Astro . Los datos y valores definidos en el script del componente se pueden utilizar en la plantilla del componente para producir HTML creado dinámicamente.

```html
---
// Your component script here!
import Banner from '../components/Banner.astro';
import ReactPokemonComponent from '../components/ReactPokemonComponent.jsx';
const myFavoritePokemon = [/* ... */];
const { title } = Astro.props;
---
<!-- HTML comments supported! -->
{/* JS comment syntax is also valid! */}

<Banner />
<h1>Hello, world!</h1>

<!-- Use props and other variables from the component script: -->
<p>{title}</p>

<!-- Include other UI framework components with a `client:` directive to hydrate: -->
<ReactPokemonComponent client:visible />

<!-- Mix HTML with JavaScript expressions, similar to JSX: -->
<ul>
  {myFavoritePokemon.map((data) => <li>{data.name}</li>)}
</ul>

<!-- Use a template directive to build class names from multiple strings or even objects! -->
<p class:list={["add", "dynamic", {classNames: true}]} />
```