# Usa la imagen con soporte SSL como base
FROM pps/apache-ssl

# Instalar herramientas necesarias
RUN apt update && apt install -y libapache2-mod-security2 libapache2-mod-evasive

# Habilitar módulos de seguridad
RUN a2enmod headers ssl security2 evasive rewrite

# Copiar archivos de configuración desde la carpeta local
COPY security.conf /etc/apache2/conf-enabled/security.conf
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf

# Asegurar permisos correctos para el directorio web
RUN chmod -R 755 /var/www/html && chown -R www-data:www-data /var/www/html

# Reiniciar Apache para aplicar cambios
CMD ["apache2ctl", "-D", "FOREGROUND"]
