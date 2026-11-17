# Patrones aprobados

## Cuándo usarlo

Antes de crear una abstracción reutilizable dentro de `yii2-ed`.

## Reglas

- Los controladores REST base centralizan formatos, paginación y respuesta.
- `ModeloBase` contiene comportamiento ActiveRecord transversal.
- Los formateadores representan una misma respuesta en varios formatos.
- La importación parte del archivo exportado por el mismo modelo; el modelo
  decide cómo encontrar o crear la instancia que recibirá cada fila.
- La bitácora de cambios y de peticiones se integra como componente Yii, no se
  repite con eventos en cada entidad del dominio.
- Repository, Service, Factory, DTO y un contenedor propio no están aprobados.

## Regla: importación de registros

`ModeloBase::resolverModeloImportacion(array $fila)` es el punto de extensión
para una importación XLSX. La implementación predeterminada busca una llave
primaria simple cuando viene en la fila y, si no existe, regresa una instancia
nueva. Un modelo puede reemplazarlo para usar una llave natural o compuesta.

~~~php
public static function resolverModeloImportacion(array $fila) {
  $correo = trim((string)($fila['correo'] ?? ''));
  $modelo = $correo === '' ? null : static::findOne(['correo' => $correo]);

  return $modelo ?? new static();
}
~~~

No se exige una columna `id` si el dominio puede resolver una entidad con otro
criterio. La misma hoja exportada debe ser válida como plantilla de importación.

## Regla: bitácora transversal

`ModeloBase` notifica a `bitacoraCambio` tras guardar o eliminar. La aplicación
decide si registra cada modelo mediante `bitacoraHabilitada()`; los modelos que
almacenan la propia bitácora deben devolver `false` para evitar recursión.

- Guardar sólo atributos modificados en actualizaciones.
- Omitir contraseñas, tokens, semillas TOTP y atributos sensibles.
- No asumir que hay petición HTTP al ejecutar comandos de consola.
- Usar arreglos y objetos nativos para columnas `json` o `jsonb`; el driver de
  PostgreSQL realiza la conversión correspondiente.

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

