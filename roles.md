__jbeltranleon@X405UA:~/Documents/estudio/postgresql$__ psql

psql (9.6.6)

Type "help" for help.

__jbeltranleon=#__ \c jbeltranleon

You are now connected to database "jbeltranleon" as user "jbeltranleon".

__jbeltranleon=#__ CREATE ROLE rol_usuario LOGIN PASSWORD 'pass123';

CREATE ROLE

__jbeltranleon=#__ SELECT * FROM pg_roles
