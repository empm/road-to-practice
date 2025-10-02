> **Objetivo:** dejar de abusar de div y montar un head limpio con las metas que realmente importan.


## **🧭 Mapa mental rápido**

- **Landmarks:** header, nav, main, aside, footer
    
- **Agrupar contenido:** section (bloque temático con título), article (unidad independiente)
    
- **Medios y detalles:** figure + figcaption, time, address
    
- **Formularios:** form, label, fieldset + legend
    
- **Tablas:** table, caption, thead, tbody, tfoot, th

---

## **✅ Decálogo práctico (cuándo usar qué)**

- **header**: cabecera de la página **o** de una sección/artículo (puede haber varios).
    
- **nav**: grupo de enlaces de navegación (principal, secundaria, breadcrumbs…).
    
- **main**: el **único** contenido principal de la página (una vez por documento).
    
- **section**: bloque temático con **encabezado propio** (h2–h6). Si no tiene título, igual es un div.
    
- article: pieza **independiente** y reutilizable (post, tarjeta de noticia, ficha que “se entiende **sola**”).
    
- **aside**: contenido **tangencial** (sidebar, módulos relacionados, anuncios).
    
- **footer**: pie de página **o** pie de una sección/artículo (también puede haber varios).
    
- **figure** + **figcaption**: medios con pie de foto/nota.
    
- **address**: información de **contacto** del autor/organización del artículo o de la página.
  

> **Regla de oro:** div es el “comodín” cuando **ningún** semántico aplica. Si una parte tiene sentido por sí misma → article. Si es un grupo temático con título → section. Si es navegación → nav. Etc.

---

## **🧱 Esqueleto recomendado (layout semántico)**

```html
<!doctype html>
<html lang="es-ES">
<head>
  <meta charset="utf-8">
  <title>Mi sitio</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <!-- SEO básico -->
  <meta name="description" content="Resumen claro de la página (≈150–160 caracteres).">
  <link rel="canonical" href="https://ejemplo.com/pagina">

  <!-- Color de la UI del navegador -->
  <meta name="theme-color" content="#0b1020" media="(prefers-color-scheme: dark)">
  <meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">

  <!-- Robots -->
  <meta name="robots" content="index,follow">

  <!-- Social (Open Graph + Twitter) -->
  <meta property="og:title" content="Título social">
  <meta property="og:description" content="Descripción social">
  <meta property="og:image" content="https://ejemplo.com/og.jpg">
  <meta property="og:url" content="https://ejemplo.com/pagina">
  <meta property="og:locale" content="es_ES">
  <meta name="twitter:card" content="summary_large_image">
</head>
<body>
  <header>
    <h1>Mi sitio</h1>
    <nav aria-label="Principal">
      <a href="/">Inicio</a>
      <a href="/blog">Blog</a>
      <a href="/contacto">Contacto</a>
    </nav>
  </header>

  <main id="contenido">
    <article>
      <header>
        <h2>Título del artículo</h2>
        <p><time datetime="2025-10-02">2 oct 2025</time></p>
      </header>

      <section aria-labelledby="sec-intro">
        <h3 id="sec-intro">Introducción</h3>
        <p>…</p>
      </section>

      <figure>
        <img src="/img/diagrama.png" alt="Diagrama del flujo X">
        <figcaption>Diagrama general del proceso.</figcaption>
      </figure>

      <footer>
        <address>Autor: Equipo X — hola@ejemplo.com</address>
      </footer>
    </article>

    <aside aria-label="Relacionado">
      <ul>
        <li><a href="/post/relacionado-1">Relacionado 1</a></li>
      </ul>
    </aside>
  </main>

  <footer>
    <small>© 2025 Mi sitio</small>
  </footer>
</body>
</html>
```

---

## **🏷️ Metas esenciales (qué hacen y cómo usarlas)**
### **1)** **meta charset**

> Define la codificación del documento (tildes, emojis, etc.).

```html
<meta charset="utf-8">
```

### **2)** **meta name="viewport"**

> Indica ancho lógico y zoom inicial para responsive real.

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
<!-- iOS con notch -->
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
```

### **3)** **meta name="description"**

> Resumen que a menudo usan buscadores como snippet.

```html
<meta name="description" content="Descripción clara (150–160 caracteres).">
```

### **4)** **meta name="robots"**

> Control de indexado y seguimiento de enlaces.

```html
<meta name="robots" content="index,follow">
<!-- Variantes: noindex, nofollow, noarchive, max-image-preview:large, etc. -->
```

### **5)** **meta name="theme-color"**

> Color de la barra del navegador (móvil); puedes ofrecer valores por tema.

```html
<meta name="theme-color" content="#0b1020" media="(prefers-color-scheme: dark)">
<meta name="theme-color" content="#ffffff" media="(prefers-color-scheme: light)">
```

### **6) Metas sociales (Open Graph / Twitter)**  

> No son name (en OG) sino property, pero controlan la vista al compartir.

```html
<meta property="og:title" content="Título social">
<meta property="og:description" content="Descripción social">
<meta property="og:image" content="https://ejemplo.com/og.jpg">
<meta property="og:url" content="https://ejemplo.com/pagina">
<meta property="og:locale" content="es_ES">
<meta name="twitter:card" content="summary_large_image">
```

**Bonus útiles:**

```html
<!-- evita duplicados -->
<link rel="canonical" href="https://ejemplo.com/pagina">         
<!-- PWA -->
<link rel="manifest" href="/site.webmanifest">           
<!-- control del referrer -->        
<meta name="referrer" content="no-referrer-when-downgrade">      
```

---

## **🌐 “Locale” e idioma**

>  Idioma principal del documento: **lang** en html (no una meta).

```html
<html lang="es-ES">
```

- Dirección del texto (si aplica):

```html
<html dir="ltr"> <!-- o rtl -->
```

- Para social/OG:

```html
<meta property="og:locale" content="es_ES">
```

> `meta http-equiv="content-language"` está obsoleto: usa `lang` en html.

---

## **🔎 Mini-cheatlist de a11y/SEO + semántica**

- Usa **landmarks**: header, nav, main, aside, footer.
- Cada section/article con encabezado propio (h2/h3…).
- Evita div si existe un semántico equivalente.
- img siempre con alt descriptivo.
- Enlaces con texto claro (evita “clic aquí”).
- En head: meta charset, meta viewport, meta description, meta robots, meta theme-color, OG/Twitter y link rel="canonical" si aplica.

---

## **🔁 Refactor “div → semántico”**

**Antes**

```html
<div class="header">
  <div class="menu">…</div>
</div>
<div class="content">
  <div class="article">
    <h2>Post</h2>
    <p>…</p>
  </div>
  <div class="sidebar">…</div>
</div>
<div class="footer">©</div>
```

**Después**

```html
<header>
  <nav aria-label="Principal">…</nav>
</header>
<main>
  <article>
    <h2>Post</h2>
    <p>…</p>
  </article>
  <aside aria-label="Relacionado">…</aside>
</main>
<footer>©</footer>
```

---

## **📝 Micro-prompts para autoevaluarte**

- “¿Esto se entiende por sí solo fuera del contexto de la página?” → article.
- “¿Es un bloque temático con **título**?” → section.
- “¿Es navegación?” → nav.
- “¿Es decorativo/estructura sin semántica?” → div.
  

---
## Enlaces útiles

- MDN HTML Elements: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements
