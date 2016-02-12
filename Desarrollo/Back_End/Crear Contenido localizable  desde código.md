
# Crear Contenido localizable desde código.



Hay diversas formas y posibles esenarios en los cuales necesitemos crear contenido en nuestros sitios desde código (Habitualmente desde un hook_update_N o desde hook_install), para este caso tomaré un caso muy comun, crear terminos de taxonomía desde código con su propiedad name en español e ingles (Estos terminos son localizados).

```/**
 * Create localized taxonomy terms.
 */
function my_module_update_7000() {
  $vocabulary = taxonomy_vocabulary_machine_name_load('my_vocabulay');
  
  //In this implemetation we use a keyed arrray, the key is the value in English and the "value" is the value in
  //spanish for the name property of the taxonomy term.
  
  $new_terms_strings = array(
    'My term 1' => 'Mi termino 1',
    'My term 2' => 'Mi termino 2',
    'My term 3' => 'Mi termino 3',
  );
  foreach($new_terms_strings as $original_string => $translation) {
    $term = new stdClass();
    $term->name = $original_string;
    $term->vid = $vocabulary->vid;
    taxonomy_term_save($term);
    i18n_string_translation_update(array('taxonomy', 'term', $term->tid, 'name'), $translation, 'es');
  }
}
```
Como se puede observar la función encargada de realizar la asignación de la traducción es i18n_string_translation_update, a esta función le pasamos como parametros un arrelo de opciones, la traducción y el lenguaje de la traducción que estamos agregando.
Si necesita más información sobre traducciones y está función puede seguir el siguiente enlace - https://www.drupal.org/node/1114010
 

