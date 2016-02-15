
# Procedimiento de actualización del core de drupal 7



## Preparación previa


* Avisar que se va actualizar el sitio para que no se trabaje sobre el mismo durante el proceso y así preservar la consistencia.
* Se hace pull del ambiente de dev y por seguridad se descartan o se hace commit de los cambios locales (según sea necesario). Esto para todos los colaboradores.
* Realizar backup del ambiente de  Dev
* Descargar los files y BD de live hacia el ambiente local.
* Clonar el ambiente de Live hacia Dev
* En caso de que haya un Solr; restablecer el Solr en el ambiente Dev. (Postear schema.xml y reindexar los índices)

## Actualización del Core
* Leer changelog de las actualizaciones por realizar (core) para dimensionar el tamaño e implicaciones de la actualización.
* Se verifican módulos parchados [Importante definir procedimiento de "parchado" de módulos: dónde buscar los parches?, qué se hace con los módulos parchados? (dejarlos en core o moverlos?); cualquier otro aspecto importante al respecto].
* Es importante tener en cuenta esto; ya que luego deberán re-aplicarse estos parches en caso de que el motivo de dicho parche no haya sido solucionado en el módulo actualizado.
* Se aplica la actualización desde el dashboard de Pantheon marcando siempre la casilla de correr el update.php y en la medida de lo posible la de "Auto-resolve conflicts" (Tener cuidado con esta en caso de parches en el core).
* Se hace ```git pull``` y se corre el update.php en local.
* En caso de que hayan módulos parchados del core, se procede a re-aplicar los parche (Siempre revisar el Changelog del módulo, con el fin de saber si el parche por aplicar aún tiene validez). En caso de tener que re-aplicarlo; hacerlo y luego subir los cambios.
* Se hacen pruebas críticas sobre el ambiente local y dev:
  * En caso de problemas; revertir los cambios.
* Realizar backup del ambiente dev para tener respaldo de una versión estable del mismo con el Core actualizado.

#Actualización de módulos Contrib 
(Sólo actualizaciones de seguridad) (Post-actualización del core)

* Se procede  a revisar la lista de los módulos actuales esto desde el drupal y verificamos si existen módulos con actualizaciones de seguridad pendientes, si los hay, se procederá con la actualización inmediata únicamente de estos:
* Leer changelog de las actualizaciones por realizar (módulos de contrib) para dimensionar el tamaño e implicaciones de la actualización.
* Se verifican módulos parchados [Importante definir procedimiento de "parchado" de módulos: dónde buscar los parches?, qué se hace con los módulos parchados? (dejarlos en contrib o moverlos?); cualquier otro aspecto importante al respecto].
* Es importante tener en cuenta esto; ya que luego deberán re-aplicarse estos parches en caso de que el motivo de dicho parche no haya sido solucionado en el módulo actualizado.
* "drush dl" para cada uno de los módulos por actualizar.
* Correr el update.php (drush updatedb) para aplicar las actualizaciones de BD que sean necesarias para los módulos recientemente actualizados.
* Se hacen pruebas críticas sobre el ambiente local:
  1. Si hay problemas con esto; tratar de resolverlos o en última instancia; revertir los cambios locales.
  2. Si todo funciona bien; subir los cambios.
* Se hacen pruebas críticas sobre el ambiente dev:
  1. Si hay problemas con esto; tratar de resolverlos o en última instancia; revertir los cambios.


#Procedimiento general post-actualización en Dev
* Realizar un backup de Test
* Pull de dev hacia Test (importando files y bd desde Live)
* En caso de que haya un Solr; restablecer el Solr en el ambiente Dev. (Postear schema.xml y reindexar los índices)
* Realizar pruebas críticas en Test
* Pedir al cliente que revise el ambiente de Test
Una vez aprobado o realizadas las correcciones correspondientes:
* Backup de Live
* Hacer pull hacia live (y realizar update.php)
* Al finalizar el proceso de actualización y recibida la confirmación del cliente de que todo esta correcto y se realizó el pase el ambiente de live se  notifica a todo el equipo del proyecto que el proceso de actualización ha finalizado y que realicen "git pull" para actualizar su repositorio local.


#Revertir los cambios locales
En caso de necesitar revertir los cambios locales; si aún no ha hecho commit; sólamente debe hacer un ```git checkout -- nombreArchivo ```para cada archivo o carpeta que desea revertir.

Además; debe re-importar una base de datos consistente con el estado actual de los módulos que revirtió.

#Revertir los cambios al ambiente Dev

## Si se dispone de backup
Restaurar el backup
Desde el dashboard obtener el identificador del último commit
```git reset --hard identificadorCommit```

Si no se dispone de backup
Con ```git log``` buscar hasta que commit se quiere restaurar y copiar los primeros caracteres del identificador.

```git reset --hard identificadorCommit```
```git push -f origin master```

Restablecer una base de datos que no tenga los recientes cambios de base de datos (podría ser la de live)



