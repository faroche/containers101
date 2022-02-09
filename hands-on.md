#descargar una imagen o contenedor
docker pull <nombre_imagen>

ej:
docker pull postgres

docker pull postgres:latest
docker pull postgres:9.6
docker pull postgres:9-alpine


#listar las imagenes o contendores descargados
docker images

#ejecutar imagen recién descargada
docker run <nombre_contenedor> || <contenedor_id>

-d ejecución en background
-p exponemos puertos de contenedor puertolocal:puertocontenedor
-v volumes mapeamos folder o storage local al contenedor  ~/app:/app

#listar contenedores en ejecución
docker ps 

#listar historial de contenedores que se ejecutaron
docker ps -a

#ejecutar comandos dentro de contenedores
docker exec <contenedor_id> 

#ejecutar una sesion interactiva con el contenedor
docker exec -it <contenedor_id> comando

#detener la ejecución de un contenedor
docker stop <contenedor_id>

#borrar un contenedor
docker rmi <contenedor_id>


#########################################
#Ejemplo 1 - crear contenedor en base a un repositorio de github
git clone https://github.com/TylerPottsDev/weather-app-js

#crear Dockerfile
nano Dockerfile


FROM nginx
COPY . /usr/share/nginx/html

docker build -t mi-primer-app:dev .

# Docker file best practices -> https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
#                            -> https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/

#ejecutar nuestro contenedor
docker run -dp 8080:80 mi-primer-app:dev

docker run -dp 8080:80 -v ~/dev/weather-app-js:/usr/share/nginx/html mi-primer-app:dev

#Ejemplo 2 - 
git clone https://github.com/faroche/containers101

docker build -t la-otra-app:dev .

docker run -dp 8081:3000 la-otra-app:dev
docker run -dp 8081:3000 -v ~/dev/containers101:/etc/todos la-otra-app:dev

#Ejemplo 3 - docker compose y sesiones interactivas 
Ejecutar sesiones o comandos en el contenedor
#descargamos imagen
docker pull mysql
#ejecutamos el contenedor
docker-compose up -d
#session bash
docker exec -it <contendor_id> mysql -p

Documentacion de mysql -> https://hub.docker.com/_/mysql

#docker compose
#crear docker-compose.yml

version: "3.2"

services:
  mi-primer-app:
    image: mi-primer-app:dev
    ports:
      - 8080:80
    volumes:
      - ~/dev/weather-app-js:/usr/share/nginx/html

