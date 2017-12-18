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

__Service__ :arrow_lower_right: __Database__: Organización lógica de datos y código (posee una relación a archivos físicos en el directorio de archivos):arrow_lower_right: __Schema__: Organización lógica de datos y código (con el esquema podemos separar datos...)

__Tablespace__: Es un archivo en donde se indica la distribución que va a tener la base de datos en por ejemplo: _un arreglo de discos_

__Catalog__: En donde se encuentran las bases de datos que almacenan los metadatos de los esquemas, ejemplo: _¿cuantas tablas tengo creadas en el esquema?_

__Archivos de configuración__: Tener cuidado al editar las configuraciones :no_entry_sign:

* __postgresql.conf__: memoria, rutas de almacenamiento (especifico para instancia)
* __pg_hba.conf__: controla el acceso a nuestra instancia,
* __pg_ident.conf__: mapeo de usuarios

*********************************************************
* postgresql.conf.- Es el archivo de configuración general, aca puedo cambiar la cambiar la cantidad de memoria que puedo utilizar.
        * max_connections=100 #requiere que se reinicie y cada uno gasta 400 bytes por conexión.

    * pg_hba.conf.- Se aceptan conexiones solo de LAN o de conexiones cifradas o de solo usuarios del sistema operativo
        * Cuando tengo en METHOD trust no requiere autenticación.
            * TRUST: Si estamos en el mismo servidor. Menos segura, no requiere password.
            * PASSWORD: Requiere password, lo manda en simple texto.
            * MD5: Requiere password, está cifrado con hash MD5.
            * IDENT: Utiliza el archivo pg_ident.conf, si estamos autenticados con el usuario del sistema operativo podremos ingresar sin autenticación. Debes agregarlo en pg_ident.conf y pg_hba.conf

    * pg_ident.conf.- Sirve para poder automáticamente logeo asociando el usuario de postgree al del sistema operativo.
    * data_directory.- Donde se encuentran los tablespaces.
    * ¿Dónde estan? Normalmente estan /usr/local/postgree o en otro sitio. Tambien lo puedes encontrar con la query: "SELECT name, setting FROM pg_settings WHERE category = 'File Locations';"
    * En psql puedes hacer "\h" para ayuda
    * En psql puedes hacer "\q" para salir
    * ¿Cómo se modifica? .- Lo más seguro es modificar por query.
        * SELECT name, context, unit, setting, boot_val, reset_val FROM pg_settings WHERE name IN ('listen_addresses', 'max_connections', 'shared_buffers', 'effective_cache_size', 'work_mem', 'maintenance_work_mem') ORDER BY context, name; --¿Cómo mirar los parámetros de la instancia de base de datos.?
        * Los parámetros que estan con postmaster se pueden cambiar con necesidad de reinicio.
        * Los parámetros user se pueden cambiar en caliente.
        * ALTER SYSTEM set work_mem=8192; --Actualizar la cantidad de memoria. Se guardan en el archivo postgresql.auto.conf, para que se actualicen cuando reinicies el servidor.
        * SELECT pg_reload_conf(); --función que ayuda a recargar a configuración de PostreSQL.

    * SELECT * FROM pg_stat_activity; --¿Quién tiene actividad en nuestro servidor?
    * SELECT pg_cancel_backend(proc id); --¿Cómo terminar la operación de una query?
    * SELECT pg_terminate_backend(proc id); --¿Cómo cerrar una session?
*********************************************************

> UN __SERVICE__ PUEDE CONTENER UNA O MÁS __BASES DE DATOS__
> | UNA __BASE DE DATOS__ PUEDE CONTENER UNO O MÁS __SCHEMAS__
