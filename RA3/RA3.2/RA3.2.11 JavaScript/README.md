# JavaScript en DVWA

## Contexto - Security LOW

URL del Desafío: /dvwa/vulnerabilities/javascript/

Objetivo: Enviar correctamente la frase "success" para superar el nivel.

Herramientas necesarias: Ninguna (solo navegador).

## Análisis Inicial

Nos encontramos con un formulario simple que está pre-rellenado con la palabra "ChangeMe".

Intentamos cambiarlo manualmente por "success" y enviarlo, pero recibimos el error "Invalid token".

Esto indica que existe un campo oculto "token" en el formulario que necesita un valor correcto para validar el envío.


## Revisión del Código Fuente

Al inspeccionar el código de la página:

Vemos un campo oculto llamado token.

Encontramos un script en JavaScript que genera el valor de token.

## Entendiendo el JavaScript

Funciones principales:

rot13()

Esta función toma un texto y aplica el cifrado ROT13, rotando cada letra 13 posiciones en el alfabeto.

Todas las letras del input son transformadas según este algoritmo.

generate_token()

Toma el valor del campo phrase.

Aplica rot13() sobre él.

Luego calcula el hash MD5 del resultado.

Finalmente, asigna ese hash al campo oculto token.

## Procedimiento para Romper el Formulario

Aplicamos ROT13 sobre "success":

Resultado: fhpprff

Calculamos el MD5 de fhpprff:

Resultado: 38581812b435834ebf84ebcc2c6424d6

En la consola del navegador (Ctrl+Shift+K), ejecutamos:

document.getElementById("token").value = "38581812b435834ebf84ebcc2c6424d6";

Luego, enviamos el formulario escribiendo "success" en el campo de entrada.


# Contexto - Medium

URL del Desafío: /dvwa/vulnerabilities/javascript/

Objetivo: Enviar correctamente la palabra "success" para superar el nivel.

Herramientas necesarias: Ninguna.

## Análisis Inicial

En este nivel, el funcionamiento es similar al modo "Low", pero se han aplicado cambios en el código JavaScript.

Al hacer clic en "View Source", observamos que el script relevante está en el archivo:

vulnerabilities/javascript/source/medium.js

El código encontrado es:

function do_something(e){
  for(var t="",n=e.length-1;n>=0;n--)
    t+=e[n];
  return t
}

setTimeout(function(){
  do_elsesomething("XX")
},300);

function do_elsesomething(e){
  document.getElementById("token").value = do_something(e + document.getElementById("phrase").value + "XX");
}

## Entendiendo el Código

do_something(e) invierte el string recibido.

setTimeout espera 300 ms antes de ejecutar do_elsesomething("XX").

do_elsesomething(e) toma el valor del input, le añade "XX" antes y después, y luego invierte toda la cadena.

Este valor invertido es asignado al campo oculto token.

## Procedimiento para Romper el Formulario

Según el código:

Si introducimos "success" en el formulario,

El token esperado debería ser XXsseccusXX.

Solución paso a paso

Abrir la consola del navegador (Ctrl+Shift+K).

Ejecutar el siguiente código:

document.getElementById("token").value = "XXsseccusXX";

Introducir "success" en el campo del formulario.

Pulsar "Submit".
