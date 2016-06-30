# Metatags

El proceso para implementar metatags en un sitio en drupal 7 es relativamente sencillo.

- Correr: `drush en metatag metatag_opengraph metatag_twitter_cards -y`. 

Dependiendo de cómo se esté haciendo, añada además la configuración necesaria a master y al drupal.make.yml).

- Se configuran los metatags como sea necesario según el sitio. Revisar con las herramientas de facebook y de twitter: 

https://developers.facebook.com/tools/debug
https://cards-dev.twitter.com/validator

Esto también es útil:
http://iframely.com/debug

Nota importante. En los ambientes de desarrollo de pantheon las el robots.txt bloquea todo, esto para asegurarse que ningún crawler agarre sitios que están en producción, por tanto, no existirán twitter cards de todo lo que tenga un '/' después de la url.