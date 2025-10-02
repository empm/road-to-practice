Una forma **muy fácil de hablar con Promesas** (tareas que tardan) como si fueran pasos normales:

- **async** delante de una función: la función **siempre devuelve una Promesa**.
	
- **await** delante de una Promesa: la función **se “pausa”** hasta que la Promesa termine (bien o con error).
	- Importante: **solo se pausa esa función**, no el navegador ni tu app completa.


## **Explicación sencilla:**

Imagina que tu programa es una **receta de cocina**:

- **Promesa (Promise)**: “La pizza estará lista cuando suene el temporizador”.
    
- **await**: “Me quedo **dentro de esta cocina** esperando a que suene el temporizador **para seguir con el siguiente paso** (pero la ciudad sigue funcionando: no paro el mundo)”.
    
- **async**: “Esta **receta** necesita que vigilemos la cocina; por eso la marco como especial”.


## **¿Cómo se usa? (ejemplo con** **fetch** **)**

### **Sin** **async/await**  **(con**  **.then** **)**

```js
fetch('/pizza')
  .then(res => res.json())
  .then(data => {
    console.log('Lista la pizza:', data);
  })
  .catch(err => console.error('Se quemó:', err));
```

### **Con** **async/await** **(más lineal y legible)**

```js
async function pedirPizza(){
  try {
    const res = await fetch('/pizza');
    if (!res.ok) throw new Error('Horno roto: ' + res.status);
    const data = await res.json();
    console.log('Lista la pizza:', data);
  } catch (err) {
    console.error('Se quemó:', err);
  }
}
```

## **¿Cuándo conviene usarlo?**

- **Pasos en orden** (uno depende del anterior). Ej: “login → pedir perfil → pintar pantalla”.
    
- **Código más claro** que parezca receta: paso 1, paso 2, paso 3.
    
- **Manejo de errores** sencillo con try/catch.
  

## **¿Cuándo NO usar** **await** **(o usarlo con cuidado)?**

- **Tareas independientes** que pueden ir **en paralelo**. Mejor:

```js
const [pizza, bebida] = await Promise.all([
  fetch('/pizza').then(r=>r.json()),
  fetch('/bebida').then(r=>r.json())
]);
```

- Dentro de Array.forEach (no espera). Usa `for...of` o `Promise.all`:

```js
// Bien (secuencial)
for (const url of urls) {
  const res = await fetch(url);
  // ...
}

// En paralelo
const results = await Promise.all(urls.map(u => fetch(u)));
```

- **Trabajo pesado de CPU** (calcular millones de cosas) no se beneficia; ahí usa Web Workers u optimiza la lógica.

## **Cosas clave para no tropezar**

- Una función async **devuelve una Promesa**. Puedes usar `await pedirPizza()` o `.then(...)`.
    
- Si te **olvidas await**, sigues con un **objeto Promesa** en vez del dato real.
    
- await **solo** se puede usar dentro de async (o **top-level** en módulos ES).
    
- Pon try/catch cuando esperas que algo pueda fallar.
  

## **Mini-chuleta

```js
// Patrón base
async function tarea(){
  try {
    const res = await fetch('/api');
    if(!res.ok) throw new Error(`HTTP ${res.status}`);
    const data = await res.json();
    return data; // <- devuelve Promesa resuelta con data
  } catch (e) {
    // Decide si relanzas o gestionas aquí
    console.error(e);
    throw e; // opcional
  }
}

// Usar la función async
(async () => {
  try {
    const datos = await tarea();
    console.log(datos);
  } catch (e) {
    // manejar error
  }
})();
```