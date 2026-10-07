# Convenciones de código

## Cuándo usarlo

Al crear o modificar PHP en `modelos/`, `modulos/`, `comandos/`,
`migraciones/` o `config/` de una aplicación `yii2-app-ed`.

## Reglas

- Usar K&R, dos espacios y sin tabuladores.
- Dejar una línea en blanco entre la declaración de una clase y su primer
  miembro, y usar llaves aun cuando el bloque tenga una sola instrucción.
- Clases en PascalCase; métodos y atributos en camelCase.
- La validación vive en `rules()` y `scenarios()`, no se duplica en controladores.
- La identidad viene de `Yii::$app->user->identity`; no leer `Authorization`.
- Usar ActiveRecord, Query Builder o parámetros enlazados.
- No registrar contraseñas, tokens ni encabezados de autorización.
- No exponer claves, tokens, hashes ni atributos sensibles de usuario en una
  respuesta de API.

## Plantilla

Fragmento de `yii2-app-ed/modelos/Usuario.php`:

~~~php
public function rules() {
  return [
    [['id', 'correo', 'clave', 'nombre', 'apellidos', 'rol'], 'required'],
    [['estatus'], 'integer'],
    [['correo'], 'string', 'max' => 128],
    [['id'], 'unique'],
  ];
}
~~~

Error esperado de API:

~~~php
return (new Respuesta())
  ->esError()
  ->mensaje('Producto no encontrado');
~~~

## Evitar

- `var_dump`, `die` o `exit` en código entregable.
- Concatenar entrada del usuario en SQL.
- Copiar una clase de `yii2-ed` a la aplicación.

## Referencias

- `yii2-app-ed/modelos/Usuario.php`
- `yii2-ed/src/data/Respuesta.php`
- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
