# Controlador personalizado

## Cuándo usarlo

Cuando el guardado o el borrado necesita ejecutar un caso de uso del dominio que
el CRUD base no puede resolver por sí mismo.

## Reglas

- El controlador CRUD siempre declara `buscador()`; un controlador personalizado
  sobrecarga `actionPost()` o `actionDelete()`.
- Conservar la autenticación heredada de `AuthController`.
- La acción recibe HTTP, invoca un método de negocio del modelo principal y
  devuelve `Respuesta`.
- El modelo principal valida, coordina otras entidades y abre la transacción si
  el caso de uso modifica varias tablas.
- Sobrescribir sólo la acción necesaria; el resto del CRUD permanece heredado.

## Plantilla

Ejemplo completo de un `actionPost()` para un pedido con detalles. El contrato
del modelo es `Pedido::guardarConDetalles()`: recibe los datos y el usuario
autenticado, valida, guarda el pedido y sus detalles dentro de una transacción,
y devuelve `false` agregando errores si no puede completar el caso de uso.

~~~php
<?php

namespace v1\controladores;

use app\modelos\Pedido;
use eDesarrollos\data\Respuesta;
use eDesarrollos\rest\AuthController;

class PedidoController extends AuthController {

  public $modelClass = Pedido::class;

  public function buscador(&$query, $request) {
    $id = $request->get($this->modeloID, '');

    if ($id !== '') {
      $query->andWhere([$this->modeloID => $id]);
    }
  }

  public function actionPost() {
    $pedido = new Pedido();

    if (!$pedido->guardarConDetalles(
      $this->req->getBodyParams(),
      $this->usuario
    )) {
      return (new Respuesta($pedido))
        ->esError()
        ->mensaje('Hubo un problema al guardar el pedido');
    }

    $pedido->refresh();
    return (new Respuesta($pedido))
      ->mensaje('Pedido guardado');
  }
}
~~~

Para `actionDelete()` se aplica el mismo límite: localizar el modelo y delegar
una baja de negocio, por ejemplo `$pedido->cancelar($this->usuario)`. No se
copia `actionDelete()` sólo para cambiar un mensaje.

## Evitar

- Usar `buscador()` para guardar o eliminar.
- Reimplementar la transacción, validaciones de detalles o reglas de inventario
  dentro del controlador.
- Sobrescribir `actionPost()` sólo para cambiar mensajes o filtros.
- Añadir un controlador especializado para carga de archivos: ese es un caso
  técnico separado.

## Referencias

- [08-controlador-crud.md](./08-controlador-crud.md)
- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
- [07-modelo.md](./07-modelo.md)

