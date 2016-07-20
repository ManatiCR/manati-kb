# GIT

https://kalamuna.atlassian.net/wiki/display/KALA/Creating+Pantheon+Sites+and+Choosing+an+Upstream

http://try.github.io/


## Flujo de trabajo de git


Branching

Repositories associated with the distribution will be using a basic Gitflow workflow.

* Production branch: master - merge into
* Development branch: develop - branch off
* Feature branch prefix: feature/ - branch from develop
* Release branch prefix: release/ - fork from develop
* Hotfix branch prefix: hotfix/ - fork from master


Branch names should start with the JIRA Story identifier, followed by a dash, then a short description.

Branches should be deleted locally and remotely once merged.
Commits

Commit early, commit often, and push often! Keep your commits atomic as possible.
Commit messages

Commit messages should start with the JIRA Story identifier, followed by a colon, then a brief description of what occurred.


Una vez que está lista la funcionalidad en el feature branch, se debe hacer push y crear un Pull Request hacia la rama principal (develop). A este PR se le asignarán los labels "Awaiting Code Review" y "Awaiting Functional Review" (según corresponda) y se escribirán pasos detallados para probar la funcionalidad.

El equipo de desarrollo es responsable de revisar los PRs de los demás miembros del equipo; para ello, realiza el code review desde la interfaz de Github, si todo está bien, cambia el label de "Awaiting Code Review" por "Passes Code Review"; en caso contrario, lo cambiará por "Needs Work".

Si se requiere, ejecutará los pasos en su ambiente local para realizar el review funcional, y de igual forma cambiará el label "Awaiting functional review" por "Passes functional review" o "Needs Work" según corresponda.

Si el Pull Request tiene los dos labels ("Passes Code Review" y "Passes Functional Review") y los tests ya pasaron (en caso de que existan) y no hay ningún motivo para evitar el merge, se debe además agregar el label "Ready to Merge" y el creador es responsable de realizar el merge y eliminar la rama en Github.