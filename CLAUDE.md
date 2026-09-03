# CLAUDE.md — Serenna (serenna.lat)

## Contexto del proyecto

Serenna es una tienda Shopify que vende un único producto: un suplemento capilar dirigido a mujeres en Colombia. La tienda actual fue construida con una app de Shopify y tiene problemas serios de velocidad de carga, especialmente en imágenes. El objetivo de esta reconstrucción es recrear la tienda con código a la medida (tema Shopify custom-coded, sin depender de apps pesadas) manteniendo Shopify como backend, priorizando velocidad y siguiendo buenas prácticas de SEO desde la base.

Este documento no da instrucciones literales de "qué escribir" en cada página. Da las reglas que cualquier página, componente o sección generada debe cumplir. Aplícalas de forma consistente en todo el tema, no solo en la página de inicio.

---

## Principio rector

El SEO no es una capa que se añade al final. Es una condición estructural del código: si el contenido no está en el HTML inicial, si la jerarquía de encabezados es inconsistente, o si las URLs no reflejan la arquitectura del sitio, ninguna optimización posterior lo corrige del todo. Antes de dar por terminado un componente, verifica que cumple las reglas de esta sección correspondiente.

---

## 1. Renderizado y velocidad (prioridad máxima)

- Todo el contenido textual relevante para SEO (nombres de producto, descripciones, textos de categoría, preguntas frecuentes, testimonios) debe existir en el HTML servido inicialmente, no depender de JavaScript para aparecer. Si un bloque se hidrata o se carga vía cliente, su contenido de texto debe estar igualmente presente en el marcado inicial.
- Las imágenes deben usar `loading="lazy"` salvo la imagen principal visible sin scroll (above the fold), que debe cargar de forma prioritaria (`fetchpriority="high"`, sin lazy load).
- Sirve imágenes en formatos modernos (WebP/AVIF) y en el tamaño real que se va a mostrar; evita que el navegador redimensione imágenes sobredimensionadas.
- Minimiza JavaScript de terceros y apps embebidas. Cada script adicional debe justificarse por una necesidad funcional, no por comodidad de implementación.
- Verifica siempre que, con JavaScript desactivado, el contenido esencial de la página se siga leyendo. Esa es la prueba de fuego antes de dar cualquier página por terminada.

## 2. Estructura de URLs

- Palabras separadas por guiones medios, nunca guiones bajos ni espacios.
- Jerarquía de lo genérico a lo específico: dominio → categoría (si aplica) → producto o página.
- No repitas la palabra clave completa en cada nivel de la ruta si el nivel superior ya la contiene.
- Ninguna URL debe devolver error ni quedar huérfana de su nivel superior; si se elimina o renombra una página, configura una redirección permanente (301) desde la ruta antigua.
- Las URLs deben ser legibles por una persona sin necesitar abrir la página: deben transmitir de qué trata el contenido solo con leerlas.

## 3. Titles

- Cada página tiene un `<title>` único, que contiene la palabra clave principal de esa página y solo de esa página. No reutilices el mismo title en dos URLs distintas.
- No amontones palabras clave de varias páginas en un solo title.
- Elimina cualquier texto genérico o automático que el gestor de plantillas pueda insertar por defecto (nombre de la app, texto placeholder).
- Si en algún momento el negocio tiene presencia local o geográfica relevante, ese dato se añade al title de las páginas donde corresponda; en el caso de Serenna, prioriza claridad del beneficio y la palabra clave sobre relleno de marca.

## 4. Encabezados (H1, H2, H3)

- Cada página tiene un único `<h1>`, y contiene la palabra clave principal de esa página.
- Los `<h2>` dividen el contenido en secciones lógicas; los `<h3>` subdividen dentro de una sección. La jerarquía debe leerse como el índice de un libro: nunca saltes de H1 a H3 sin H2 intermedio.
- Aprovecha los H2/H3 para introducir de forma natural palabras derivadas o relacionadas de la investigación de palabras clave, sin forzar el texto ni convertirlos en listas de keywords.
- Ningún H1 ni H2 debe estar contenido dentro de una imagen como texto no seleccionable; si un diseño requiere texto estilizado, se implementa como texto real con CSS, no como imagen.

## 5. Metadescripción

- Cada página tiene una metadescripción única, redactada para humanos (no es un factor de posicionamiento directo, pero afecta el clic).
- Aprovecha el espacio para incluir de forma natural palabras secundarias relevantes a esa página específica.
- Nunca la dejes vacía ni dependas del texto que Shopify autogenera a partir del contenido; escríbela explícitamente por plantilla o por página.

## 6. Datos estructurados (Schema.org / JSON-LD)

Implementa el marcado que corresponda según el tipo de página, sirviéndolo en el HTML inicial (no inyectado solo por JS del lado cliente):

- Página de inicio: `OnlineStore` / `Organization`.
- Ficha de producto: `Product`, con precio, disponibilidad, valoración si existen reseñas reales, e imagen.
- Si se añade contenido de blog en el futuro: `Article` o `BlogPosting`.
- Si se añaden preguntas frecuentes: `FAQPage`.
- No marques datos que no correspondan a contenido real visible en la página (por ejemplo, no declares `AggregateRating` si no hay reseñas reales mostradas).

## 7. Enlazado interno

- Menú principal: solo las páginas más importantes del sitio, presentes en toda la navegación.
- Pie de página: páginas que interesan al posicionamiento pero no son la prioridad de navegación del usuario en ese momento.
- Migas de pan: inclúyelas en cualquier página que no sea la home, reforzando la jerarquía real de la URL.
- Página de inicio: enlaza desde ahí el producto y las páginas que más quieras posicionar; es la página más rastreada del sitio.
- Evita páginas huérfanas: todo componente o sección nueva debe quedar enlazado desde al menos un punto navegable del sitio.

## 8. Contenido y arquitectura

- Cada página ataca una única intención de búsqueda. No mezcles contenido informativo (educar, explicar) con contenido transaccional (vender) en la misma URL sin dejar clara la vía de conversión.
- Las páginas orientadas a venta (producto, checkout) deben tener siempre una vía de conversión visible: llamada a la acción, formulario o botón de compra: no dejes contenido puramente explicativo sin salida comercial.
- Si se agrega una sección de blog o contenido educativo en el futuro, cada artículo debe incluir al menos un enlace hacia la página de producto o de compra.
- No dupliques contenido textual completo entre páginas (por ejemplo, la misma descripción de producto repetida en dos secciones); si necesitas reutilizar información, resúmela o enlázala en vez de copiarla literal.

## 9. Buenas prácticas técnicas generales

- HTTPS en todo el sitio, sin excepciones ni recursos mixtos (http dentro de https).
- Diseño responsive: cada componente debe verse y funcionar correctamente en móvil antes de darse por terminado, dado que la mayoría del tráfico de e-commerce en Colombia es móvil.
- Sitemap XML actualizado automáticamente al agregar o quitar páginas/productos.
- No implementes redirecciones automáticas basadas en la ubicación geográfica del visitante que impidan a los rastreadores (Google, bots de modelos de lenguaje) ver el contenido real de la página.
- Nombra los archivos de imagen de forma descriptiva (no "IMG_001.jpg") y usa siempre el atributo `alt` describiendo el contenido real de la imagen, no como relleno de palabras clave.

## 10. Checklist antes de dar una página por terminada

1. ¿El contenido esencial se ve con JavaScript desactivado?
2. ¿La URL es limpia, con guiones, y refleja la jerarquía real?
3. ¿El title es único para esta página y contiene su palabra clave?
4. ¿Hay un solo H1 correcto, y los H2/H3 ordenan el contenido sin saltos de jerarquía?
5. ¿La metadescripción es única y está escrita explícitamente?
6. ¿Están los datos estructurados que corresponden al tipo de página?
7. ¿La página está enlazada desde al menos un punto de navegación (menú, pie, home o breadcrumb)?
8. ¿Las imágenes están optimizadas, con lazy load salvo la principal, y con `alt` descriptivo?
9. ¿Esta página ataca una sola intención de búsqueda, sin competir con otra página del sitio?
10. ¿Existe una vía de conversión clara si la página es transaccional?git add .
## Identidad de marca (Manual SERENNA)

Respeta esta identidad en todo el tema. No sustituyas colores ni tipografías por defaults del framework o del theme base.

### Tipografía

- **Encabezados y títulos (H1, H2, H3, logotipo, hero):** Keroll.
  - Keroll no es una fuente de Google Fonts. Debe cargarse como `@font-face` con los archivos de fuente propios (woff2/woff). Si Codex no tiene el archivo de la fuente, debe dejarlo señalado como pendiente en vez de sustituirla silenciosamente por otra tipografía.
- **Cuerpo de texto, párrafos, UI general:** IBM Plex Sans.
  - Esta sí está disponible en Google Fonts / npm, se puede cargar de forma estándar.
- No mezclar: Keroll solo para títulos/jerarquía visual, IBM Plex Sans para todo el texto corrido, botones, formularios y microcopys.

### Paleta de colores

Colores sólidos (uso principal, marca, acentos, CTAs):
- `#F8C4BF`
- `#7E2710`
- `#D45B14`

Colores pastel (fondos, superficies suaves, secciones secundarias):
- `#FCCF9F`
- `#FAF1F0`
- `#FEE7D0`

Reglas de uso:
- Los colores sólidos son los que deben usarse para elementos de alto contraste y llamadas a la acción (botones de compra, enlaces destacados, elementos de marca).
- Los colores pastel son para fondos, separadores de sección y superficies donde no debe competir con el contenido principal.
- Mantener buen contraste de accesibilidad (WCAG AA) entre texto y fondo al combinar estos colores; si una combinación de la paleta no cumple contraste mínimo, ajustar tono/peso de fuente antes que sustituir el color por uno fuera de la paleta.
- No introducir colores nuevos fuera de esta paleta sin consultarlo primero.

Voy a levantar una ficha técnica de las tres zonas directamente desde el tema: **barra de anuncios, encabezado y Section 1**. Así te doy colores, tipografías, tamaños, espaciados, bordes y comportamiento responsive sin inventar valores.

Ya tengo los valores actuales. Ahora voy a contrastar la estructura recomendada con la documentación de Shopify para separar bien lo que debe ir en Liquid/CSS del tema y lo que todavía depende de GemPages.

Sí. Para migrarlo correctamente, separaría la página en **tres componentes independientes**: la barra de anuncios, el encabezado nativo y una nueva sección Liquid/CSS para reemplazar Section 1 de GemPages.

La inspección confirma los siguientes valores.

## 1. Barra de anuncios

Esta parte **no usa GemPages**. Es una sección nativa de Shopify ubicada antes del encabezado.

### Contenido actual

Hay dos mensajes configurados:

- **“Envíos a todo Colombia”**
- **“Recibe regalos especiales”**

Ambos anuncios no tienen enlace configurado.

La rotación automática está desactivada:

```text
Rotación automática: false
Intervalo configurado: 5
```

El intervalo de 5 segundos no tiene efecto mientras la rotación siga desactivada. No se pudo confirmar desde la configuración si se muestra siempre el primer anuncio o si existe navegación manual.

### Colores confirmados

La barra usa el esquema `scheme-4`:

```css
--announcement-background: #421314;
--announcement-text: #FFFFFF;
--announcement-button-background: #FFFFFF;
--announcement-button-text: #8E605D;
--announcement-secondary-button-text: #FFFFFF;
--announcement-shadow: #421314;
```

Visualmente, el fondo es un **borgoña/marrón oscuro** y el texto es blanco.

### Otros ajustes

```text
Línea separadora: activada
Redes sociales: desactivadas
Selector de país: desactivado
Selector de idioma: desactivado
```

La barra no tiene altura, tamaño de letra, interlineado o padding configurados directamente. Esos valores vienen del CSS interno del tema.

### Tipografía

La barra hereda la tipografía general del tema:

```css
font-family: "IBM Plex Sans", sans-serif;
font-weight: 400;
```

La configuración global indica:

```text
Fuente de encabezados: IBM Plex Sans
Fuente de cuerpo: IBM Plex Sans
Variante: ibm_plex_sans_n4
Escala de encabezados: 110
Escala del cuerpo: 105
```

Los valores `110` y `105` son escalas del tema, no tamaños directos en píxeles.

### Estructura recomendada en código

```html
<div class="announcement-bar">
  <div class="announcement-bar__inner">
    <p>Envíos a todo Colombia</p>
  </div>
</div>
```

CSS base recomendado:

```css
.announcement-bar {
  background: #421314;
  color: #ffffff;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  font-family: "IBM Plex Sans", sans-serif;
  font-weight: 400;
}

.announcement-bar__inner {
  width: min(100% - 56px, 1200px);
  margin-inline: auto;
  min-height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
}
```

La altura de `40px` sería una propuesta inicial, no un valor confirmado del tema. Para una copia exacta habría que medir la barra renderizada.

---

## 2. Encabezado

El encabezado también es **nativo de Shopify**, no de GemPages. Está ubicado inmediatamente después de la barra de anuncios.

### Estructura actual

```text
Logo en escritorio: top-center
Logo en móvil: centrado
Menú: main-menu
Tipo de menú: dropdown
Encabezado fijo: on-scroll-up
Línea separadora: activada
Esquema de color: scheme-3
Esquema del menú: scheme-3
```

`on-scroll-up` significa que el encabezado reaparece cuando el usuario se desplaza hacia arriba. No está configurado como permanentemente fijo.

### Logo

El logo actual tiene:

```text
Ancho configurado: 140px
Recurso: gid://shopify/MediaImage/39486118396135
```

No hay un ancho móvil independiente configurado. Por eso, el código debería mantener el ancho de escritorio y permitir que el logo se reduzca en pantallas pequeñas si fuera necesario.

Ejemplo:

```css
.site-header__logo {
  display: block;
  width: 140px;
  height: auto;
}

@media screen and (max-width: 749px) {
  .site-header__logo {
    width: 120px;
  }
}
```

El valor móvil de `120px` es una recomendación inicial, no un valor confirmado.

### Colores confirmados

El encabezado utiliza `scheme-3`:

```css
--header-background: #FFFFFF;
--header-text: #421314;
--header-button-background: #8E605D;
--header-button-text: #421314;
--header-secondary-button-text: #FFFFFF;
--header-shadow: #B7A2A6;
```

En términos visuales:

- Fondo blanco.
- Texto y navegación en borgoña oscuro `#421314`.
- Botones en marrón rosado `#8E605D`.
- Línea separadora activada.

El color exacto y la opacidad de la línea separadora no están expuestos en la configuración.

### Elementos activos

El encabezado tiene activados:

```text
Selector de país/región: sí
Selector de idioma: sí
Avatar de cliente: sí
```

El menú es:

```text
main-menu
```

No se obtuvo el contenido interno del menú, por lo que para migrarlo debes conservar desde Shopify:

- Los nombres de los enlaces.
- El orden.
- Las URL.
- Los submenús.
- Los elementos anidados.

No se confirmaron configuraciones independientes para el icono de búsqueda, carrito, contador del carrito o iconos personalizados. Esos elementos pueden formar parte de la estructura fija del tema.

### Espaciado confirmado

```text
Margen inferior: 0
Padding superior: 0
Padding inferior: 0
```

Valores generales del tema:

```text
Ancho máximo: 1200px
Separación horizontal de grid: 28px
Separación vertical de grid: 28px
Separación entre secciones: 0
```

El encabezado no tiene altura fija. Su altura depende del logo, los iconos, el menú y el CSS interno del tema.

### Estructura recomendada

```html
<header class="site-header">
  <div class="site-header__inner">
    <button class="site-header__menu-button" aria-label="Abrir menú">
      <!-- icono del menú -->
    </button>

    <nav class="site-header__nav">
      <!-- main-menu -->
    </nav>

    <a class="site-header__logo" href="/">
      <!-- imagen del logo -->
    </a>

    <div class="site-header__actions">
      <!-- país, idioma, cuenta, búsqueda, carrito -->
    </div>
  </div>
</header>
```

Sin embargo, la posición exacta del menú y de los iconos debe medirse visualmente, porque la configuración solo confirma que el logo está centrado arriba en escritorio, no todos los offsets internos.

---

## 3. Section 1 de GemPages

Esta es la parte que sí conviene reemplazar por una sección nativa de Shopify. Actualmente está implementada con GemPages como la primera sección de la página de inicio.

```text
Nombre: Section 1
Tipo: gp-section
Estado: activa
Ubicación: primera sección de contenido de la página de inicio
Precarga: activada
```

La sección contiene texto, un CTA, un SVG y un video de fondo para tablet.

### Contenido exacto

#### Texto principal

```html
Menos caída, más densidad
```

Color:

```css
color: #7F1201;
background: transparent;
```

No tiene una familia tipográfica definida directamente en el HTML.

#### Texto secundario

```html
Nunca fue tu culpa, usaste el sistema y producto equivocados
```

Color:

```css
color: #000000;
background: transparent;
```

El HTML original contiene un espacio inicial `&nbsp;`. En la migración recomiendo eliminarlo y controlar el espaciado mediante CSS, porque un espacio invisible dentro del contenido puede generar una sangría irregular:

```html
<p class="hero__description">
  Nunca fue tu culpa, usaste el sistema y producto equivocados
</p>
```

#### Botón

Texto:

```text
Recuperar mi cabello
```

Color del texto:

```css
color: #FBF0EF;
```

No se pudo confirmar:

- El enlace de destino.
- El color de fondo.
- El borde.
- El radio.
- El padding.
- El estado hover.
- El tamaño.
- La sombra.

Esos valores deben recuperarse desde el editor de GemPages o desde el CSS renderizado antes de reemplazar el botón.

#### Texto adicional

```html
Recomendado por dermatólogos
```

Color:

```css
color: #000000;
background: transparent;
```

#### SVG

Se utiliza el mismo archivo para escritorio y móvil:

```text
https://cdn.shopify.com/s/files/1/0767/8292/8103/files/hampoos_29.svg?v=1785644419
```

En la migración debes incluirlo con `width`, `height` y `alt` para evitar saltos visuales mientras carga:

```html
<img
  src="https://cdn.shopify.com/s/files/1/0767/8292/8103/files/hampoos_29.svg?v=1785644419"
  alt="Producto para el cuidado del cabello"
  width="600"
  height="600"
  loading="eager"
>
```

Los valores `600 × 600` son solo de ejemplo. Debes reemplazarlos por las dimensiones reales del SVG.

#### Video

Para tablet está configurado este video externo:

```text
https://media.w3.org/2010/05/sintel/trailer.mp4
```

Antes de publicarlo, verifica que sea intencional. El archivo se llama `Sintel trailer`, por lo que parece un recurso de demostración o prueba, no necesariamente el video definitivo de la marca.

La estructura recomendada sería:

```html
<section class="hero">
  <div class="hero__media">
    <video
      autoplay
      muted
      loop
      playsinline
      preload="metadata"
      aria-hidden="true">
      <source
        src="https://media.w3.org/2010/05/sintel/trailer.mp4"
        type="video/mp4">
    </video>
  </div>

  <div class="hero__content">
    <p class="hero__eyebrow">Recomendado por dermatólogos</p>
    <h1>Menos caída, más densidad</h1>
    <p class="hero__description">
      Nunca fue tu culpa, usaste el sistema y producto equivocados
    </p>
    <a class="hero__button" href="/collections/all">
      Recuperar mi cabello
    </a>
  </div>

  <div class="hero__product">
    <img src="..." alt="Producto para el cuidado del cabello">
  </div>
</section>
```

La URL `/collections/all` es únicamente un ejemplo. El destino real del botón no fue identificado y no conviene inventarlo.

### Colores de Section 1

Usa estas variables locales:

```css
:root {
  --hero-heading: #7F1201;
  --hero-body: #000000;
  --hero-button-text: #FBF0EF;
  --hero-transparent: transparent;
}
```

El color `#7F1201` coincide con el color de botón del `scheme-1` del tema, pero Section 1 no está configurada oficialmente con `scheme-1`. Por tanto, debes tratar estos colores como valores propios de GemPages.

### Tipografía de Section 1

Aquí hay que ser cuidadosos. El HTML de GemPages no define la fuente. El tema global utiliza:

```css
font-family: "IBM Plex Sans", sans-serif;
font-weight: 400;
```

Pero GemPages podría aplicar otra fuente mediante clases o CSS propio. La migración debería fijar la fuente explícitamente para que el diseño no cambie:

```css
.hero {
  font-family: "IBM Plex Sans", sans-serif;
}

.hero h1,
.hero p,
.hero a {
  font-family: inherit;
}
```

No se confirmaron los tamaños, pesos, interlineados ni espaciados de los textos de GemPages. Por eso, para una copia exacta hay que medir la página actual o revisar el CSS generado por GemPages.

---

## Sistema de colores completo

Puedes centralizar los colores así:

```css
:root {
  /* Barra de anuncios */
  --color-announcement-bg: #421314;
  --color-announcement-text: #FFFFFF;

  /* Encabezado */
  --color-header-bg: #FFFFFF;
  --color-header-text: #421314;
  --color-header-button: #8E605D;
  --color-header-shadow: #B7A2A6;

  /* Section 1 */
  --color-hero-heading: #7F1201;
  --color-hero-body: #000000;
  --color-hero-button-text: #FBF0EF;

  /* Tipografía */
  --font-heading: "IBM Plex Sans", sans-serif;
  --font-body: "IBM Plex Sans", sans-serif;
}
```

También están configurados globalmente:

```text
Radio de botones: 40px
Borde de botones: 1px
Opacidad del borde: 100%
Ancho máximo de contenido: 1200px
Grid horizontal: 28px
Grid vertical: 28px
```

El radio de `40px` y el borde de `1px` aplican con seguridad a los botones nativos del tema, pero no se puede garantizar que el botón de GemPages los utilice.

## Orden recomendado de implementación

1. Mantener la barra de anuncios como componente nativo o convertirla en una sección Liquid muy pequeña.
2. Mantener el encabezado nativo de Shopify, porque no pertenece a GemPages y ya está optimizado dentro del tema.
3. Crear una sección Liquid propia para reemplazar únicamente Section 1.
4. Eliminar la sección de GemPages después de comprobar que el reemplazo visual funciona.
5. Confirmar que no queden scripts, snippets o app embeds de GemPages cargándose en la página.
6. Servir el SVG y las imágenes desde Shopify CDN.
7. Usar `loading="eager"` solo para el contenido visible al cargar y `loading="lazy"` para imágenes que estén más abajo.
8. Añadir dimensiones reales a las imágenes para evitar cambios de layout.
9. Mantener un solo `h1` en la sección principal.
10. Verificar la versión móvil, tablet y escritorio por separado.

Shopify señala que las aplicaciones pueden dejar código en el tema incluso después de desinstalarse, así que quitar visualmente GemPages no siempre elimina automáticamente todos sus recursos. Conviene revisar también los snippets, app embeds y scripts asociados antes de medir el rendimiento.

Lo que todavía no se puede confirmar únicamente desde la configuración de Shopify es el tamaño exacto de los textos, la posición de cada elemento, el padding interno, los breakpoints, el estilo hover del botón, el enlace del CTA y las reglas de reproducción del video. Esos datos están dentro de GemPages o de su CSS generado.

