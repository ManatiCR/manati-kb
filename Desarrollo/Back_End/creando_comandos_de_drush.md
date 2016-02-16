# Creando comandos de Drush

Si tu módulo necesita integración con Drush; esta es muy fácil de realizar; para ello, necesitamos un archivo [nombre-modulo].drush.inc

Lo primero que necesitamos es implementar el ```hook_drush_command```

```
/**
 * Implemts hook_drush_command().
 */
function mimodulo_drush_command() {
  $items = array();
  $items['mi-comando'] = array(
    'description' => dt('Descripción de mi comando de ejemplo'),
    'examples' => array(
      'Standard example' => 'drush mi-comando',
    ),
    'aliases' => array('mmmc'),
    'callback' => 'drush_mimodulo_mi_comando',
    'arguments' => array(
      'argumento' => 'Descripción para este argumento',
    ),
  );
  return $items;
}
```
Luego; en la función ```drush_mimodulo_mi_comando ```realizamos las acciones necesarias.


```
function mimodulo_mi_comando($argumento) {
  // Aquí realizamos todo lo necesario para mi-comando.
}
```

Una vez hecho lo anterior, con sólo habilitar el módulo (drush en -y mimodulo y limpiar la caché de drush (si ya estaba habilitado) (drush cc drush) tenemos nuestro nuevo comando disponible.

Espero que sea de ayuda.
