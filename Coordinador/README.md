
Despues de levantar todo el anillo de cassandra

docker exec -it coordinatorNode bash

cqlsh

CREATE KEYSPACE postulaciones WITH replication={'class':'SimpleStrategy', 'replication_factor' : 3};
use postulaciones;

crear tablas (redundancia permitida por paradigma de Cassandra segun PPT 38 presentacion 2 de cassandra)

CREATE TABLE postulacion_por_facultad (cedula text, periodo text, sexo text, preferencia int, carrera text, matriculado text, facultad text, puntaje int, grupo_depen text, region text, latitud float, longitud float, ptje_nem int, psu_prom int, pace text, gratuidad text, PRIMARY KEY(facultad, periodo, psu_prom));

CREATE TABLE postulacion_por_carrera (cedula text, periodo text, sexo text, preferencia int, carrera text, matriculado text, facultad text, puntaje int, grupo_depen text, region text, latitud float, longitud float, ptje_nem int, psu_prom int, pace text, gratuidad text, PRIMARY KEY(carrera, periodo, psu_prom));

Copiar datos en las tablas

COPY postulacion_por_facultad (cedula , periodo , sexo , preferencia , carrera , matriculado , facultad , puntaje , grupo_depen , region , latitud , longitud , ptje_nem , psu_prom , pace , gratuidad ) FROM 'postulaciones2.csv' WITH DELIMITER=';' AND HEADER=TRUE;

COPY postulacion_por_carrera (cedula , periodo , sexo , preferencia , carrera , matriculado , facultad , puntaje , grupo_depen , region , latitud , longitud , ptje_nem , psu_prom , pace , gratuidad ) FROM 'postulaciones2.csv' WITH DELIMITER=';' AND HEADER=TRUE;