# Usa la imagen base con ModSecurity y OWASP CRS
FROM pps/apache-owasp

# Instalar mod_evasive
RUN apt update && apt install -y libapache2-mod-evasive

# Copiar configuración de mod_evasive
COPY evasive.conf /etc/apache2/mods-available/evasive.conf

# Habilitar mod_evasive en Apache
RUN a2enmod evasive

# Crear directorio de logs
RUN mkdir -p /var/log/mod_evasive && chown -R www-data:www-data /var/log/mod_evasive

# Reiniciar Apache
CMD ["apache2ctl", "-D", "FOREGROUND"]
