# Convenciones de arquitectura

## Cuándo usarlo

Al crear un modelo, endpoint, módulo, comando o migración en `yii2-app-ed`.

## Reglas

- `config/` define componentes; `modelos/` contiene persistencia, relaciones y
  validación; `modulos/v1/` expone la API.
- El controlador coordina HTTP. Las reglas persistentes viven en el modelo.
- Cada caso de uso pertenece al modelo principal que representa el proceso y
  expone un método con nombre de negocio completo, como `crearConDetalles()` o
  `confirmar()`. Ese método puede coordinar otros modelos cuando sea necesario.
- Extender `AuthController` para rutas protegidas y `JsonController` para rutas
  públicas.
- Cuando un caso de uso modifica varias entidades, valida primero y guarda todo
  dentro de una transacción; ante un error revierte la operación completa.
- Antes de una migración confirmar el motor y el estado real de la base. Las
  entidades propias usan nombres en español, tablas singulares y campos
  `camelCase`, salvo integración externa. Las auditorías son `creado`,
  `modificado` y `eliminado`.
- En PostgreSQL usar `defaultExpression('CURRENT_TIMESTAMP')`, no sintaxis de
  MySQL como `ON UPDATE CURRENT_TIMESTAMP`.
- Una migración debe poder aplicarse y revertirse con seguridad; no transformar
  ni eliminar datos sin autorización y respaldo verificable.
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

Caso de uso en el modelo principal:

~~~php
public function confirmar(): void {
  $transaccion = Yii::$app->db->beginTransaction();
  try {
    // Validar y guardar los cambios coordinados del proceso.
    $transaccion->commit();
  } catch (\Throwable $e) {
    $transaccion->rollBack();
    throw $e;
  }
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

  public function safeDown() {
    $this->dropTable('Producto');
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
