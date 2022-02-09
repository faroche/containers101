# Práctica de Docker desde 0

La podemos realizar desde una conexión SSH, powershell o directamente en la línea de comandos

#### Descargar una imágen o contenedor

```powershell
docker pull <nombre_imagen>
```

Ejemplo: Descargar imagen oficial de PostgreSQL

```bash
docker pull postgres
```

Se nos permite descarga diferentes versiones del contenedor, por medio de la etiqueta -> container_name:tag

```bash
docker pull postgres:latest
docker pull postgres:9.6
docker pull postgres:9-alpine
```

#### Listar las imágenes o contendores descargados

```bash
docker images
```

#### Ejecutar imagen recién descargada

```bash
docker run <nombre_contenedor> || <contenedor_id>
```

Parámetros más comunes:

-d ejecución en background
-p exponemos puertos de contenedor puertolocal:puertocontenedor
-v volumes mapeamos folder o storage local al contenedor  ~/app:/app

#### Listar contenedores en ejecución

```powershell
docker ps 
```

#listar historial de contenedores que se ejecutaron

```bash
docker ps -a
```

#### Ejecutar comandos dentro de contenedores

```
docker exec <contenedor_id> 
```

#### Ejecutar una sesión interactiva con el contenedor

```
docker exec -it <contenedor_id> comando
```

#### Detener la ejecución de un contenedor

```
docker stop <contenedor_id>
```

#### Borrar un contenedor

```
docker rmi <contenedor_id>
```

#### Cómo construir nuestros contenedores?

##### Ejemplo 1:

Creamos contenedor en base a un repositorio de github, para lo cual clonamos este repositorio por medio de:

```bash
git clone https://github.com/TylerPottsDev/weather-app-js
```

Creamos un Docker File, este debe tener como nombre Docker file

```yaml
#crear Dockerfile
nano Dockerfile


FROM nginx
COPY . /usr/share/nginx/html
```

Construimos el contenedor a partir del Dockerfile recién creado:

```bash
docker build -t mi-primer-app:dev .
```

Parametros:

> build - instrucción para crear el contenedor
>
> -t <nombrecontenedor:tag>
>
> .  Copia los archivos que estan el directorio actual hacia el contenedor

##### Docker file best practices 

- https://docs.docker.com/develop/develop-images/dockerfile_best-practices/

- https://takacsmark.com/dockerfile-tutorial-by-example-dockerfile-best-practices-2018/

Ejecutamos nuestro contenedor recien creado, en background, exponiendo el puerto 8080 en host y 80 en el contenedor y ejecutamos este con el tag

```bash
docker run -dp 8080:80 mi-primer-app:dev
```

También podemos ejecutar este, montando un volumen desde el filesystem del host hacia el contenedor y podemos ser capaces de editar los archivos en tiempo real y estos se verán reflejados en el contenedor:

```bash
docker run -dp 8080:80 -v ~/dev/weather-app-js:/usr/share/nginx/html mi-primer-app:dev
```

##### Ejemplo 2 - Datos persistentes 

Clonamos el repostorio:

```bash
git clone https://github.com/faroche/containers101
```

Construimos el contenedor con los archivos clonados del repositorio:

```bash
docker build -t la-otra-app:dev .
```

Ejecutamos el contenedor:

```
docker run -dp 8081:3000 la-otra-app:dev
```

y podemos conectarnos con el browser a http://localhost:8081 y podemos agregar datos a la base de datos que corre en el contenedor pero cuando detenemos este, los datos se pierden.

Una solucion es ejecutar el contenedor con los archivos de la base de datos directamente del host y no desde el contenedor de la siguiente forma: 

```bash
docker run -dp 8081:3000 -v ~/dev/containers101:/etc/todos la-otra-app:dev
```

##### Ejemplo 3 - Docker compose y sesiones interactivas 

Ejecutar sesiones o comandos en el contenedor, iniciamos con la descarga del siguiente imagen:

```bash
docker pull mysql
```

Ejecutamos el contenedor:

```bash
docker-compose up -d
```

creamos la session bash conectandonos al contenedor:

```bash
docker exec -it <contendor_id> mysql -p
```

Documentacion de mysql -> https://hub.docker.com/_/mysql

Ejemplo 4 - Utilizar el docker compose para ejecutar el contenedor que construimos en el ejemplo 1:

```yaml
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
```

