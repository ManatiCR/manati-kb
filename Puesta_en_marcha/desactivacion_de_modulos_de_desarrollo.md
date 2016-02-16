# Desactivación de modulos de Desarrollo

 Siempre que se está en el proceso de desarrollo de un sitio en drupal se utilizan ciertos módulos o configuraciones en el sitio para agilizar y facilitar el desarrollo de nuestros sitios, pero estos módulos muy a menudo ya sea por descuido o falta de tiempo pasan a ambientes de producción, lo cual claramente no es correcto ya que consumen memoria de manera innecesaria en nuestro sitio ya que no se van a utilizar más  (al menos en el ambiente de producción).
 
 Ejemplos claros de estos son views_ui, context_ui, devel, entre muchos otros, el procedimiento adecuado es desactivar estos módulos, a continuación describiremos diversas formas de como desactivar estos módulos para tener sitios con mejor performance y mejores practicas de desarrollo.
 
 
 Como parte de las nuevas políticas de desarrollo de sitios de manatí (establecidas en 2016 por acuerdo mutuo de la gerencia y desarrolladores) se establece como prioridad tener la configuración de ambientes(dev, test, live) en código, para la parte de módulos se usará el modulo **Master** para drupal 7, el cual se encargará de llevar el control de que módulos están activos por ambiente, acá podrá encontrar más información [Como utilizar el módulo Master](Desarrollo/Back_End/como_utilizar_master_modulo_drupal_7.md).
 
 
## Desactivación de modulos con Master.


En nuestro ambiente live y suponiendo que se ha seguido la guía de utilización de Master y se ha hecho una correcta implementación del mismo procedemos a ejecutar el siguiente comando de drush en nuestro sitio:

```drush master-set-current-scope [NOMBRE AMBIENTE LIVE]```

Con la ejecución de este comando nos aseguramos de que en live solo van a estar los módulos especificados en la configuración que definimos en Master.


## Desactivación Manual.

En caso de no tener Master en nuestro sitio se debe hacer una desactivación módulo por módulo, la recomendación en este caso es levantar un listado con todos los módulos innecesarios para luego desactivarlos uno por uno.

## Que módulos son innecesarios? 
Son todos aquellos que el usuario no va utilizar, normalmente todos los módulos de UI son desactivados, otro ejemplo claro es devel que solo provee funciones y utilitarios para el desarrollo.

## Como deactivar?

**Con drush:**

Primero desactivamos el módulo o módulos (recordemos que podemos desactivar varios módulos con una sola ejecución del comando).

```drush dis nombre_modulo -y```

Posteriormente realizamos la desinstalción del módulo o los módulos, al igual que en el caso de ```drush dis``` podemos desactivar varios módulos con solo una ejecución del comando.

```drush pm-uninstall nombre_modulo -y```


**Desde listado de módulos:**
 
 Básicamente desde la interfaz de módulos de drupal buscamos por nombre el módulo a desactivar, lo desactivamos y una vez desactivado vamos a la pestaña de desinstalar(Unistall) , buscamos nuestro módulo y lo desactivamos.





