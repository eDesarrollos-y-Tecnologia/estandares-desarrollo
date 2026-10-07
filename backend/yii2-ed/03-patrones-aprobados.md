# Patrones aprobados

## Cuándo usarlo

Antes de crear una abstracción reutilizable dentro de `yii2-ed`.

## Reglas

- Los controladores REST base centralizan formatos, paginación y respuesta.
- `ModeloBase` contiene comportamiento ActiveRecord transversal.
- Los formateadores representan una misma respuesta en varios formatos.
- Repository, Service, Factory, DTO y un contenedor propio no están aprobados.

## Plantilla

Configuración real de `yii2-ed/src/rest/AuthController.php`:

~~~php
public function behaviors() {
  $behavior = parent::behaviors();
  $behavior['authenticator']['authMethods'] = [
    QueryParamAuth::class,
    HttpBearerAuth::class
  ];
  return $behavior;
}
~~~

## Evitar

- Agregar patrones por preferencia personal.
- Incluir dominio de aplicación en una clase compartida.
- Cambiar un contrato sin identificar consumidores.

## Referencias

- `yii2-ed/src/rest/JsonController.php`
- `yii2-ed/src/rest/AuthController.php`
- `yii2-ed/src/models/ModeloBase.php`

