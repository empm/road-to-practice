> **Fetch API** proporciona una interfaz para recuperar recursos (incluso a través de la red).
> **Fetch** es como un “**cartero universal**” de la web.

- **Request**: el **paquete que envías**: a qué dirección (URL) va, qué tipo de envío es (GET, POST…), qué etiquetas lleva (headers) y qué metes dentro (body).

- **Response**: el **paquete que recibes**: si llegó bien o mal (status 200, 404…), qué etiquetas trae (headers) y qué trae dentro (texto, JSON, imagen…).

### **¿Qué significa “definición genérica de objeto”?**

Como si todos los paquetes del mundo siguieran **la misma plantilla**.

Una **definición genérica** es un **plano estándar** (una “forma común”) que dice: “todo Request tiene estas partes y funciones… y todo Response también”. Da igual si envías una carta, un dibujo o un juguete: **la forma de abrir/cerrar, mirar la etiqueta y sacar lo de dentro es siempre igual**.

### **Concepto y uso**

> El Fetch API define **interfaces estándar** para Request y Response (y cosas relacionadas como Headers, FormData, AbortController), de modo que **todas las operaciones de red se manejan con la misma API**, sin importar el recurso o el método HTTP.

### **Mini-ejemplos**

```js
// 1) Hacer una petición (Request “genérico”)
const req = new Request('https://api.ejemplo.com/usuarios', {
  method: 'GET',
  headers: { 'Accept': 'application/json' }
});

// 2) Usar fetch con ese Request y obtener un Response “genérico”
const res = await fetch(req);

// Estas operaciones son SIEMPRE iguales, sea lo que sea el contenido:
console.log(res.ok, res.status);  // ¿fue bien? ¿código?
const data = await res.json();    // si era JSON
// o:
// const texto = await res.text(); // si era texto
// const imagen = await res.blob(); // si era imagen/binario
```

Fíjate: **las mismas funciones** (.ok, .status, .json(), .text(), .blob()) te valen para cualquier respuesta, porque **Response** tiene una **forma genérica**. 
Y lo mismo pasa con **Request**: siempre puedes mirar url, method, headers, body, etc.


---
## Enlaces útiles
- MDN Fetch API: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API