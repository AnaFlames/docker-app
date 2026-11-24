# docker-app
# docker-app

Imagen Docker de la web biblioteca-web (Apache + PHP 8.2) para desplegarla en cualquier servidor con Docker.
Autor/a: <tu nombre> · 2º DAW · Despliegue de Aplicaciones Web

## Requisitos

- Docker Engine
- Git

## Estructura

- `apache/Dockerfile`: imagen de la web con Apache, PHP 8.2 y la extensión pdo_mysql.
- `apache/biblioteca-web/`: la web; tiene su propio repositorio, por eso no está aquí y se clona aparte.

## Puesta en marcha

1. Clonar:

```
git clone https://github.com/<tu-usuario-github>/docker-app.git
cd docker-app/apache
git clone https://github.com/AnaFlames/biblioteca-web.git
```

2. Construir y arrancar:

```
docker build -t my-php-image .
docker run -d --name my-php-container -p 8090:80 my-php-image
```

3. Abrir `http://<IP-del-servidor>:8090`.

## Puertos

| Servidor | Contenedor | Servicio |
|----------|------------|----------|
| 8090     | 80         | Apache (web) |

## Pendiente

## Base de datos

- `mysql/Dockerfile`: imagen `biblioteca-mysql` a partir de `mysql:8.4`, con `biblioteca.sql` en `/docker-entrypoint-initdb.d/` (se carga solo la primera vez, con el volumen vacío).
- Variables: `bd.env` (MYSQL_ROOT_PASSWORD, MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD) y `web.env` (DB_HOST, DB_NAME, DB_USER, DB_PASSWORD). No están en el repositorio: hay que crearlas.
- Volumen `datos-biblioteca` en `/var/lib/mysql`. Red `red-biblioteca`. MySQL no publica ningún puerto.

Arrancar, desde la raíz del repositorio:

```
cd mysql && docker build -t biblioteca-mysql . && cd ..
docker network create red-biblioteca
docker volume create datos-biblioteca
docker run -d --name bd --network red-biblioteca --env-file bd.env -v datos-biblioteca:/var/lib/mysql biblioteca-mysql
docker run -d --name web --network red-biblioteca --env-file web.env -p 8090:80 my-php-image
```

Empezar de cero (se borran los datos): `docker rm -f bd` y `docker volume rm datos-biblioteca`.- La base de datos MySQL: la página Libros todavía no funciona.
