# Usa la imagen del contenedor anterior (con CSP)
FROM pps/apache-csp

# Instalar ModSecurity
RUN apt update && apt install -y libapache2-mod-security2

# Activar ModSecurity
RUN a2enmod security2

# Copiar y configurar ModSecurity para bloquear ataques
COPY modsecurity.conf /etc/modsecurity/modsecurity.conf
RUN sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/modsecurity/modsecurity.conf

# Reiniciar Apache
CMD ["apachectl", "-D", "FOREGROUND"]
