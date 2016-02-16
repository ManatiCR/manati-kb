# hook_update_N con multipass

Cuando tenemos que realizar un``` hook_update_N``` que por su naturaleza es muy posible que rompa los timeouts de PHP; es recomendable utilizar la técnica de multipass; es decir, realizar un sólo ```hook_update_N``` pero en varias pasadas. Para ello, utilizamos el parámetro opcional del hook llamado &$sandbox y podemos realizarlo de la siguiente manera.

```
/**
 * Poner de autor al user 1 para los nodos creados por anonymous.
 */
 function demo_update_7000(&$sandbox) {
   if (!isset($sandbox['progress'])) {
     $sandbox['progress'] = 0;
     // Cantidad total de nodos.
     $sandbox['max'] = db_select('node', 'n')
       ->fields('n', array('nid'))
       ->condition('n.uid', 0)
       ->countQuery()
       ->execute()
       ->fetchField();
     // Aquí almacenaremos mensajes si fuera necesario.
     $sandbox['messages'] = array();
     // Aquí llevamos control del nodo actual.
     $sandbox['current_node'] = -1; 
   }
   // Cuántos nodos visitaremos por cada pasada (valor arbitrario).
   $limit = 100;
   // Traer sólo los nodos mayores al nodo actual.
   $result = db_select('node', 'n')
     ->fields('n', array('nid'))
     ->orderBy('n.nid', 'ASC')
     ->condition('n.nid', $sandbox['current_node'], '>')
     ->condition('n.uid', 0)
     ->extend('PagerDefault')
     ->limit($limit)
     ->execute();

   foreach ($result as $row) {
     $nid = $row->nid;
     // Llamamos a una función que realice la acción (o podemos hacerla aquí).
     $return = demo_node_set_uid($nid, 1);
     if ($return) {
       $sandbox['messages'][] = t('Updated author for node @nid: @title', array('@nid' => $nid, '@title' => $return));
     }
     // Actualizar la información de progreso..
     $sandbox['progress']++;
     $sandbox['current_node'] = $nid;
  }
  // Después de esta pasada actualizamos #finished. Si es 1 implica que terminó;
  // sino, actualiza el progreso.
  $sandbox['#finished'] = ($sandbox['progress'] >= $sandbox['max']) ? TRUE : ($sandbox['progress'] / $sandbox['max']);

  // Si ya terminó.
  if ($sandbox['#finished']) {
    if (!count($sandbox['messages'])) {
      $sandbox['messages'][] = t('No messages');
    }
    // Crear el mensaje final que vamos a imprimir al usuario.
    $final_message = '<ul><li>' . implode('</li><li>', $sandbox['messages']) . '</li></ul>';
    return t('Finished updating node authors. Messages: @message', array('@message' => $final_message));
  }
 }
```
Como podemos ver; aunque es un poco más complejo que el ```hook_update_N``` normal; en realidad sigue siendo sencillo y útil para procesos que podrían tomar mucho tiempo y que posiblemente alcanzarían los timeouts de php.
