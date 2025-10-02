> La idea es: un **input de búsqueda** capta texto del usuario y tú **filtras** un **array de objetos** con JavaScript (en el navegador o en el servidor).

---

## **1) El input de búsqueda (lado cliente)**

HTML mínimo y accesible:

```html
<label for="search">Buscar</label>
<input id="search" type="search" placeholder="Nombre, id, etc." aria-controls="lista" />
<ul id="lista"></ul>
```

JS: escuchar el evento input y leer value:

```js
const $search = document.querySelector('#search');

$search.addEventListener('input', (e) => {
  const q = e.target.value; // el texto que ha escrito el usuario
  // aquí llamas a tu función de filtrar y luego a tu función de pintar
});
```

**Consejo:** normaliza el texto antes de filtrar.

```js
function norm(s){
  return s
    .toLowerCase()
    .trim()
    .normalize('NFD')                // separa acentos
    .replace(/\p{Diacritic}/gu, ''); // quita acentos (á->a)
}
```

---

## **2) Filtrar un array (ejemplos fáciles)**

Imagina estos datos:

```js
const users = [
  { id: 1, name: 'Ana Pérez',   city: 'Valencia' },
  { id: 2, name: 'Juan Gómez',  city: 'Madrid'   },
  { id: 3, name: 'Clemente',    city: 'Sevilla'  }
];
```

### **A) Por nombre (contiene)**

```js
function filterByName(list, query){
  const q = norm(query);
  return list.filter(u => norm(u.name).includes(q));
}

// filterByName(users, 'ana') -> Ana Pérez
// filterByName(users, 'gó')  -> Juan Gómez (quita acentos)
```

### **B) Por id (exacto)**

```js
function filterById(list, idText){
  const id = Number(idText); // convierte a número
  if (Number.isNaN(id)) return []; // si no es número, nada
  return list.filter(u => u.id === id);
}

// filterById(users, '2') -> Juan Gómez
```

### **C) Por prefijo (empieza por)**

```js
function filterStartsWith(list, query){
  const q = norm(query);
  return list.filter(u => norm(u.name).startsWith(q));
}

// filterStartsWith(users, 'cl') -> Clemente
```

### **D) Varios campos (nombre o ciudad)**

```js
function filterNameOrCity(list, query){
  const q = norm(query);
  return list.filter(u => norm(u.name).includes(q) || norm(u.city).includes(q));
}
```

### **E) Varios criterios (id exacto y ciudad)**

```js
function filterByIdAndCity(list, idText, city){
  const id = Number(idText);
  const c  = norm(city);
  return list
    .filter(u => (Number.isNaN(id) ? true : u.id === id)) // si no hay id, no filtra por id
    .filter(u => (c ? norm(u.city).includes(c) : true));  // si no hay ciudad, no filtra por ciudad
}
```

### **F) Pequeño debounce/delay (opcional)**

Evita filtrar en cada pulsación; espera un poquito:

```js
function debounce(fn, ms=200){
  let t; 
  return (...args) => { clearTimeout(t); t = setTimeout(() => fn(...args), ms); };
}
$search.addEventListener('input', debounce(e => {
  // filtras aquí con e.target.value
}, 200));
```

---

## **3) Tu ejemplo de Express (lado servidor)**


Tú tienes esto:

```js
app.get('/datos/:id', (req, res) => {
  const data = JSON.parse(fs.readFileSync(FILE_PATH));
  const resultado = data.find(item => item.id == req.params.id);
  if (resultado) res.json(resultado);
  else res.status(404).send({ error: 'Dato no encontrado' });
});
```

### **¿Es parecido a lo que se hace en el ejercicio?**

- **Sí, en espíritu**: estás **filtrando datos según un criterio** (el :id) y devolviendo el resultado.
    
- **Diferencias clave**:
    - Aquí filtras **en el servidor** con **parámetro de ruta** (/datos/3) y usas find (devuelve **un** elemento).
        
    - En la app del día 1, normalmente filtras **en el cliente** sobre un array ya cargado, y sueles usar filter (devuelve **varios**).    


### **Variante “lista filtrada” con query string**

Si quieres devolver **lista** filtrada (no un único id), lo típico es usar **query params**:

```js
// GET /datos?name=ana&city=vale
app.get('/datos', (req, res) => {
  const data = JSON.parse(fs.readFileSync(FILE_PATH));
  const { name, city, id } = req.query;

  const norm = s => s?.toLowerCase().normalize('NFD').replace(/\p{Diacritic}/gu,'').trim();

  let r = data;

  if (id) {
    const n = Number(id);
    r = Number.isNaN(n) ? [] : r.filter(x => x.id === n);       // exacto por id
  }
  if (name) r = r.filter(x => norm(x.name).includes(norm(name))); // contiene por nombre
  if (city) r = r.filter(x => norm(x.city).includes(norm(city))); // contiene por ciudad

  res.json(r);
});
```

- Cliente pediría /datos?name=ana o /datos?id=2.
- Esto es **equivalente** a lo que haces en el cliente, pero el filtrado ocurre **en el server**.

---

## **4) 💡 Tipos de filtros más usados (los “básicos”)**

1. **Texto contiene** (con includes)
    - Para nombre, email, ciudad, etc.
    - Normaliza: minúsculas, trim y acentos.
    
2. **Exacto por id** (números)
    - Convierte a Number y compara con ===.
    
3. **Prefijo** (startsWith)
    - Suele sentirse “más estricto” que contiene.
    
4. **Varios campos** (OR)
    - Buscar en nombre **o** ciudad **o** username.
    
5. **Rango numérico/fechas**
    - price >= min && price <= max, date >= from && date <= to.
    
6. **Categoría/estado** (select o checkbox)
    - country === 'ES', status === 'active'.
  

> Luego ya vienen extras como **ordenación** (sort), **paginación**, y **búsqueda en servidor** cuando hay muchos datos.

---

## **5) Errores típicos y cómo evitarlos**

- Filtrar con === cadenas/números sin convertir → usa Number(idText).
    
- No hacer trim() → búsquedas fallan por espacios.
    
- Acentos → usa la función norm.
    
- find cuando quieres una lista → usa filter.
    
- En servidor, fs.readFileSync en cada request bloquea el event loop (para práctica vale, pero en real usarías lectura asíncrona o una BD).


# Ejemplo función para buscar

```js
/* Creamos una función para no tener que normalizar con notación punto */
function normalizarTexto(s){
  return s
    .toLowerCase()
    .trim()
    .normalize('NFD')                // separa acentos
    .replace(/\p{Diacritic}/gu, ''); // quita acentos (á->a)
}
/* Función onSearch */
function onSearch(e) {
  // ✅ TODO: filtra por nombre; normaliza con toLowerCase() y trim()
  const q = e.target.value;
  const norm = normalizarTexto(q);
  //console.log(q);

  /* Hay que obligar a que la comparación que la haga en minuscula
    El texto que busquemos lo compara con el dato que haya en user.name (nombre del listado de usuarios) por eso tenemos que ponerlo en minuscula, si no comparamos minuscula con el nombre original que empieza en mayuscula
  */
  const filtered = norm 
  ? users.filter(u => u.name.toLowerCase().includes(norm)) 
  : users;

  // Intento 2 de hacerlo mas facil
  const filt2 = norm
  ? users.filter(u => normalizarTexto(u.name).includes(norm))
  : users;

  render(filt2) // devolvemos los usuarios que cumplan el filtro
}
/* No hay botón para buscar, si no que se busca conforme escribes */
$search.addEventListener('input', onSearch);
```