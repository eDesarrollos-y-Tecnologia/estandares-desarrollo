# Modelo

## Cuándo usarlo

Al crear una entidad persistente en `modelos/<Nombre>.php`.

## Reglas

- Extender `ModeloBase` para conservar UUID, reportes y comportamiento compartido.
- Definir `tableName()`, `rules()`, `attributeLabels()`, `fields()` y
  `extraFields()` según corresponda.
- Declarar relaciones con `hasOne()` o `hasMany()` en el modelo.
- Mantener validación, relaciones y comportamiento persistente dentro del
  modelo; el controlador no los duplica.

## Plantilla

Ejemplo completo basado en `yii2-app-ed/modelos/Modulo.php`:

~~~php
<?php

namespace app\modelos;

/**
 * Modelo de la tabla "Modulo".
 *
 * @property string $id
 * @property string|null $nombre
 * @property string|null $creado
 * @property string|null $modificado
 * @property string|null $eliminado
 *
 * @property Permiso[] $permisos
 */
class Modulo extends ModeloBase {

  public static function tableName() {
    return 'Modulo';
  }

  public function rules() {
    return [
      [['id'], 'required'],
      [['creado', 'modificado', 'eliminado'], 'safe'],
      [['id'], 'string', 'max' => 50],
      [['nombre'], 'string', 'max' => 200],
      [['id'], 'unique'],
    ];
  }

  public function attributeLabels() {
    return [
      'id' => 'ID',
      'nombre' => 'Nombre',
      'creado' => 'Creado',
      'modificado' => 'Modificado',
      'eliminado' => 'Eliminado',
    ];
  }

  public function fields() {
    return [
      'id',
      'nombre',
      'creado',
      'modificado',
      'eliminado',
    ];
  }

  public function extraFields() {
    return [
      'permisos',
    ];
  }

  public function getPermisos() {
    return $this->hasMany(Permiso::class, ['idModulo' => 'id'])
      ->where(['eliminado' => null]);
  }
}
~~~

## Evitar

- Crear un modelo sin `rules()`.
- Exponer una relación en `fields()` cuando debe solicitarse mediante
  `extraFields()`.
- Guardar SQL, autorización HTTP o lectura de encabezados en el modelo.

## Referencias

- `yii2-app-ed/modelos/Modulo.php`
- `yii2-app-ed/modelos/ModeloBase.php`
- [01-convenciones-codigo.md](./01-convenciones-codigo.md)

