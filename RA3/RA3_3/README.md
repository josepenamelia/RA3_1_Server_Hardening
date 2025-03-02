# Hardening de Apache con Docker

## **  Descripción**
Este proyecto implementa medidas de seguridad avanzadas en **Apache** utilizando **Docker**, incluyendo:

 Ocultación de información del servidor.  
 Restricción de listado de directorios.  
 Implementación de cabeceras de seguridad.  
 Configuración de límites para mitigar ataques DoS.  
 Habilitación de módulos de seguridad (`mod_security`, `mod_evasive`).  

Este Dockerfile construye una imagen con **todas estas configuraciones aplicadas automáticamente**.

---

## **  Pasos para Construcción y Ejecución**

### ** 1 Construcción de la Imagen Docker**
Ejecuta el siguiente comando para construir la imagen:
```sh
docker build -t pps/apache-hardened -f Dockerfile .
```
✅ **Esto generará una imagen Docker con todas las configuraciones de seguridad activas.**

### ** 2 Ejecución del Contenedor**
Inicia el contenedor con el puerto HTTPS correcto:
```sh
docker run -d -p 8443:443 pps/apache-hardened
```
 **Ahora Apache estará ejecutándose con todas las restricciones activas.**

---

## ** 1 Configuraciones Aplicadas en Apache**

### ** 1 Ocultación de Información del Servidor**
Se ha deshabilitado la exposición de detalles de Apache y del sistema operativo:
```apache
ServerTokens Prod
ServerSignature Off
```

### ** 2 Restricción de Directorios (Deshabilitar `Indexes`)**
El listado de directorios está bloqueado con:
```apache
<Directory /var/www/html>
    Options -Indexes
</Directory>
```
 **Esto impide que los visitantes vean el contenido de los directorios sin un archivo `index.html`.**

### ** 3 Cabeceras de Seguridad Implementadas**
Se han añadido cabeceras HTTP para mejorar la seguridad:
```apache
Header always append X-Frame-Options SAMEORIGIN
Header set X-XSS-Protection "1; mode=block"
Header set X-Content-Type-Options nosniff
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
Header always edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure
```
 **Estas cabeceras protegen contra ataques de Clickjacking, XSS y manipulación de cookies.**

### ** 4 Limitación del Tamaño de Peticiones para Mitigar Ataques DoS**
Se ha configurado un límite en el tamaño de solicitudes para evitar ataques de denegación de servicio:
```apache
LimitRequestBody 102400
```
 **Esto restringe el tamaño de archivos subidos a 100 KB.**

### ** 5 Instalación y Configuración de `mod_evasive` y `mod_security`**
Se han habilitado módulos avanzados de seguridad:
```sh
a2enmod headers ssl security2 evasive rewrite
```
 **Estos módulos bloquean IPs maliciosas y filtran tráfico sospechoso.**

---

## ** Pruebas de Seguridad**

### ** 1 Verificar que Apache Está Corriendo**
Ejecuta:
```sh
docker ps -a
```
 **Debe mostrar el contenedor `pps/apache-hardened` ejecutándose en el puerto `8443`.**

### ** 2 Prueba de Restricción de Directorios**
Ejecuta:
```sh
curl -k https://localhost:8443/somefolder/
```
 **Debe devolver `403 Forbidden`, confirmando que `Options -Indexes` está activo.**

### ** 3 Verificar Cabeceras de Seguridad**
Ejecuta:
```sh
curl -I -k https://localhost:8443
```
 **Debe mostrar cabeceras como:**
```
X-Frame-Options: SAMEORIGIN
X-XSS-Protection: 1; mode=block
Strict-Transport-Security: max-age=31536000; includeSubDomains
X-Content-Type-Options: nosniff
```

### ** 4 Simulación de Ataques**
Prueba si el servidor bloquea intentos de ataque con **SQL Injection**:
```sh
curl -k "https://localhost:8443/index.html?id=1' OR '1'='1"
```
 **Debe devolver `403 Forbidden`.**

Prueba de **Command Injection**:
```sh
curl -k "https://localhost:8443/index.html?cmd=ls"
```
 **Debe devolver `403 Forbidden`.**

Prueba de **Denegación de Servicio (DoS) con Apache Bench (`ab`)**:
```sh
ab -n 1000 -c 10 https://localhost:8443/
```
 **`mod_evasive` debe bloquear las peticiones después de alcanzar el umbral de seguridad.**

---
