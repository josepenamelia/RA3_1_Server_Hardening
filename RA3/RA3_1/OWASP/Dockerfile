# Usa la imagen con ModSecurity como base
FROM pps/apache-modsecurity

# Instalar Git (necesario para descargar OWASP CRS)
RUN apt update && apt install -y git

# Descargar OWASP CRS
RUN git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git /usr/share/modsecurity/owasp-crs \
    && mv /usr/share/modsecurity/owasp-crs/crs-setup.conf.example /etc/modsecurity/crs-setup.conf \
    && cp /usr/share/modsecurity/owasp-crs/rules/* /etc/modsecurity/crs/

# Copiar archivos de configuración de OWASP CRS
COPY security2.conf /etc/apache2/mods-enabled/security2.conf
COPY crs-setup.conf /etc/modsecurity/crs-setup.conf
COPY 000-default.conf /etc/apache2/sites-available/000-default.conf
COPY apache2.conf /etc/apache2/apache2.conf
COPY index.html /var/www/html/index.html

# Asignar permisos adecuados
RUN chown -R www-data:www-data /var/www/html /etc/modsecurity \
    && chmod -R 775 /var/www/html /etc/modsecurity

# Ejecutar Apache en primer plano
CMD ["apache2ctl", "-D", "FOREGROUND"]
