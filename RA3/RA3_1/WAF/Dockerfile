# Usa la imagen del contenedor anterior (con CSP)
FROM pps/apache-csp

# Instalar ModSecurity
RUN apt update && apt install -y libapache2-mod-security2

# Activar ModSecurity
RUN a2enmod security2

# Configurar ModSecurity para bloquear ataques
COPY modsecurity.conf /etc/modsecurity/modsecurity.conf
RUN sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/modsecurity/modsecurity.conf

# Descargar e instalar OWASP Core Rule Set
RUN git clone https://github.com/coreruleset/coreruleset.git /usr/share/modsecurity/owasp-crs \
    && cp /usr/share/modsecurity/owasp-crs/crs-setup.conf.example /etc/modsecurity/crs-setup.conf

# Configurar Apache para usar las reglas de OWASP
RUN echo "IncludeOptional /etc/modsecurity/crs-setup.conf" >> /etc/apache2/mods-available/security2.conf \
    && echo "IncludeOptional /usr/share/modsecurity/owasp-crs/rules/*.conf" >> /etc/apache2/mods-available/security2.conf

# Reiniciar Apache
CMD ["apachectl", "-D", "FOREGROUND"]
