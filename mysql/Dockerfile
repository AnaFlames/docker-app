#Imagen base: MySQL oficial, siempre con versión
FROM mysql:8.4

#Los .sql de esta carpeta se cargan solos la PRIMERA vez (volumen vacío)
COPY biblioteca.sql /docker-entrypoint-initdb.d/

