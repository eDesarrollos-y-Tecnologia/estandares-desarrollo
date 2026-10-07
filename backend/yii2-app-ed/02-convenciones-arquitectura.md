# Convenciones de arquitectura

## Cuándo usarlo

Al crear un modelo, endpoint, módulo, comando o migración en `yii2-app-ed`.

## Reglas

- `config/` define componentes; `modelos/` contiene persistencia, relaciones y
  validación; `modulos/v1/` expone la API.
- El controlador coordina HTTP. Las reglas persistentes viven en el modelo.
- Extender `AuthController` para rutas protegidas y `JsonController` para rutas
  públicas.
- Una migración preserva datos existentes.
- Lo reutilizable por dos o más aplicaciones se propone para `yii2-ed`.

## Plantilla

Basada en `yii2-app-ed/modulos/v1/controladores/ModuloController.php`:

~~~php
namespace v1\controladores;

use eDesarrollos\rest\AuthController;

class ProductoController extends AuthController {
  public $modelClass = \app\modelos\Producto::class;
}
~~~

Migración basada en `m240718_041517_inicio.php`:

~~~php
class m260101_000000_crear_producto extends \yii\db\Migration {
  public function safeUp() {
    $this->createTable('Producto', [
      'id' => $this->string(50)->notNull(),
      'nombre' => $this->string(100)->notNull(),
    ]);
    $this->addPrimaryKey('ProductoPK', 'Producto', 'id');
  }
}
~~~

## Evitar

- Crear `Service`, `Repository` o DTO sin propuesta aprobada.
- Sobrescribir CRUD base si `modelClass` o `buscador()` bastan.
- Mover dominio de cliente a `yii2-ed`.

## Referencias

- `yii2-app-ed/config/web.php`
- `yii2-app-ed/modulos/v1/Modulo.php`
- `yii2-app-ed/migraciones/m240718_041517_inicio.php`

