
Despues de levantar todo el anillo de cassandra

docker exec -it coordinatorNode bash

cqlsh

CREATE KEYSPACE postulaciones WITH replication={'class':'SimpleStrategy', 'replication_factor' : 3};

use postulaciones;

crear tablas (redundancia permitida por paradigma de Cassandra segun PPT 38 presentacion 2 de cassandra)

CREATE TABLE postulacion_por_facultad (cedula text, periodo text, sexo text, preferencia int, carrera text, matriculado text, facultad text, puntaje int, grupo_depen text, region text, latitud float, longitud float, ptje_nem int, psu_prom int, pace text, gratuidad text, PRIMARY KEY(facultad, periodo, cedula));

CREATE TABLE postulacion_por_carrera (cedula text, periodo text, sexo text, preferencia int, carrera text, matriculado text, facultad text, puntaje int, grupo_depen text, region text, latitud float, longitud float, ptje_nem int, psu_prom int, pace text, gratuidad text, PRIMARY KEY(carrera, periodo ,psu_prom ,cedula));

CREATE TABLE postulacion_por_region_carrera (cedula text, periodo text, sexo text, preferencia int, carrera text, matriculado text, facultad text, puntaje int, grupo_depen text, region text, latitud float, longitud float, ptje_nem int, psu_prom int, pace text, gratuidad text, PRIMARY KEY((region,carrera), periodo, cedula));

Copiar datos en las tablas

COPY postulacion_por_facultad (cedula , periodo , sexo , preferencia , carrera , matriculado , facultad , puntaje , grupo_depen , region , latitud , longitud , ptje_nem , psu_prom , pace , gratuidad ) FROM 'postulaciones2.csv' WITH DELIMITER=';' AND HEADER=TRUE;

COPY postulacion_por_carrera (cedula , periodo , sexo , preferencia , carrera , matriculado , facultad , puntaje , grupo_depen , region , latitud , longitud , ptje_nem , psu_prom , pace , gratuidad ) FROM 'postulaciones2.csv' WITH DELIMITER=';' AND HEADER=TRUE;

COPY postulacion_por_region_carrera (cedula , periodo , sexo , preferencia , carrera , matriculado , facultad , puntaje , grupo_depen , region , latitud , longitud , ptje_nem , psu_prom , pace , gratuidad ) FROM 'postulaciones2.csv' WITH DELIMITER=';' AND HEADER=TRUE;

Para conectar a excel

Bajarse ODBC de aca https://downloads.datastax.com/#odbc-jdbc-drivers (si es de 32 o 64 bits depende de la version de excel que se ve abriendo excel->inicio->cuenta->acerca de excel)

Instalar e iniciar, apretar en agregar, luego elegir cassandra, configurar nombre, host (ip del WSL aunque diria que quizas funciona con la de windows/AWS, usar ipconfig para esto), puerto 9042 y default keyspace postulaciones

# Consideraciones

## Tabla postulacion_por_facultad

Se considera como las filas que debe devolver la BD si un postulante tiene dos postulaciones en una misma facultad, devuelve solo una fila