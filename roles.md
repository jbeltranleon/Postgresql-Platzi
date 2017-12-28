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
