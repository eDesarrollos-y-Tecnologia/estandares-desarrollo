# Patrones aprobados

## Cuándo usarlo

Antes de introducir una estructura adicional para resolver un caso de aplicación.

## Reglas

- Extender infraestructura de `yii2-ed` desde el namespace de la aplicación.
- Usar ActiveRecord para entidades, relaciones y reglas del dominio.
- Colocar un caso de uso que coordina entidades en el modelo principal del
  proceso, no en el controlador ni en la vista.
- Usar módulos Yii para separar rutas de API.
- `Service`, `Repository`, Factory, DTO y eventos no son patrones aprobados.

## Plantilla

Extensión real de `yii2-app-ed/modelos/ModeloBase.php`:

~~~php
namespace app\modelos;

class ModeloBase extends \eDesarrollos\models\ModeloBase {
}
~~~

Módulo basado en `yii2-app-ed/modulos/v1/Modulo.php`:

~~~php
namespace v1;

class Modulo extends \eDesarrollos\base\Modulo {
  public $controllerNamespace = 'v1\\controladores';
}
~~~

Un caso de uso se nombra por el resultado de negocio y no por un paso técnico:

~~~php
public function crearConDetalles(array $detalles): void {
  // El modelo coordina su persistencia y la de sus detalles.
}
~~~

## Evitar

- Crear una capa por moda o por otro framework.
- Editar `vendor/edesarrollos/yii2-ed`.
- Adoptar un patrón generado por IA sin revisión.

## Referencias

- `yii2-app-ed/modelos/ModeloBase.php`
- `yii2-app-ed/modulos/v1/Modulo.php`
- [06-excepciones.md](./06-excepciones.md)
