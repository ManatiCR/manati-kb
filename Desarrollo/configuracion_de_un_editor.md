# Configuración de un editor

Cada desarrollador es libre de usar el editor de su preferencia; solamente se solicita configurarlo con algunos elementos necesarios para mejorar la calidad del código producido.
Dichos elementos son:
- Linter (php, drupalcs y eslint)
- [Editorconfig](http://editorconfig.org)

En esta guía no pretendemos cubrir todas las configuraciones necesarias para cada editor, más bien, se pretende aportar links para guías de configuración de algunos editores:

* [Vim](https://www.drupal.org/node/29325)
* [Sublime Text](https://www.drupal.org/node/1346890)
* Atom
* VSCode



### Configuración de DrupalCS

* Asegúrese de tener [composer instalado](https://getcomposer.org/download).
* Instale coder.
  * ```composer global require drupal/coder:\>8```
  * ```cp -r ~/.composer/vendor/drupal/coder/coder_sniffer/Drupal* ~/.composer/vendor/squizlabs/php_codesniffer/CodeSniffer/Standards/```
 * Instale el plugin respectivo para su editor de texto.




### Configuración de EsLint

* Asegúrese de tener npm instalado (ojalá con nvm).
* Instale eslint global:
  * ```npm install -g eslint```
* Instale el plugin respectivo para su editor de texto.




