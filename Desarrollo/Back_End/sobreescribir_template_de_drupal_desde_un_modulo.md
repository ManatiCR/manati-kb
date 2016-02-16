# Sobreescribir template de Drupal desde un módulo

En una situación normal, para sobreescribir un template de Drupal, lo copio con el nombre correcto en la carpeta de templates del tema. Pero hay situaciones en las que no tenemos acceso al tema (tal vez un módulo contrib o sólo tenemos acceso a los módulos) y tenemos que realizar este sobreescritura desde un módulo. Drupal por default no lee templates desde los módulos, pero podemos cambiar ese comportamiento de forma muy sencilla con sólo implementar el``` hook_theme_registry_alter```. Ejemplo:

```
/**
 * Implements hook_theme_registry_alter().
 */
function demo_theme_registry_alter(&$theme_registry) {
  // Defined path to the current module.
  $templates_path = drupal_get_path('module', 'demo') . '/templates';
  // Find all .tpl.php files in this module's folder recursively.
  $template_file_objects = drupal_find_theme_templates($theme_registry, '.tpl.php', $templates_path);
  // Iterate through all found template file objects.
  foreach ($template_file_objects as $key => $template_file_object) {
    // If the template has not already been overridden by a theme.
    if (!isset($theme_registry[$key]['theme path']) || !preg_match('#/themes/#', $theme_registry[$key]['theme path'])) {
      // Alter the theme path and template elements.
      $theme_registry[$key]['theme path'] = $templates_path;
      $theme_registry[$key] = array_merge($theme_registry[$key], $template_file_object);
      $theme_registry[$key]['type'] = 'module';
    }
  }
}
```

Con lo anterior le estamos diciendo a Drupal que busque todos los archivos .tpl.php que encuentre en la carpeta templates del módulo demo y los trate como templates que podrían usarse para sobreescribir otros. De esta forma, logramos nuestro objetivo y ya podemos poder cualquier template dentro de /templates y utilizarlo para sobreescribir otros. Ojalá les sea útil.