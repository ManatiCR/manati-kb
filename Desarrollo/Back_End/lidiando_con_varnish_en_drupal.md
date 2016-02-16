# Lidiando con Varnish en Drupal

### No aplica para pantheon.


Imaginá que tenés un sitio en Drupal con un tamaño moderado; entonces tuviste que usar varnish como un proxy caché para el mismo. En este mismo sitio tenés en el homepage algunas vistas que muestran el contenido más reciente. Acabamos de toparnos con un problema: cuando ingresa nuevo contenido, esta vista no se refresca porque varnish no se entera de que hay contenido nuevo; qué hacer en este caso?

La solución es sencilla; echamos mano de dos módulos contrib expire y varnish; los descargamos y habilitamos como de costumbre.

Luego; en la configuración de varnish introducimos los parámetros necesarios para que Drupal pueda conectarse con la consola del Varnish. Al final de esta página tenemos el estado de dicha conexión.

Acto seguido, en el módulo expire; configuramos el comportamiento deseado cada vez que ingresa una entidad de cada uno de los tipos actualmente disponibles en el sistema (nodos, términos de taxonomía, usuarios, etc).

Hechas ambas configuraciones; lo único pendiente es agregar unas líneas que necesita el módulo varnish en el archivo ```settings.php``` .


```
// Add Varnish as the page cache handler.
$conf['cache_backends'] = array('sites/all/modules/varnish/varnish.cache.inc');
// Drupal 7 does not cache pages when we invoke hooks during bootstrap. This needs
// to be disabled.
$conf['page_cache_invoke_hooks'] = FALSE;
$conf['cache_class_external_varnish_page'] = 'VarnishCache';
```

Una vez listo todo esto; tu varnish debería estar funcionando de la mejor manera; por supuesto; una limpieza de caché sería necesaria para dar por finalizada nuestra configuración.

