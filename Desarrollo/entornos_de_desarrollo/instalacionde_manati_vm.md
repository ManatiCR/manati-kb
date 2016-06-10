# Instalación de manati-vm


Manati VM es la máquina virtual utilizada para el desarrollo de proyectos en Manatí. Es Open Source y ud la puede descargar o clonar desde [Github](https://github.com/manaticr/manati-vm)

Para utilizarla, requiere tener instalados los siguientes programas o paquetes en su computadora:

* Virtualbox
* Vagrant (instalado desde la página oficial)
* Ansible (instalado desde la página oficial)
* NFS (instalado por default en Mac; en linux necesita instalarse)

Una vez instalados los requerimientos y clonada el repositorio, procedemos a ejecutar `vagrant up` dentro de la carpeta clonada. Esto iniciará el proceso de creación y provisionamiento de la máquina virtual. Al mismo nivel que la carpeta manati-vm, creará una carpeta www donde usted debe almacenar los docroot de los proyectos en los que va a trabajar. Además de eso, debe realizar una asignación de DNS en el archivo /etc/hosts de su máquina física de la siguiente forma:

Ej. Si en la carpeta www, usted tiene un proyecto llamado ejemplo, debe agregar en el archivo /etc/hosts (debe abrirse con permisos de superusuario) una línea como la siguiente:

`10.10.10.10  ejemplo.local.dev`

Esto le permitirá visualizar el proyecto en su navegador al acceder a `ejemplo.local.dev`. 10.10.10.10 es la dirección IP que tiene la máquina virtual.

Es importante mencionar que de acuerdo con la documentación oficial, NFS no es soportado en particiones encriptadas; por ello, es necesario que la partición donde se va a utilizar no esté encriptada :'( 

Se cuenta con varios scripts en la carpeta deploy para realizar algunas tareas (crear db, importar db, agregar core de solr, etc); para ejecutarlos, solamente debe utilizar el siguiente comando:
`./run-playbook deploy/script.yml`

Para más información, por favor consulte el [README.md](https://github.com/ManatiCR/manati-vm/blob/master/README.md) del proyecto.

Cualquier sugerencia de documentación o mejora del proyecto es bienvenida como Pull Request o issue en el repositorio.
