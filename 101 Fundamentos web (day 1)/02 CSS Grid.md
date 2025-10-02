> Piensa en **Grid** como una **tabla flexible de filas y columnas** que tú diseñas: dices cuántas columnas hay, qué tamaño tienen y cómo se colocan los bloques.

---

## **1) Lo básico (contenedor y celdas)**

```html
<div class="grid">
  <div class="item">A</div>
  <div class="item">B</div>
  <div class="item">C</div>
  <div class="item">D</div>
</div>
```

```css
.grid{
  display: grid;                       /* Activa grid */
  grid-template-columns: 1fr 1fr 1fr;  /* 3 columnas iguales */
  gap: 1rem;                           /* hueco entre celdas (filas/columnas) */
}
.item{  /* Aspecto celda */
    background: var(--card); 
    padding:1rem; 
    border-radius:8px; 
    width: 3.5rem;                    /* Indicamos nosotros el ancho */
    text-align: center;
} 
```

- **fr**: “fracción disponible”.
- **gap**: sustituye a `grid-row-gap`/ `grid-column-gap`.

![[Pasted image 20251002104048.png]]

---

## **2) Columnas con tamaños mezclados**

```css
.grid{
  display: grid;
  grid-template-columns: 200px 1fr 2fr; /* fija + flexible */
  gap: 1rem;
}
```

- 1ª col: 200px fijos
- 2ª col: 1 parte del espacio restante
- 3ª col: 2 partes del espacio restante

![[Pasted image 20251002104339.png]]

---

## **3) Hacer que un ítem ocupe varias columnas/filas (span)**

```html
<div class="grid">
	<div class="item">A</div>
	<div class="item">B</div>
	<div class="item">C</div>
	<div class="item--destacado">Destacado</div>
</div>
```

```css
 .grid{
  display: grid;
  grid-template-columns: repeat(4, 1fr); /* 4 columnas iguales */
  gap: .75rem;
}
.item, .item--destacado{  /* Aspecto celda */
    background: var(--card); 
    padding:1rem; 
    border-radius:8px;
    text-align: center;
    display: flex;
    align-items: center;
} 
.item--destacado{
  grid-column: 1 / span 2;  /* ocupa 2 columnas desde la columna 1 */
  grid-row: 1 / span 2;     /* ocupa 2 filas */
}
```

La celda `D`, ocupa el tamaño de dos columnas (`grid-column: span2`), empezando desde la primera (`grid-column: 1`) > `grid-column: 1 / span 2;`
![[Pasted image 20251002105200.png]]

---

## ⚡️ **4) Colocación automática e “inteligente”**

```css
.grid{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 4rem;     /* alto por defecto de filas implícitas */
  grid-auto-flow: row dense; /* rellena huecos si puede (backfilling) */
  /* Si se quiere espacio entre ellas ~ gap: 1rem; */
}
```

- **grid-auto-flow**: dense intenta **aprovechar huecos**, útil en rejillas tipo “masonry light” (muro de ladrillo).

### grid-auto-flow

#### row dense
![[Pasted image 20251002110430.png]]

#### column dense

En este caso, se desactiva `grid-template-columns: repeat(3, 1fr);`.

![[Pasted image 20251002110504.png]]


---

## ✅ **5)** **Adaptativo sin media queries (auto-fit + minmax)**

Conforme se hace pequeño el contenedor, **disminuyen las columnas** automáticamente:

```css
.container {          /* Padre */
    width: 70%;       /* Ancho pantalla */
    margin: 0 auto;   /* Centrado */
}

.grid{
  display: grid;
  /* Se colocan tantas columnas como en 70% quepan con tamaño de 4rem */
  grid-template-columns: repeat(auto-fit, minmax(4rem, 1fr));
  gap: 1rem;           /* Espacios */
}

.item{  
    background: var(--card); 
    padding:1rem; 
    border-radius:8px;
    text-align: center;
} 
```

- **auto-fit**: **colapsa** columnas vacías y deja que las existentes se estiren.
- **minmax(240px o 4rem, 1fr)**: cada tarjeta mide **al menos** 240px; si sobra espacio, crece.

> ¿Diferencia rápida?
- > auto-fit: “que las columnas vacías desaparezcan y las llenas ocupen todo”.
- > auto-fill: “mantén los tracks ‘fantasma’ (a veces verás huecos)”.

### 755px ancho página - 120px ancho celda
![[Pasted image 20251002130548.png]]

Si acortamos un pixel de ancho a la página (754px), la celda valdría menos de 120px y, como es el ancho mínimo (minmax), se quita una columna y se añade una fila.

![[Pasted image 20251002130653.png]]



---

## **6)** **Adaptativo con media queries (control explícito)**

```css
.gallery{
  display: grid;
  gap: 1rem;
  grid-template-columns: repeat(4, 1fr);   /* ≥1200px → 4 col */
}

@media (max-width: 1200px){
  .gallery{ grid-template-columns: repeat(3, 1fr); }  /* 3 col */
}
@media (max-width: 900px){
  .gallery{ grid-template-columns: repeat(2, 1fr); }  /* 2 col */
}
@media (max-width: 600px){
  .gallery{ grid-template-columns: 1fr; }             /* 1 col */
}
```

- Útil si quieres **momentos exactos** de cambio (breakpoints).

---

## **7) Layout de página con áreas (muy legible)** {borrador}

```html
<div class="page">
  <header class="hdr">Header</header>
  <aside class="sdb">Sidebar</aside>
  <main  class="cnt">Contenido</main>
  <footer class="ftr">Footer</footer>
</div>
```

```css
.page{
  display: grid;
  grid-template-columns: 220px 1fr;
  grid-template-rows: auto 1fr auto;
  grid-template-areas:
    "hdr hdr"
    "sdb cnt"
    "ftr ftr";
  min-height: 100dvh;
  gap: 1rem;
}
.hdr{ grid-area: hdr; }
.sdb{ grid-area: sdb; }
.cnt{ grid-area: cnt; }
.ftr{ grid-area: ftr; }

/* En móvil, sidebar abajo y una sola columna */
@media (max-width: 700px){
  .page{
    grid-template-columns: 1fr;
    grid-template-areas:
      "hdr"
      "cnt"
      "sdb"
      "ftr";
  }
}
```

---

## **8) Alineación (rápido)** {borrador}

- **Dentro de cada celda**:
    `justify-items` (horizontal) y `align-items` (vertical).


```css
.grid{ justify-items: center; align-items: start; }
```

- **Conjunto de la rejilla** (sobrante del contenedor):
    `justify-content` y `align-content`.
---

## **9) “Subgrid” (cuando el título y el cuerpo deben alinear columnas) ** {borrador}

_(Opcional, moderno y ya soportado en navegadores actuales)_

```css
.card-list{
  display: grid;
  grid-template-columns: 2fr 1fr;
  gap: 1rem;
}
.card{
  display: grid;
  grid-template-columns: subgrid; /* hereda columnas del padre */
  grid-column: 1 / -1;            /* la tarjeta ocupa ambas columnas */
}
```

- Útil para alinear internamente elementos con la misma malla que el contenedor.
---

## **10) “Plantillas” que vas a usar todo el tiempo** {borrador}

**Tarjetas adaptativas**

```css
.grid-cards{
  display:grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 1rem;
}
```

**Cuadrícula simple 12 columnas**

```css
.grid-12{
  display:grid;
  grid-template-columns: repeat(12, 1fr);
  gap: .75rem;
}
/* Un bloque que ocupa 4 columnas desde la 3 */
.bloque{ grid-column: 3 / span 4; }
```

**Hero + 3 columnas abajo, colapsando a 1 en móvil**

```css
.landing{
  display:grid;
  grid-template-rows: auto auto;
  gap: 1.25rem;
}
.cards{ display:grid; grid-template-columns: repeat(3, 1fr); gap:1rem; }
@media (max-width: 800px){
  .cards{ grid-template-columns: 1fr; }
}
```

---

## **Truco de depuración**

```css
* { outline: 1px solid rgba(255,255,255,.05); } /* quitar luego */
```

Te ayuda a “ver” la rejilla mientras maquillas.

---

### **Cuándo usar qué**

- **Listado de tarjetas responsive** → repeat(auto-fit, minmax(...)).
    
- **Layouts de página claros** → grid-template-areas.
    
- **Colocaciones especiales** (destacar una card) → grid-column / grid-row con span.
    
- **Control fino por diseño** → media queries.

---
## Enlaces útiles

- MDN CSS Grid: https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout/Grids
- Grid auto flow: https://developer.mozilla.org/en-US/docs/Web/CSS/grid-auto-flow
