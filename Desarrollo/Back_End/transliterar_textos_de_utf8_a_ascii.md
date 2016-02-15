# Transliterar textos de utf8 a ascii

Cuando estamos en el proceso de desarrollo de un sitio que posee funcionalidad custom normalmente se nos presentan retos interesantes, uno de esos es transliterar texto, la transliteración es básicamente pasar un texto codificado en el A al formato B. 

Acá un ejemplo de como realizarlo en drupal 7.

```
setlocale(LC_ALL,'es_US.utf8');
$trans_sentence = iconv('UTF-8', 'ASCII//TRANSLIT', 'garcía');
```

Información de sobre función ```setlocale()```
http://php.net/manual/en/function.setlocale.php


Información sobre función ```iconv()```
http://php.net/manual/es/function.iconv.php


