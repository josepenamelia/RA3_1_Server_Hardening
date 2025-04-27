# CSP Bypass en DVWA

---

## Contexto - Security Low

En este ejercicio se analiza la vulnerabilidad relacionada con **Content Security Policy (CSP)** en DVWA configurado en nivel de seguridad **Low**.

El reto consiste en encontrar una forma de ejecutar código JavaScript a pesar de la política CSP configurada en la página.

---

## Observación inicial

- Existe una política CSP definida que **limita la carga de scripts** solo a dominios específicos.
- No se permite la ejecución de código `inline` (por ejemplo, etiquetas `<script>` escritas directamente en la página).

---

## Estrategia de explotación

Para sortear la restricción de CSP:

- No podemos usar `<script>alert('XSS')</script>` porque sería bloqueado.
- Sin embargo, podemos utilizar otros elementos HTML que permitan la ejecución de código indirectamente.

Una opción válida es emplear la etiqueta `<svg>`, aprovechando eventos que sí ejecuten JavaScript.

---

## Payload utilizado

```
<svg/onload=alert('CSP Bypassed')>
```

El atributo onload del elemento <svg> es ejecutado al renderizar la imagen.

Como los eventos en etiquetas HTML no son bloqueados en esta configuración CSP débil, el código se ejecuta.

### Procedimiento

Acceder a la sección vulnerable de DVWA CSP Bypass.

Insertar el siguiente payload en el campo de entrada:

```
<svg/onload=alert('CSP Bypassed')>
```

Enviar el formulario.

Observar que se dispara una ventana emergente (alert) mostrando que el código JavaScript se ejecutó exitosamente.

---

# Contexto - Medium

En este ejercicio se analiza la vulnerabilidad relacionada con Content Security Policy (CSP) en DVWA configurado en nivel de seguridad Medium.

El objetivo es encontrar una forma de ejecutar código JavaScript en la página a pesar de las restricciones CSP configuradas.

---

## Observación inicial

Al capturar los encabezados de respuesta HTTP, se observa que la política CSP ha cambiado respecto al nivel "Low".

Ahora se incluye:

script-src 'self' 'unsafe-inline' 'nonce-TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA='

La presencia del nonce sugiere que los scripts deben incluir este valor para ser considerados seguros y ser ejecutados.

---

## Análisis del Nonce

El valor TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA= parece ser un string codificado en Base64.

Al decodificarlo en una herramienta online, obtenemos:

Never going to give you up

Dado que el nonce no cambia entre peticiones, podemos reutilizarlo en nuestros scripts para conseguir la ejecución.

---

## Payload utilizado
```
<script nonce="TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA=">console.log("CSP Bypass");</script>
```
Insertamos el script en la página incluyendo el atributo nonce requerido.

El navegador valida el nonce y ejecuta el código.

Variante para prueba de alertas:
```
<script nonce="TmV2ZXIgZ29pbmcgdG8gZ2l2ZSB5b3UgdXA=">alert("CSP Bypass Success");</script>
```
---

## Procedimiento

Acceder a la sección vulnerable de DVWA CSP Bypass.

Insertar el script anterior en el campo de entrada o mediante manipulación de peticiones.

Enviar la solicitud.

Observar que se ejecuta el código JavaScript permitido por el nonce configurado.