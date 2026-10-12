# Controlador CRUD

## Cuándo usarlo

Al exponer el CRUD estándar de un modelo protegido por autenticación, sin
acciones particulares fuera de los parámetros de búsqueda.

## Reglas

- Extender `AuthController` para que la autenticación sea centralizada.
- Declarar `modelClass` y siempre implementar `buscador()` como punto de ajuste
  de los parámetros de búsqueda.
- El CRUD base resuelve `actionIndex()`, `actionPost()`, `actionPut()` y
  `actionDelete()`.
- La validación permanece en el modelo y los errores usan `Respuesta`.

## Plantilla

~~~php
<?php

namespace v1\controladores;

use eDesarrollos\rest\AuthController;

class ProductoController extends AuthController {

  public $modelClass = \app\modelos\Producto::class;

  public function buscador(&$query, $request) {
    $id = $request->get($this->modeloID, "");

    if ($id !== "") {
      $query->andWhere([$this->modeloID => $id]);
    }
  }
}
~~~

Con esta clase, las rutas estándar del módulo usan los métodos heredados sin
copiar código CRUD al controlador. Los filtros adicionales se agregan dentro de
`buscador()`.

## Evitar

- Reimplementar `actionPost()`, `actionPut()` o `actionDelete()` sin una
  necesidad de dominio concreta.
- Leer o validar `Authorization` dentro de una acción.
- Añadir una acción especial que sólo cambia filtros; se ajusta `buscador()`.

## Referencias

- `yii2-ed/src/rest/JsonController.php`
- `yii2-ed/src/rest/AuthController.php`
- [09-controlador-personalizado.md](./09-controlador-personalizado.md)
