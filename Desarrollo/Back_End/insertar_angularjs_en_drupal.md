# Insertar AngularJS en Drupal

Hace poco me encontré con el reto de buscar una forma elegante de introducir Angular en un bloque de Drupal; pero además de eso, en Angular debía acceder a unas variables de Drupal (Drupal.settings). ¿Cómo resolver esto? Justamente de eso tratará este post.

Empecemos desde el lado php; aquí necesitamos implementar hook_block_info y hook_block_view para crear un bloque. Según la documentación de hook_block_view, debemos devolver un arreglo con subject (título del bloque) y content. Este índice content puede ser markup o un arreglo rendereable; siendo esta segunda la opción recomendada; tomémosla.

```
/**
 * Implements hook_block_view().
 */
function mimodulo_block_view($delta) {
  $path = drupal_get_path('module', 'mimodulo');
  $block = array();
  switch($delta) {
    case 'mi_bloque':
      $block = array(
        'subject' => t('Título de mi bloque'),
        'content' => array(
          '#markup' => theme('mimodulo_mitema'),
          // Usamos #attached para agregar js porque esta es la forma recomendada en lugar de drupal_add_js().
          '#attached' => array(
            'js' => array(
              // $settings es un arreglo que contiene los valores que queremos enviar hacia javascript.
              0 => array(
                'data' => array('mimodulo' => $settings),
                'type' => 'setting',
              ),
              $path . '/ruta/a/mis/dependencias.js' => array(
                'type' => 'file',
              ),
              // Aquí otros archivos de dependencias.

              // Este es el archivo principal de nuestra aplicación en angular.
              $path . '/js/app/app.module.js' => array(
                'type' => 'file',
              ),
              // Este es el controlador main de nuestra aplicación en angular.
              $path . '/js/app/main/main.controller.js' => array(
                'type' => 'file',
              ),
              // Este es el un factory que vamos a usar para contener los datos provenientes de Drupal..
              $path . '/js/components/drupalData/drupalData.service.js' => array(
                'type' => 'file',
              ),
            ),
          ),
         ),
      );
  }
  return $block;
}
```

No agrego el hook_block_info() y algunos detalles pequeños para no alargar más el post. Empecemos ahora con la parte divertida de esta entrada (javascript).

Es importante recordar que cuando pasamos variables de php a javascript (a través de settings); esta se guarda en la propiedad settings del objeto global Drupal; sin embargo, necesitamos asegurarnos de que este objeto está listo antes de poder accederlo; y para hacerlo, tenemos que envolver nuestro código en Drupal.behaviors. Los behaviors de Drupal se ejecutan en cada bootstrap del sistema; por ello hay que tener ciertas consideraciones cuando los utilicemos; les recomiendo [este post](https://www.lullabot.com/articles/understanding-javascript-behaviors-in-drupal) al respecto. Es por eso, que no queremos que nuestro código de AngularJS se ejecute dentro de los behaviors de Drupal; para ello la arquitectura de nuestro javascript será algo como lo siguiente: - Creamos el módulo de AngularJS desde el primer momento. - Dentro de Drupal Behaviors (y dentro de jquery once) capturamos los valores que necesitamos. - En el evento dom ready tomamos el controlador de angular y de este tomamos un service. - Invocamos una función de este service que a su vez emite un evento de angular. - Este evento lo escuchamos y manejamos en el controlador para preparar todo lo que necesitamos para nuestro template. - Aplicamos los cambios en el scope del controlador.

Ahora sí, manos a la obra.

##app.module.js

```
(function () {
  'use strict';

  // Creamos el módulo de AngularJS.
  angular.module('mimoduloAngular', ['ngCookies', 'ngResource', 'ngSanitize', 'ngTouch']);
  Drupal.behaviors.mimodulo = {
    attach: function(context) {
      // Nos aseguramos de que sólo se ejecute una vez.
      jQuery('#mimoduloId', context).once('mimodulo', mimoduloOnceFunction);
      function mimoduloOnceFunction() {
        var setting1 = Drupal.settings.mimodulo.setting1;
        var setting2 = Drupal.settings.mimodulo.setting2;
        var data = {
          setting1: setting1,
          setting2: setting2
        };

        // We need to ensure dom is ready before getting this element.
        angular.element(document).ready(setDrupalData);
        function setDrupalData() {
          // mainController es el ID del elemento en el template que tiene la directiva data-ng-controller
          var mainControllerElement = angular.element(document.getElementById('mainController'));
          // A través del injector obtenemos el service donde vamos a enviar los datos.
          var drupalDataService = mainControllerElement.injector().get('drupalDataService');
          drupalDataService.setDrupalData(data);
          // Una vez hecho todo necesitamos aplicar el scope para que funcione.
          mainControllerElement.scope().$apply();
        }
      }
    }
  };
})();
```

##drupalData.service.js
En este archivo vamos a crear un service para nuestra aplicación de angular que reciba los datos provenientes del drupal, los guarde en una variable privada, emita un evento de angular y exponga una función para devolver el valor de la variable privada.

```
(function () {
  'use strict';

  angular.module('mimoduloAngular').factory('drupalDataService', drupalDataService);

  // Necesitamos $rootScope para emitir el evento desde aquí y que sea accesible desde cualquier controller.
  function drupalDataService($rootScope) {
    var data = {};

    function setDrupalData(newData) {
      // Aquí manejamos los datos a nuestro antojo y luego los guardamos en la variable data.
      data = newData;
      // Emitimos un evento para indicar que los datos provenientes de Drupal ya están listos.
      $rootScope.$emit('drupalDataReady');
    }

    function getDrupalData() {
      return data;
    }

    // Exponemos ambas funciones públicamenete para nuestro factory.
    return {
      setDrupalData: setDrupalData,
      getDrupalData: getDrupalData
    };
  }
})();
```
##main.controller.js
En este archivo vamos a crear un controller que se encargue de preparar los datos necesarios para nuestro template. En dicho controlador, vamos a escuchar el evento, obtener los datos del service, hacer ‘unbind’ al evento para mejorar el rendimiento y luego procesar todo lo que necesitemos.
```
(function () {
  'use strict';

  angular.module('mimoduloAngular').controller('mainController', mainController);

  function mainController($rootScope, drupalDataService) {
    /* jshint validthis: true */
    var main = this;

    // Escuchemos el evento drupalDataReady para reaccionar a él.
    $rootScope.$on('drupalDataReady', function() {

      // Unbind the event.
      var mainDiv = angular.element(document.getElementById('mainController'));
      angular.element(mainDiv).unbind('drupalDataReady');

      var data = drupalDataService.getDrupalData();
      var setting1 = data.setting1;
      var setting2 = data.setting2;

      // Aquí hacemos el procesamiento necesario para que el template funcione correctamente.

    });
  }
})();
```
Con eso, tenemos la estructura deseada para nuestra aplicación de angular; solamente del lado de Drupal nos falta implementar el``` hook_theme(```) que es invocado en el ```hook_block_view()```. Dicho ```hook_theme``` puede simplemente hacer render a un template ubicado dentro de nuestro módulo.

De esta forma, podemos incrustar angular dentro de un bloque de Drupal de la manera correcta y evitando tener nuestro código de AngularJS dentro de los Drupal Behaviors para evitar comportamientos inesperados que suelen ocurrir en este contexto.

Este post surgió como parte de un proyecto que estamos desarrollando en [Manatí](http://www.estudiomanati.com/) en el que estamos desarrollando un módulo que esperamos poder contribuir con la comunidad: [apachesolr_angularjs_search](https://github.com/ManatiCR/apachesolr_angularjs_search). Aún está en una etapa muy temprana de desarrollo, pero apenas esté listo lo haremos saber.

Espero que esta entrada les haya sido útil.
