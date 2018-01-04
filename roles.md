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
