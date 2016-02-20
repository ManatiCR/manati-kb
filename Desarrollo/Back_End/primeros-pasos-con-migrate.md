# Primeros pasos con Migrate
Página del proyecto: https://www.drupal.org/project/migrate

Cuando estamos frente al reto de una migración siempre se tiene la intriga de: Cual es la mejor opción para migrar nuestro contenido? normalmente la respuesta es Migrate. Migrate es un módulo que provee todo un API de especializado para realizar migraciones, con el podemos realizar migraciones de un drupal a otro drupal, de una Base de datos X a drupal, incluso podemos realizar migraciones desde archivos de texto (CSV).

Migrate es muy versatil, pero hay un detalle, es algo complejo empezar con el dado que no hay mucha documentación en la web, practicamente la mejor documentación es el mismo código de los módulos de migrate, por eso acá daremos un pequeño paseo sobre como iniciar con migrate y sus configuraciones iniciales.
Como pueden suponer Migrate no es algo que podamos configurar desde interfaz gráfica, lo único que quedará disponible desde interfaz gráfica sera la interfaz de migración (Desde donde le diremos al sistema que inicie la migración), todo el resto tiene que ser definido via código, por lo que tenemos que crear un módulo custom donde implementaremos los métodos del API de migrate.



##Instalación de migrate.


Si tenemos Drush a mano podemos descargarlo usando:

```drush dl migrate```

si no fuera el caso podemos ir a la [página del proyecto](https://www.drupal.org/project/migrate) y descargarlo y activarlo manualmente.


Normalmente cuando estamos realizando migraciones necesitamos de este otro amigo [Migrate extras](https://www.drupal.org/project/migrate_extras), ya que migrate extras nos provee integraciones con muchos otros módulos contrib, por ejemplo el módulo countries,location, flag, field colletion, entre otros. Por lo que quedá en ustedes y su caso de migración ver si en verdad es necesario este módulo. Dentro de la página del proyecto anteriormente citada pueden verificar para cuales módulos proveé integraciones.

Lo restante es activar el o los módulos deseados, dentro de migrate vienen varios submodulos, normalmente activamos migrate y migrate_ui.



##Desarrollo de la migración.
 
En este enlace hay documentación básica de como arrancar el proceso de desarrollo de la migración [Getting started with Migrate](https://www.drupal.org/node/1006982).


Sigue el procedimiento básico de desarrollo de cualquier módulo para drupal.

### Achivo mi_migracion.module

El archivo ```.module``` en este caso normalmente está vacío, pero se puede usar para definir funciones 'utilitarias' por ejemplo, una función para obtener el ultimo elemento importado.


### Archivo mi_migracion.migrate.inc

En nuestro archivo ```mi_migracion.migrate.inc``` describimos o definimos las migraciones que vamos a tener en nuestra migración así como nuestros grupos de migración.

Para este propósito implementamos el hook ```hook_migrate_api()``` donde realizaremos la descripción de nuestras clases de migración.

```
/**
 * Implements hook_migrate_api().
 */
 
function mi_migracion_migrate_api() {
  $api = array(
    'api' => 2,
    'groups' => array(
      'content_migration' => array(
        'title' => t('Content migration'),
      ),
    ),
    'migrations' => array(
      'NewsNodes' => array(
        'class_name' => 'NewsNodesMigration',
        'group_name' => 'content_migration',
      ),
     ),
  );
  return $api;
}
```

**Los grupos de migración** además de dar orden, son sumamente importantes en caso de que queramos agilizar el proceso de invocación de nuestras migraciones. 
Por ejemplo: Tenemos un conjunto de migraciones para distintos tipos de contenido (Eventos, noticias, entre otros.) los cuales queremos ejecutar, si estás migraciones no están en un mismo grupo sería tedioso ir una por una desde la interfaz gráfica o desde ```drush``` realizando la invocación de cada una de las migraciones, en cambio, si nuestras migraciones están en un mismo grupo podemos llamarlas invocar solamente la ejecución del grupo de migración y migrate se encargara de ir llamando una por una cada migración.

Otro caso en el que son importantes **los grupos de migración **es cuando tenemos una migración dependiente de otra migración. Por ejemplo: En nuestra migración de noticias debemos de llenar unos field collections, para poder realizarlo necesitamos que primero exista la noticia (nodo noticia), esto por que el field collettion es un elemento que se adjunta a contenido existente; Lo que hace en este caso es crear 2 clases de migración, una para nuestros nodos de noticias y otra para nuestros field colletion, el paso siguente es decirle a nuestra clase de migración de field colletions que va a tener como dependencia la clase de migración de noticias. Esto nos asegura que al invocar nuestro grupo de migración primero se va invocar primero la migración de noticias y luego la del field colletion.


Más adelante veremos con un ejemplo en código como aplicar las dependencias y como jugar con ellas.



###Clases de migración.

###Archivo mi_migracion.info




