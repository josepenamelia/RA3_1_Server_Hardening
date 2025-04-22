# RA3_1

# Resumen del Proyecto: Seguridad en Apache con Docker

## ** Descripción General**
Este proyecto tiene como objetivo **fortalecer la seguridad de Apache** mediante la implementación de diversas estrategias y módulos en contenedores Docker. Se han abordado múltiples aspectos de la seguridad web, incluyendo:

 **Content Security Policy (CSP)** → Protección contra ataques XSS.  
 **Web Application Firewall (WAF) con ModSecurity** → Detección y bloqueo de amenazas como SQLi y XSS.  
 **OWASP Core Rule Set (CRS)** → Implementación de reglas avanzadas de seguridad.  
 **Protección contra ataques DoS con mod_evasive** → Bloqueo de IPs que exceden el umbral de peticiones.  

Cada una de estas soluciones ha sido desplegada en **contenedores Docker**, asegurando una implementación modular y escalable.

---

## ** Estructura del Proyecto**
```sh
.
├── CSP
│   ├── apache2.conf
│   ├── Dockerfile
│   ├── README.md
│   └── Capturas/
├── WAF
│   ├── modsecurity.conf
│   ├── Dockerfile
│   ├── README.md
│   └── Capturas/
├── OWASP
│   ├── 000-default.conf
│   ├── apache2.conf
│   ├── crs-setup.conf
│   ├── security2.conf
│   ├── Dockerfile
│   ├── index.html
│   ├── README.md
│   └── Capturas/
├── DDOS
│   ├── evasive.conf
│   ├── Dockerfile
│   ├── README.md
│   ├── results.tsv
│   └── Capturas/
├── NGINX
│   ├── README.md
└── README.md
```
Cada carpeta contiene archivos de configuración, `Dockerfile` para la construcción del contenedor y capturas de pantalla con la validación de cada implementación.

---

## **  Implementaciones Realizadas**
### ** 1 [Content Security Policy (CSP)](CSP/README.md)**
 **Objetivo:** Implementar CSP en Apache para mitigar ataques XSS.  
 **Dockerfile:** Se ha configurado Apache para aplicar restricciones en la carga de recursos.  
 **Validación:** Se comprobó que los headers reflejan la política de seguridad configurada.  
 **Capturas:** Se documentó la construcción de la imagen y la validación de los headers.  

### ** 2 [Web Application Firewall (WAF) con ModSecurity](WAF/README.md)**
 **Objetivo:** Proteger Apache con un WAF basado en ModSecurity.  
 **Dockerfile:** Se activó ModSecurity en Apache y se configuró para bloquear amenazas.  
 **Validación:** Se ejecutaron ataques simulados para confirmar que ModSecurity los bloquea.  
 **Capturas:** Se documentó la configuración de `modsecurity.conf` y la creación de la imagen.  

### ** 3 [OWASP Core Rule Set (CRS)](OWASP/README.md)**
 **Objetivo:** Integrar las reglas avanzadas de OWASP CRS en ModSecurity.  
 **Dockerfile:** Se instaló OWASP CRS y se configuró para cargar solo sus reglas.  
 **Validación:** Se realizaron pruebas de concepto contra SQLi, XSS y Command Injection.  
 **Capturas:** Se documentó el proceso de implementación y las pruebas de ataque.  

### ** 4 [Protección contra ataques DoS con mod_evasive](DDOS/README.md)**
 **Objetivo:** Implementar `mod_evasive` para mitigar ataques de denegación de servicio.  
 **Dockerfile:** Se instaló `mod_evasive` y se configuró para bloquear IPs agresivas.  
 **Validación:** Se utilizó Apache Bench (`ab`) para generar tráfico y verificar el bloqueo.  
 **Capturas:** Se documentaron los ataques bloqueados y la prueba de carga con `ab`.  
 **Informe:** Se generó un reporte `results.tsv` con los datos del test de carga.  

---

