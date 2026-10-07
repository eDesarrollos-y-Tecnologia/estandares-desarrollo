# Revisión de código

## Cuándo usarlo

Antes de publicar o aprobar un cambio en `yii2-ed`.

## Reglas

- Revisar consumidores y contrato público, no sólo el caso local.
- Evaluar compatibilidad, dependencias, documentación, versión y pruebas.
- El código generado por IA no tiene excepción de revisión.

## Plantilla

~~~md
- [ ] El problema es transversal y no dominio de cliente.
- [ ] Se identificaron consumidores y compatibilidad.
- [ ] Se documentó la API, formato o configuración modificados.
- [ ] Las dependencias nuevas están justificadas.
- [ ] La versión Composer comunica el impacto.
- [ ] Se ejecutaron pruebas focalizadas y validación del paquete.
~~~

## Evitar

- Aprobar porque no falla en el proyecto de origen.
- Cambiar rutas o respuestas sin versión y migración.
- Ocultar una incompatibilidad como corrección menor.

## Referencias

- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
- [06-excepciones.md](./06-excepciones.md)

