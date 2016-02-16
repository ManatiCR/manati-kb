# Como utilizar Master (Módulo Drupal 7)

Master es un módulo de gran utilidad cuando tenemos diversos ambientes (base, dev, test, live) ya que normalmente cada uno de estos ambientes poseen distintas necesidades (Módulos, configuraciones, entre otros), en este caso Master viene a ayudarnos con la Parte de los módulos, administrando que módulos están o no activos por ambiente.

Básicamente con Master definimos desde nuestro settings listados de módulos, definimos uno como nuestro listado 'base' de módulos, módulos que estarán presentes en todos los ambientes, posteriormente empezamos a definir listados por ambiente, para nuestro ambiente local, para el server dev, test y live o los que tengamos, con esto nos aseguramos de tener solamente los módulos necesarios por ambiente.


## Instalación.

Si estamos en un proyecto en el que todo debe estar definido en código se preguntaran - Si Master activa los módulo que le indiquemos, quien activa a Master? - Para está pregunta tenemos 2 opciones , ponerlo como dependencia de un Perfil de instalación de nuestro sitio ([Creción de perfiles de instalación](/Desarrollo/Back_End/creacion_de_perfiles_de_instalacion_drupal_7.html)) o simplemente activarlo de manera manual ya sea por **drush** o desde la interfaz de módulos de drupal.


## Configuración.

Como es bien sabido el``` settigs.php``` de nuestro drupal no es solo para poner las credenciales de nuestra base de datos, acá se puede colocar configuración global para nuestro sitio, en este caso Master hace uso del``` settings.php``` para obtener la configuración proveída por nosotros.

Acá un ejemplo básico de como utilizar Master:

```
/**
 * Master module configuration.
 *
 * @see https://www.drupal.org/project/master
 */
$conf['master_version'] = 2;
$conf['master_allow_base_scope'] = TRUE;
$conf['master_modules'] = array();
$conf['master_modules']['base'] = array(
  // Core modules.
  'contact',
  'file',
  'image',
  'list',
  'menu',
  'number',
  'options',
  'path',
  'taxonomy',

  // Contrib modules.
  'admin_menu',
  'admin_views',
  'adminimal_admin_menu',
  'ckeditor',
  'ctools',
  );
  
  
  // Local environment.
$conf['master_modules']['local'] = array(
  'dblog',
  'devel',
  'diff',
  'field_ui',
  'memcache_admin',
  'migrate_ui',
  'rules_admin',
  'views_ui',
);

$conf['master_modules']['development'] = $conf['master_modules']['local'];
$conf['master_modules']['test'] = array();
$conf['master_modules']['production'] = array();
  
```

Como se puede ver en el ejemplo anterior se definieron varias listas de módulos para distintos ambientes (local y base); Con Master como ya mencionamos podemos tener los ambientes que deseemos por lo que el nombre para cada ambiente es de libre elección con una excepción, en caso de que queramos tener un ambiente 'base' (Ya mencionado anteriormente) para tener módulos fijos en todos los ambientes (**Recomendado**), Master ya tienes este nombre reservado para este propósito pero hay que indicarle que queremos hacer uso de esta característica y lo hacemos de la siguiente manera ```$conf['master_allow_base_scope'] = TRUE;```, como se puede ver en el ejemplo anterior esta linea está presente.


Tambien podemos ver algo curioso, que nuestro ambiente de 'development' va a tener lo mismo que  'local', lo cual tiene sentido ya que 'development'  en este contexto hace referencia al server de desarrollo, además tenemos nuestros ambientes de 'test'y 'production' igualados a un array vacío ya que solo nos interesa que estos ambientes tengan la configuración 'base'.

Nota: Si usamos la configuración base no agregar a ella módulos de UI o development.




##Como cambiar de scope.

Para cambiar nuestro 'scope' solo necesitamos utilizar un comando de drush:

``` drush master-set-current-scope [NOMBRE DEL AMBIENTE]```

##Exportar configuración inicial de master.

Es muy tedioso escribir la primera configuración de Master por que si queremos ahorrar tiempo definiendo nuestro listado de módulos  podemos usar el siguiente comando de drush:

```drush master-export [--scopes=local,stage,live]```


Más información sobre Master en el siguiente enlace: https://www.drupal.org/project/master


















