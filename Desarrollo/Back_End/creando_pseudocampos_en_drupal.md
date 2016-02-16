# Creando pseudocampos en Drupal

Imaginá que necesitás desplegar contenido relativo a un nodo, pero que no se relaciona directamente con ninguno de los campos del mismo; o se relaciona con varios de estos. Cómo hacemos? Podemos utilizar un pseudo-campo; es decir; un campo que solo se muestra en el despliegue del nodo; pero nunca se ingresa; pues para mostrarlo se obtiene con cálculos o algún trozo custom de código.

Ahora sí; cómo hago eso? [hook_field_extra_fields](https://api.drupal.org/api/drupal/modules%21field%21field.api.php/function/hook_field_extra_fields/7) y [hook_node_view](https://api.drupal.org/api/drupal/modules%21node%21node.api.php/function/hook_node_view/7) vienen al rescate.

Veamos un ejemplo:

```
/**
 * Implements hook_field_extra_fields().
 */
function demo_field_extra_fields() {
  $extra['node']['page']['display']['demo_field'] = array(
    'label' => t('My Demo Field'),
    'description' => t('Provides a Demo Field'),
    'weight' => 0,
  );
  return $extra;
}

/**
 * Implements hook_node_view().
 */
function demo_node_view($node, $view_mode, $langcode) {
  if ($node->type === 'page') {
    $node->content['demo_field'] = array(
      '#type' => 'item',
      '#title' => '',
      '#markup' => 'Content for your demo field',
    );
  }
}
```

En el primer hook le dijimos a Drupal que para los nodos de tipo page en el display agregue un campo demo_field con esas propiedades. En el segundo hook le dijimos a Drupal que si el nodo es de tipo page en content (lo que se va a desplegar) agregue ```demo_field``` (mismo nombre que el extra field) con esas propiedades.

Espero que les pueda servir de algo :)