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