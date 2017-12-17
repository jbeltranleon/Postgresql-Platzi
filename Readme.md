# PostgreSQL

### Soporta:

* Stored Procedures: Multiples lenguajes. Extensiones, pueden estar escritos en:
    - SQL
    - PL/pgSQL
    - PL/Perl
    - PL/Python :full_moon_with_face:
    - PL/V8 _(javascript)_
    - PL/R

* Funciones de agregación: __AVG__, __COUNT__, Funciones de agregación personalizadas, ejemplo: _mediana_ :sunglasses:

* Datos Complejos:
    - Fracciones: __¼__
    - Json _(podemos acceder a sus llaves y valores, no sólo guardarlos como texto)_ :key:
    - Hstore _(para guardar cualquier tipo de dato no estructuado)_ :email:
    - PostGIS _(geolocalización)_ :earth_americas:

### Donde encontramos Postgresql

* Heroku :purple_heart:
* Amazon RDS :yellow_heart:

### Herramientras Administrativas y Desarrollo

* PSQL  :arrow_right: __Texto/Terminal__
* PgAdmin :arrow_right: __UI__

## Entendiendo PostgreSQL

* Organización de artefactos u objetos lógicos como físicos
    - Físicos _(librerias...)_
    - Lógicos _(funcionamiento del motor)_

### Servidor Físicos
* librerias y archivos físicos
* Servicios :arrow_right: __Instancia:__ Responde a una dirección IP, tiene sus propios permisos de acceso y mantiene aislada su información, ejemplo: _instancia administrativa, instancia área de producción, que no tienen nada de relación una con la otra._

__Service__

:arrow_double_down:

__Database__
