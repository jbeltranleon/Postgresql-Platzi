__jbeltranleon@X405UA:~/Documents/estudio/postgresql$__ psql

psql (9.6.6)

Type "help" for help.

__jbeltranleon=#__ \c jbeltranleon

You are now connected to database "jbeltranleon" as user "jbeltranleon".

__jbeltranleon=#__ CREATE ROLE rol_usuario LOGIN PASSWORD 'pass123';

CREATE ROLE

__jbeltranleon=#__ SELECT * FROM pg_roles

__jbeltranleon=#__ DROP ROLE rol_usuario;

DROP ROLE


__jbeltranleon=#__ CREATE ROLE rol_usuario LOGIN ENCRYPTED  PASSWORD 'pass123';

CREATE ROLE

__jbeltranleon=#__ CREATE ROLE usuario_finito LOGIN ENCRYPTED PASSWORD 'pass123' VALID UNTIL '2018-2-2';

CREATE ROLE

__jbeltranleon=#__ CREATE ROLE usuario_infinito_crea_bd LOGIN ENCRYPTED PASSWORD 'pass123' CREATEDB VALID UNTIL 'infinity';

CREATE ROLE

Podemos heredar permisos usando:
__jbeltranleon=#__ CREATE ROLE rol_herencia INHERIT;

CREATE ROLE

El único permiso que no se puede heredar es el de superusuario

De esta forma se heredan los pg_roles

__jbeltranleon=#__ GRANT usuario_infinito_crea_bd TO rol_herencia;
GRANT ROLE

Podemos cambiar a otro rol usando SET

__jbeltranleon=#__ SET ROLE usuario_infinito_crea_bd;

SET


> Podemos crear una base de datos y convertirla en una plantilla.

__jbeltranleon=#__ \c jbeltranleon

You are now connected to database "jbeltranleon" as user "jbeltranleon".

__jbeltranleon=#__ CREATE DATABASE curso_pg;

CREATE DATABASE

__jbeltranleon=#__ DROP DATABASE curso_pg;

ERROR:  database "curso_pg" is being accessed by other users
DETAIL:  There is 1 other session using the database.

__jbeltranleon=#__ DROP DATABASE curso_pg;

DROP DATABASE

__jbeltranleon=#__ CREATE DATABASE curso_pg TEMPLATE template1;
CREATE DATABASE

__jbeltranleon=#__ UPDATE pg_database SET datistemplate = true WHERE datname = 'curso_pg;

__jbeltranleon=#__ CREATE SCHEMA contabilidad;

CREATE SCHEMA

curso_pg=# CREATE ROLE other_user LOGIN ENCRYPTED PASSWORD 'pass123' CREATEDB VALID UNTIL 'infinity';

CREATE ROLE

curso_pg=# GRANT ALL ON ALL TABLES IN SCHEMA contabilidad TO other_user;

GRANT

__jbeltranleon=#__ CREATE SEQUENCE secuencia_ejemplo;

CREATE SEQUENCE

__jbeltranleon=#__ SELECT nextval('secuencia_ejemplo');

 nextval
---------
       1
(1 row)

__jbeltranleon=#__ SELECT currval('secuencia_ejemplo');

 currval
---------
       1
(1 row)

__jbeltranleon=#__ SELECT nextval('secuencia_ejemplo');

 nextval
---------
       2
(1 row)

__jbeltranleon=#__ SELECT setval('secuencia_ejemplo', 1);

 setval
--------
      1
(1 row)

__jbeltranleon=#__ SELECT currval('secuencia_ejemplo');

 currval
---------
       1
(1 row)

__jbeltranleon=#__ SELECT nextval('secuencia_ejemplo');

 nextval
---------
       2
(1 row)

__jbeltranleon=#__ SELECT lpad('ab', 3, '0') AS pad, repeat('-', 4) || 'zy' AS dash, trim('    tr    ') AS trim;

 pad |  dash  | trim
-----+--------+------
 0ab | ----zy | tr
(1 row)

__jbeltranleon=#__ SELECT split_part('324-472-8899', '-', 2) AS x;

  x  
-----
 472
(1 row)

__jbeltranleon=#__ SELECT string_to_array('aaa.bbb.ccc', '.') AS y;

       y       
---------------
 {aaa,bbb,ccc}
(1 row)

__jbeltranleon=#__ SELECT ARRAY[1997, 2008, 2016] AS yrs;

       yrs        
------------------
 {1997,2008,2016}
(1 row)

## Los dos puntos son usados cuando se realiza un cast o una conversión de datos (como un parse)


__jbeltranleon=#__ SELECT '{gato, conejo}'::text[] AS animales;
   animales    
---------------
 {gato,conejo}
(1 row)

__jbeltranleon=#__ SELECT animales[1] FROM (SELECT  '{gato, conejo}'::text[] AS animales) AS primer;
 animales
----------
 gato
(1 row)

__jbeltranleon=#__ SELECT animales[2:5] FROM (SELECT  '{gato, conejo, pato, perro, mico, gallina}'::text[] AS animales) AS primer;

         animales         
--------------------------
 {conejo,pato,perro,mico}
(1 row)

__jbeltranleon=#__ SELECT UNNEST(animales[2:5]) FROM (SELECT  '{gato, conejo, pato, perro, mico, gallina}'::text[] AS animales) AS primer;

 unnest
--------
 conejo
 pato
 perro
 mico
(4 rows)


## Rangos

* Los paréntesis () determinan que el valor no se incluye en el rango
* Los corchetes [] sí incluyen el valor en el rango

__jbeltranleon=#__ SELECT '(0,6)'::int8range;
 int8range
-----------
 [1,6)
(1 row)

__jbeltranleon=#__ SELECT '[0,6)'::int8range;
 int8range
-----------
 [0,6)
(1 row)

__jbeltranleon=#__ SELECT '[0,6]'::int8range;
 int8range
-----------
 [0,7)
(1 row)

__jbeltranleon=#__ SELECT '[2016-09-10, 2018-01-06]'::daterange;

        daterange        
-------------------------
 [2016-09-10,2018-01-07)
(1 row)

__jbeltranleon=#__ SELECT '[78,]'::int8range;

 int8range
-----------
 [78,)
(1 row)

__curso_pg=#__ CREATE TABLE profiles(id SERIAL PRIMARY KEY, profile JSON);

CREATE TABLE

__curso_pg=#__ SELECT * FROM profiles;

 id | profile
----+---------
(0 rows)

__curso_pg=#__ INSERT INTO profiles(profile) VALUES (

__curso_pg(#__ '{"name":"Jhon", "tech": ["postgresql", "python", "wordpress"]}');

INSERT 0 1

__curso_pg=#__ INSERT INTO profiles(profile) VALUES (
'{"name":"Fredy", "tech": ["bash", "django"]}');

INSERT 0 1

__curso_pg=#__ SELECT * FROM profiles;

 id |                            profile                             
----+----------------------------------------------------------------
  1 | {"name":"Jhon", "tech": ["postgresql", "python", "wordpress"]}
  2 | {"name":"Fredy", "tech": ["bash", "django"]}
(2 rows)

__curso_pg=#__ SELECT json_extract_path_text(profile, 'name') FROM profiles;

 json_extract_path_text
------------------------
 Jhon
 Fredy
(2 rows)

__curso_pg=#__ CREATE TABLE profiles_binary(id SERIAL PRIMARY KEY, profile JSONB);

CREATE TABLE

__curso_pg=#__ INSERT INTO profiles_binary(profile) VALUES (                     
'{"name":"Fredy", "tech": ["bash", "django"]}');

INSERT 0 1

__curso_pg=#__ INSERT INTO profiles_binary(profile) VALUES (                     
'{"name":"Jhon", "tech": ["postgresql", "python", "wordpress"]}');

INSERT 0 1

__curso_pg=#__ SELECT * FROM profiles_binary;
                             
 id |                             profile                             
----+-----------------------------------------------------------------
  1 | {"name": "Fredy", "tech": ["bash", "django"]}
  2 | {"name": "Jhon", "tech": ["postgresql", "python", "wordpress"]}
(2 rows)
