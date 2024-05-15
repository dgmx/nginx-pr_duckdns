## Uso de NGINX Proxy Manager y DuckDNS para exponer aplicaciones locales utilizando SSL con Docker 

 NGINX es un **servidor web** que permite el uso como *proxy inverso*. NGINX Proxy Manager es un GUI que permite 
crear y administrar servicios o hosts detras del proxy con cobertura HTTPS, es decir, nos permite implementar 
certificados TLS en el proxy para no tener que hacer dicha configuración en cada uno de los servicios externos
que queremos exponer

NPM (Nginx Proxy Manager) usa internamente `Let's Encrypt` para gestionar y solicitar los certificados TLS que se 
aplicarán en cada servicio.

Para la configuracioón DNS vamos a utilizar DuckDNS, que es un servicio de DNS dinámico gratuito que nos permite 
registrar hasta 5 subdominios.
 
Para implementar estos servicios vamos a usar Docker Compose

## NGINX Proxy Manager

Para crear el contenedor de NPM creamos una carpeta para generar el archivo docker-compose.yml

```console
$ mkdir nginx-proxy-manager
$ cd nginx-proxy-manager

```
El contenido del archivo será el siguiente:

```docker

version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP

    # Uncomment the next line if you uncomment anything in the section
    # environment:
      # Uncomment this if you want to change the location of
      # the SQLite DB file within the container
      # DB_SQLITE_FILE: "/data/database.sqlite"

      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'

    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

Esta es la configuración básica minima, con una base de datos sqlite, no hace uso de una base de datos MariaDB que 
sería una configuración mas avanzada.

Para mas información sobre instrucciones de configuración podeis revisar la documentación oficial:

[Full Setup Instructions](https://nginxproxymanager.com/setup/)

