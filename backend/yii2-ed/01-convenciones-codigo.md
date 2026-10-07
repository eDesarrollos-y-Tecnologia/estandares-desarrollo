# Convenciones de código

## Cuándo usarlo

Al cambiar código público o reutilizable dentro de `yii2-ed/src/`.

## Reglas

- Usar K&R, dos espacios y sin tabuladores.
- Dejar una línea en blanco entre una clase y su primer miembro, y usar llaves
  aun en bloques de una sola instrucción.
- Mantener el namespace `eDesarrollos\\` y la compatibilidad de `composer.json`.
- Tipar parámetros y retornos cuando no rompan firmas heredadas.
- No añadir dependencias, secretos o configuración de clientes sin justificar.

## Plantilla

Fragmento real de `yii2-ed/src/rest/JsonController.php`:

~~~php
protected function debeExportarTodo(): bool {
  if (!$this->esFormatoDocumento($this->formato)) {
    return false;
  }

  return filter_var($this->req->get('todo', false), FILTER_VALIDATE_BOOLEAN);
}
~~~

## Evitar

- Cambios silenciosos en respuestas, rutas o serialización.
- `exit`, `die` o depuración en una clase reutilizable.
- Referencias a tablas o permisos de un cliente.

## Referencias

- `yii2-ed/composer.json`
- `yii2-ed/src/rest/JsonController.php`
- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
