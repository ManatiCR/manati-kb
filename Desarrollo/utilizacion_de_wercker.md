# Utilización de Wercker

En un intento por mejorar nuestros flujos de trabajo, hemos adoptado la plataforma Wercker como herramienta de CI. En ella, corremos nuestras pruebas de estándares de código y en caso de haberlos, nuestros tests de behat. Además, hemos habilitado para realizar deploys a Pantheon desde la misma herramienta. El proceso de configuración de la misma lo podemos encontrar documentado en el artículo [Iniciando un proyecto con MDSK](Desarrollo/iniciando_un_proyecto_con_mdsk.md) y en el presente artículo nos centraremos únicamente en el uso de la herramienta.

Para utilizar dicha herramienta, se necesita que cada desarrollador del proyecto cree una cuenta en http://wercker.com/ y que sea agregado al proyecto por parte de uno de los administradores de la organización Manati.


Una vez hecho lo anterior, se tendrá acceso al app de wercker desde dónde se podrán ver todos los builds e iniciar nuevos builds.

Es importante recordar que un build es iniciado automáticamente con un push en cualquier rama. Además, un push en la rama principal del proyecto hace trigger de un deploy al ambiente dev de pantheon (en algunos casos esto implica reinstalar el sitio, por lo que este ambiente de dev no debe nunca contener información que se espere que perdure).

Desde un build en cualquier rama se puede hacer un `deploy multidev` para crear un nuevo ambiente de multidev en pantheon.

Un deploy (o deploy-dev) puede usarse como punto de inicio para lanzar un deploy-test; así como deploy-test puede usarse para lanzar un deploy-live.

En algunos sitios tenemos configurado una acción llamada GoLive; esta se debe lanzar desde un deploy (o deploy-dev) y lo que hace es que realiza un deploy-test e inmediatamente (en caso de éxito) realiza un deploy live. 