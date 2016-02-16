# Actualización masiva de nodos en un update de base de datos usando el batch API

Necesitaba actualizar el título de aproximadamente 3000 nodos de un sitio en producción; claramente esta es una operación muy costosa en cuanto a recursos y tiempo; entonces; la mejor solución que encontré es utilizar un ```hook_update_N``` que haga uso del batch API; de esta forma; podemos realizar la operación desde drush y utilizando procesamiento por batches.

A continuación código de ejemplo:

```

function mimodulo_update_7001(&$sandbox) {
  if (!isset($sandbox['progress'])) {
    $sandbox['progress'] = 0;
    // Total nodes.
    $sandbox['max'] = db_select('node', 'n')
      ->fields('n', array('nid'))
      ->countQuery()
      ->execute()
      ->fetchField();
    // A place for storing messages;
    $sandbox['messages'] = array();
    $sandbox['current_node'] = -1;
  }

  // Process nodes by groups of 100 (arbitrary value).
  $limit = 100;

  $result = db_select('node', 'n')
    ->fields('n', array('nid'))
    ->orderBy('n.nid', 'ASC')
    ->condition('n.nid', $sandbox['current_node'], '>')
    ->extend('PagerDefault')
    ->limit($limit)
    ->execute();

  foreach ($result as $row) {
    $nid = $row->nid;
    // Procesar aquí cada nodo de forma individual
    $sandbox['messages'][] = t('Mensaje para ese nodo si fuera necesario');

    // Update progress information.
    $sandbox['progress']++;
    $sandbox['current_node'] = $nid;
  }

  // Set "finished" status. If it's float; this will indicate the progress
  // of the batch so the progress bar will update.
  $sandbox['#finished'] = ($sandbox['progress'] >= $sandbox['max']) ? TRUE : ($sandbox['progress'] / $sandbox['max']);

  if ($sandbox['#finished']) {

    if (!count($sandbox['messages'])) {
      $sandbox['messages'][] = t('No messages');
    }

    $final_message = '<ul><li>' . implode('</li><li>', $sandbox['messages']) . '</li></ul>';
    return t('Finished updating node titles. Messages: @message', array('@message' => $final_message));
  }

}
```