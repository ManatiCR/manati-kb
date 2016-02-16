# Como utilizar Master (Módulo Drupal 7)

Master es un módulo de gran utilidad cuando tenemos diversos ambientes (dev, test, live) ya que normalmente cada uno de estos ambientes poseen distintas necesidades (Módulos, configuraciones, entre otros), en este caso Master viene a ayudarnos con la Parte de los módulos, administrando que módulos están o no activos por ambiente.

Básicamente con Master definimos desde nuestro settings listados de módulos, definimos uno como nuestro listado base de módulos, módulos que estarán presentes en todos los ambientes, posteriormente empezamos a definir listados por ambiente, para nuestro ambiente local, para el server dev, test y live o los que tengamos, con esto nos aseguramos de tener solamente los módulos necesarios por ambiente.


## Instalación.

Si estamos en un proyecto en el que todo debe estar definido en código se preguntaran - Si Master activa los módulo que le indiquemos, quien activa a Master? - Para está pregunta tenemos 2 opciones , ponerlo como dependencia de un Perfil de instalación (FALTA ENLACE A QUE ES UN PERFIL y como crearlo) de nuestro sitio o simplemente activarlo de manera manual ya sea por **drush** o desde la interfaz de módulos de drupal.


## Configuración.

Como es bien sabido el``` settigs.php``` de nuestro drupal no es solo para poner las credenciales de nuestra base de datos, acá se puede colocar configuración global para nuestro sitio, en este caso Master hace uso del``` settings.php``` para obtener la configuración proveída por nosotros.

Acá un ejemplo básico de como utilizar Máster:

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