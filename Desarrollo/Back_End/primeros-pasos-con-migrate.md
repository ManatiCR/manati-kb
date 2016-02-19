# Primeros pasos con Migrate
Página del proyecto: https://www.drupal.org/project/migrate

Cuando estamos frente al reto de una migración siempre se tiene la intriga de: Cual es la mejor opción para migrar nuestro contenido? normalmente la respuesta es Migrate. Migrate es un módulo que provee todo un API de especializado para realizar migraciones, con el podemos realizar migraciones de un drupal a otro drupal, de una Base de datos X a drupal, incluso podemos realizar migraciones desde archivos de texto (CSV).

Migrate es muy versatil, pero hay un detalle, es algo complejo empezar con el dado que no hay mucha documentación en la web, practicamente la mejor documentación es el mismo código de los módulos de migrate, por eso acá daremos un pequeño paseo sobre como iniciar con migrate y sus configuraciones iniciales.
Como pueden suponer Migrate no es algo que podamos configurar desde interfaz gráfica, lo unico que quedará disponible desde interfaz gráfica sera la interfaz de migración (Desde donde le diremos al sistema que inicie la migración), todo el resto tiene que ser definido via código, por lo que tenemos que crear un módulo custom donde implementaremos los métodos del API de migrate.



##Instalación de migrate.


Si tenemos Drush a mano podemos descargarlo usando:

```drush dl migrate```

si no fuera el caso podemos ir a la [página del proyecto](https://www.drupal.org/project/migrate) y decargarlo y activarlo manualmente.


Normalmente cuando estamos realizando migraciones necesitamos de este otro amigo [Migrate extras](https://www.drupal.org/project/migrate_extras), ya que migrate extras nos provee integraciones con muchos otros módulos contrib, por ejemplo el módulo countries,location, flag, field colletion, entre otros. Por lo que quedá en ustedes y su caso de migración ver si en verdad es necesario este módulo. Dentro de la página del proyecto anteriormente citada pueden verificar para cuales módulos proveé integraciones.

Lo restante es activar el o los módulos deseados, dentro de migrate vienen varios submodulos, normalmente activamos migrate y migrate_ui.



##Desarrollo de la migración.
 
En este enlace hay documentación básica de como arrancar el proceso de desarrollo de la migración [Getting started with Migrate](https://www.drupal.org/node/1006982).


Sigue el procedimiento básico de como crear 


