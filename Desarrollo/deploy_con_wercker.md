# Continous Delivery con Wercker

Asumiendo que todo se configuró de acuerdo con la guía de [inicio de proyecto con MDSK](https://manati.gitbooks.io/manati-kb/content/Desarrollo/iniciando_un_proyecto_con_mdsk.html), el proceso de deploy/CD con Wercker quedaría de la siguiente forma:

## Deploy to Pantheon Dev Environment

There is an automatic deploy set for green builds in the develop branch. If it fails, you could retry it from Wercker UI. Remember that the development environment gets always REINSTALLED on every deploy, so it shouldn't have persistent data.

## Deploy to Pantheon Test Environment

Once we have a green Pantheon Dev build, you could trigger a Test build; this will update the Test environment with the latest develop code, WITHOUT reinstalling. This environment is ideal to show new stuff to the client.

## Deploy to Pantheon Live Environment

Once we have a green Pantheon Test build, you could trigger a Live build; this will update the Live environment with the latest develop code, WITHOUT reinstalling. This is the environment that will have the production site.

## Deploy to Pantheon Multidev Environment

Once we have a green build for any branch, you could trigger a multidev deploy to push that code to a new multidev environment. For example, if your branch is named feature/RP-35-pipelines; the new environment name will be rp35.

