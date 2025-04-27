# Actividad: Bypass de Insecure CAPTCHA en DVWA (adaptado)

---

## Objetivo
Realizar un ataque para cambiar la contraseña en DVWA en el módulo **Insecure CAPTCHA**, explotando vulnerabilidades de validación, usando **Burp Suite** para capturar y modificar el tráfico.

---

## Procedimiento paso a paso - Seguridad low

### 1. Acceso a la aplicación
- Iniciar sesión en **DVWA** con las credenciales habituales.
- Navegar a la sección:
  ```
  Vulnerabilities > Insecure CAPTCHA
  ```

---

### 2. Intentar cambiar la contraseña
- En la sección de cambio de contraseña, introducir:
  - Nueva contraseña: `jose`
  - Confirmación: `jose`

(No importa el valor del captcha en este momento.)

---

### 3. Capturar la solicitud con Burp Suite

- Asegurar que **Burp Suite** esté activo y configurado como proxy.
- Interceptar la petición al enviar el formulario de cambio de contraseña.
- Una vez capturada, **enviar la solicitud al Repeater** (clic derecho > Send to Repeater).

Captura:

![Cambio de contraseña](./Imagenes/Primera_low.png)

---

### 4. Modificar la solicitud en el Repeater

- Dentro de **Repeater**, en la pestaña **Request**, localizar el fragmento:
  ```
  step=1&password_new=paco&password_conf=paco&Change=Change
  ```
- Modificarlo para que quede así:
  ```
  step=2&password_new=jose&password_conf=jose&Change=Change
  ```

De esta forma estamos forzando la aplicación a pasar directamente al paso final, eludiendo el captcha.

---

### 5. Verificar la respuesta

- En **Repeater**, enviar la solicitud modificada.
- Observar la pestaña **Response** > **Render**.
- Confirmar que aparece un mensaje indicando que la contraseña se ha cambiado exitosamente **sin necesidad de resolver el captcha**.

Captura final:

![captura final](./Imagenes/Resultado_low_captcha.png)

---


