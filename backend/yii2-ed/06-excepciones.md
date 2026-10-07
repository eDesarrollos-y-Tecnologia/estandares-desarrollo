# Excepciones

## Cuándo usarlo

Cuando un cambio de `yii2-ed` necesita apartarse temporalmente de una regla de
compatibilidad o arquitectura.

## Reglas

- Registrar contrato afectado, consumidores, riesgo y versión de publicación.
- Definir responsable, aprobación y condición de retiro.
- Si sólo aplica a una aplicación, no cambia `yii2-ed`.

## Plantilla

~~~md
## Excepción compartida: <título>

- Contrato afectado: `<clase, método o formato>`
- Consumidores: `<proyectos>`
- Riesgo y compatibilidad: `<detalle>`
- Versión: `<versión>`
- Aprobó: `<persona>`
- Retiro: `<fecha o condición>`
~~~

## Evitar

- Introducir un cambio incompatible sin versión mayor.
- Usar una excepción para incorporar dominio de cliente al núcleo.
- Mantener una excepción sin seguimiento.

## Referencias

- [04-desarrollo-funcionalidades.md](./04-desarrollo-funcionalidades.md)
- [05-revision-codigo.md](./05-revision-codigo.md)

