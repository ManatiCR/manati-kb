
# Iniciando un proyecto con MDSK

MDSK son las siglas para Manati Drupal Starter Kit; es un generador de Yeoman para arrancar sitios utilizando aquifer, drupalvm y una serie de scripts custom. La página del proyecto en npm es https://www.npmjs.com/package/generator-mdsk y el proyecto en sí está hosteado en Github en https://github.com/manaticr/generator-mdsk (se aceptan Pull Requests).

Una vez instalado el generador de acuerdo con lo indicado en la página del proyecto; procedemos a utilizar las siguientes instrucciones para crear nuestro nuevo sitio usando Pantheon, Github y Wercker. En las instrucciones usamos "Manati Site" como nombre; sustituir según corresponda.

* Crear sitio en pantheon.
* Agregar a ci@estudiomanati.com al team del sitio.
* Crear carpeta en local con el machine name del sitio (usando \_ preferiblemente)
* Generar la estructura: `yo mdsk` (preferiblemente hacer coincidir el nombre máquina con el de la carpeta)
* Crear repositorio local y hacer commit inicial
* Agregar el remote de pantheon en el archivo aquifer.json
* Mover pantheon a modo git
* Crear repositorio en Github (público o privado dependiendo del proyecto)
* Agregar el remote de Github al repositorio local y hacer push
* Crear App de Wercker (en la organización Manati)
* EN SSH Keys, crear nuevo par de llaves llamado "deployment_key"
* Agregar el public key a la cuenta de pantheon de ci@estudiomanati.com
* Crear nuevo Deploy Target con los siguientes settings
  * Auto deploy en branch (develop o master según corresponda)
  * Crear variable de tipo SSH Key llamada DEPLOYMENT_KEY y agregar el key antes creado.
  * Crear variable de tipo texto llamada PANTHEON_TOKEN y agregar un token de pantheon generado en la cuenta de ci@estudiomanati.com. Que esta variable sea protegida.
  * Guardar el deploy target
* Desde la página del app de Wercker lanzar un build; debería pasar.
* Provisionar la máquina virtual (seguir instrucciones en README.md del proyecto generado)
* Construir e instalar el sitio