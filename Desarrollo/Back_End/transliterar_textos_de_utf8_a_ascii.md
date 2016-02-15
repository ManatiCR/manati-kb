# Transliterar textos de utf8 a ascii

Cuando estamos en el proceso de desarrollo de un sitio que posee funcionalidad custom normalmente se nos presentan retos interesantes, uno de esos es transliterar texto, la transliteración es básicamente pasar un texto codificado en el A al formato B.

Por ejemplo si necesitamos eliminar acentos de las palabras de un string dado para poder usar estás palabras para formar un link o crear un nombre maquina de algún elemento del sistema, podemos usar el siguiente código para "limpiar" nuestros strings pasandolos de una codificación UTF-8 a ASCII.


Acá un ejemplo de como realizarlo en drupal 7.

```
setlocale(LC_ALL,'es_US.utf8');
$trans_sentence = iconv('UTF-8', 'ASCII//TRANSLIT', 'garcía');
```

Información de sobre función ```setlocale()``` :

http://php.net/manual/en/function.setlocale.php


Información sobre función ```iconv()``` :

http://php.net/manual/es/function.iconv.php


