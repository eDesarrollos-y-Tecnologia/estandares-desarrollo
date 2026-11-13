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

## Regla: definición de permisos

Los permisos son constantes del dominio de la aplicación. No se escriben como
literales en controladores, modelos, migraciones ni frontend.

- Crear un módulo `MENU` para permisos de navegación. Sus claves comienzan con
  `MENU_`, por ejemplo `MENU_ADMINISTRACION` y
  `MENU_ADMINISTRACION_USUARIO`.
- Crear un módulo por recurso de negocio, por ejemplo `USUARIO`, `PERMISO` o
  `GRUPO`.
- Definir un permiso por acción del recurso con el formato
  `<RECURSO>_<ACCION>`: `USUARIO_CREAR`, `USUARIO_EDITAR` y
  `USUARIO_ELIMINAR`.
- Declarar las mismas claves en constantes PHP y TypeScript. La migración
  inserta los módulos y permisos iniciales usando las constantes PHP.
- Un usuario obtiene permisos efectivos por unión de sus permisos directos y
  los concedidos por todos sus grupos. Los grupos solo conceden permisos; no
  los niegan.

Plantilla de constantes:

~~~php
class Modulo extends ModeloBase {
  public const MENU = 'MENU';
  public const USUARIO = 'USUARIO';
}

class Permiso extends ModeloBase {
  public const MENU_ADMINISTRACION_USUARIO = 'MENU_ADMINISTRACION_USUARIO';
  public const USUARIO_CREAR = 'USUARIO_CREAR';
  public const USUARIO_EDITAR = 'USUARIO_EDITAR';
  public const USUARIO_ELIMINAR = 'USUARIO_ELIMINAR';
}
~~~

~~~ts
Permiso.MENU.ADMINISTRACION_USUARIO
Permiso.USUARIO.CREAR
Permiso.USUARIO.EDITAR
Permiso.USUARIO.ELIMINAR
~~~

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
