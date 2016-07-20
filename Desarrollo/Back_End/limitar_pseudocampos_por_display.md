# Limitar pseudocampos por display

ADVERTENCIA: WORK IN PROGRESS

En hook_<entity>_view:

$view_mode_extra_fields = field_extra_fields_get_display('user', 'user', $view_mode);
// Only proceed if the profile comments field is visible in the view mode.
if (isset($view_mode_extra_fields['my_extra_field'])
  && $view_mode_extra_fields['my_extra_field']['visible']) {
  // Build it.
}


