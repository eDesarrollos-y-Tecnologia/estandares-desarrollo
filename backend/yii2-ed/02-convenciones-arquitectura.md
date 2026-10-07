# Convenciones de arquitectura

## Cuándo usarlo

Al extender REST, autenticación, respuesta, serialización, modelos base,
formatos, correo, cron u OpenAPI de `yii2-ed`.

## Reglas

- El núcleo contiene infraestructura útil para dos o más aplicaciones.
- Respuesta, rutas, serialización y clases base son contratos públicos.
- Dominio, migraciones, permisos y configuración de cliente permanecen en su
  aplicación.
- Un cambio incompatible requiere versión mayor y guía de migración.

## Plantilla

Fragmento real de `yii2-ed/src/rest/AuthController.php`:

~~~php
public function beforeAction($action) {
  parent::beforeAction($action);

  $this->usuario = \Yii::$app->getUser()->getIdentity();

  if ($this->usuario === null) {
    throw new \yii\web\UnauthorizedHttpException(
      'No está autorizado para realizar esta acción.'
    );
  }

  return true;
}
~~~

## Evitar

- Copiar autenticación Bearer dentro de acciones individuales.
- Resolver una necesidad de un cliente cambiando la librería.
- Publicar una dependencia interna como contrato sin necesidad.

## Referencias

- `yii2-ed/src/rest/AuthController.php`
- `yii2-ed/src/data/Respuesta.php`
- [04-desarrollo-funcionalidades.md](./04-desarrollo-funcionalidades.md)

