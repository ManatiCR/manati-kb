# Exportar Formatos de fecha y su traducción




```
function cs_mail_notifications_date_format_importer($date_format_human_name, $date_format_machine_name, $date_format_original_format, $date_format_locale_format) {
  cs_mail_notifications_date_format_insert($date_format_original_format, $date_format_machine_name);
  db_insert('date_format_type')
    ->fields(array(
      'type' => $date_format_machine_name,
      'title' => $date_format_human_name,
      'locked' => 0,
    ))
    ->execute();

  variable_set('date_format_' . $date_format_machine_name, $date_format_original_format);

  foreach ($date_format_locale_format as $language => $locale_format) {
    cs_mail_notifications_date_format_insert($locale_format, $date_format_machine_name);
    db_insert('date_format_locale') 
      ->fields(array(
        'format' => $locale_format,
        'type' => $date_format_machine_name,
        'language' => $language,
      ))
      ->execute();
  }
}

/**
 * Helper to insert a date format.
 */
function cs_mail_notifications_date_format_insert($date_format, $date_format_machine_name) {
  db_insert('date_formats')
    ->fields(array(
      'format' => $date_format,
      'type' => $date_format_machine_name,
      'locked' => 0,
    ))
    ->execute();
}
```




