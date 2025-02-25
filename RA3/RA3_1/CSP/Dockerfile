#Imagen de Ubuntu
FROM ubuntu:latest
#Actualizacion del sistema + Instalacion de Apache
RUN apt update && apt upgrade -y && apt install -y apache2
#Carga del modulo de cabeceras
RUN a2enmod headers
#Carga el modulo ssl para el https o 443
RUN a2enmod ssl
#Activar el https
RUN a2ensite default-ssl
#Copiar configuracion 
COPY apache2.conf /etc/apache2/apache2.conf
#Exponer el puerto 80 y 443 
EXPOSE 80 443
#Inicio de Apache
CMD ["apachectl", "-D", "FOREGROUND"]