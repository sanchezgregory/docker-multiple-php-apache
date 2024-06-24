Para crear la red de la BD

docker network create mysql-network

Verificar si el conenedor tiene acceso al host para el xdebug
(entrar al docker)     docker exec -it php74 bash
(hacer ping al host)   ping host.docker.internal

En el docker compose debe estar esta directiva:

    extra_hosts:
      - "host.docker.internal:host-gateway"


El xdebug debe ser asi:
zend_extension=xdebug
xdebug.mode=develop,debug
xdebug.start_with_request=yes
xdebug.discover_client_host=0
xdebug.client_host=host.docker.internal
xdebug.client_port=9003
xdebug.log=/tmp/xdebug.log
xdebug.idekey=docker

Agregar en cada Proyecto general: php74, php80 php83 un archivo index.php con este contenido
<?php phpinfo(); echo xdebug_info();


Para quitar los procesos que tienen en uso al puerto 9003:

sudo lsof -i :9003
COMMAND      PID USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
docker-pr 522622 root    4u  IPv4 1169989      0t0  TCP *:9003 (LISTEN)
docker-pr 522631 root    4u  IPv6 1165816      0t0  TCP *:9003 (LISTEN)
sudo kill -9 522622
sudo kill -9 522631


En la carpeta de imagenes estan los captures para configurar Vscode y Phpstorm con xdebug


Configurar Xdebug PHPStorm

Abre las configuraciones de PhpStorm (Ctrl + Alt + S).
Navega a PHP > PHP.
En la sección Intérpretes, haz clic en el icono Más (un signo más).
Selecciona Local.
En el campo Ruta al intérprete PHP, introduce la ruta al ejecutable de PHP dentro de tu contenedor Docker. Por ejemplo, si tu ejecutable de PHP se encuentra en /usr/local/bin/php, introduce esa ruta.
En el campo Nombre, introduce un nombre descriptivo para tu intérprete de PHP, por ejemplo, "Intérprete PHP Docker".
Haz clic en Aplicar y luego en Aceptar

Configurar el servidor en PhpStorm:

Abre las configuraciones de PhpStorm (Ctrl + Alt + S).
Navega a Servidor.
Haz clic en Agregar.
Selecciona PHP Server.
En el campo Nombre, introduce un nombre descriptivo para tu servidor, por ejemplo, "Servidor Xdebug Docker".
En el campo Intérprete, selecciona el intérprete de PHP que configuraste anteriormente.
En la sección Configuración de la aplicación web, introduce la ruta a la raíz de tu proyecto web.
Haz clic en Aplicar y luego en Aceptar.

