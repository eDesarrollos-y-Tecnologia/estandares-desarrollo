# Módulo Yii2

## Cuándo usarlo

Al crear una nueva superficie de rutas, por ejemplo una versión de API o un
grupo de endpoints con namespace propio.

## Reglas

- Cada módulo declara su `controllerNamespace`.
- Registrar el módulo en `config/web.php` con su clase.
- Mantener la misma estructura: `modulos/<nombre>/Modulo.php` y
  `modulos/<nombre>/controladores/`.
- Las rutas públicas y protegidas se definen por el controlador base que extienda
  cada controlador, no por lógica repetida del módulo.

## Plantilla

Ejemplo completo de `yii2-app-ed/modulos/v1/Modulo.php`:

~~~php
<?php

namespace v1;

class Modulo extends \eDesarrollos\base\Modulo {

  public $controllerNamespace = 'v1\\controladores';
}
~~~

Registro correspondiente en `config/web.php`:

~~~php
'modules' => [
  'v1' => ['class' => 'v1\Modulo'],
],
~~~

## Evitar

- Registrar un módulo sin que exista su clase y namespace de controladores.
- Usar un módulo para resolver una sola acción aislada.
- Poner modelos de dominio dentro del directorio de un módulo.

## Referencias

- `yii2-app-ed/modulos/v1/Modulo.php`
- `yii2-app-ed/config/web.php`
- `yii2-ed/src/base/Modulo.php`

