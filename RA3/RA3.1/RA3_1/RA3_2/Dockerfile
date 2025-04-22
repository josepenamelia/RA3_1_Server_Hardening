# Usa la imagen con mod_evasive y OWASP CRS como base
FROM pps/apache-evasive

# Activar el módulo SSL en Apache
RUN a2enmod ssl

# Crear directorio para los certificados SSL
RUN mkdir -p /etc/apache2/ssl

# Generar certificado SSL autofirmado dentro del contenedor
RUN openssl req -x509 -nodes -days 365 \
    -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt \
    -subj "/C=ES/ST=Castellon/L=CastellondelaPlana/O=IESelCaminas/OU=EspecializacionCiberseguridad/CN=www.midominioseguro.com"

COPY hosts /etc/hosts
# Copiar configuración de Apache para SSL
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

#Asignar permisos adecuados
RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html 

# Habilitar el sitio SSL en Apache
RUN a2ensite default-ssl.conf && apachectl -k restart

# Reiniciar Apache
CMD ["apache2ctl", "-D", "FOREGROUND"]
