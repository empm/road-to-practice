Piensa en role="status" como un **rincón del aviso en voz baja** de tu web.

- Ahí pones mensajes tipo: “Guardado ✅”, “3 resultados encontrados”, “Listo”.
    
- Los lectores de pantalla **lo leen solos** cuando cambias su texto, **sin interrumpir** lo que el usuario está haciendo (no es una sirena). Eso es porque status es una **live region** “educada” (_polite_). 

## **Qué hace exactamente**

- Define una zona que, al cambiar su contenido, **se anuncia** a tecnologías de asistencia.
    
- Tiene por defecto **aria-live="polite"** (anuncia cuando el usuario esté libre) y **aria-atomic="true"** (se lee **todo** el mensaje, no solo el trocito cambiado). 

## **Cuándo usarlo**

- Para **información de estado** no urgente: “Guardado”, “Cargando terminado”, “2 elementos añadidos al carrito”.
    
- Si el mensaje es **urgente** (error crítico, sesión a punto de expirar), usa role="alert" (interrumpe y se anuncia ya). 

## **Cómo se usa**

HTML (déjalo vacío de inicio):

```html
<p id="status" role="status" aria-live="polite"></p>
```

JS (cambia el texto cuando tengas noticias):

```js
const status = document.getElementById('status');
status.textContent = 'Cambios guardados correctamente.';
```

### Consejos:

- Añade el role="status" **antes** de los cambios y **luego** modifica el contenido (así se dispara el anuncio). 
    
- Por compatibilidad, muchos equipos duplican aria-live="polite" aunque ya sea implícito. 
    
- No muevas el foco a ese elemento: es **informativo**, no interactivo.   

## **“Status” vs “Alert” (en una línea)**

- **status** = “avisador amable” (polite + atomic) → no interrumpe.
    
- **alert** = “sirena” (assertive + atomic) → interrumpe y se anuncia ya. 

---
## Enlaces útiles
- A11y role=status: https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Reference/Roles/status_role
