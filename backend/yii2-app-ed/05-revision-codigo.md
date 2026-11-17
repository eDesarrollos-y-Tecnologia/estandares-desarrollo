# Revisión de código

## Cuándo usarlo

Antes de solicitar o aprobar un pull request de una aplicación.

## Reglas

- El código generado por IA se revisa con los mismos criterios.
- Revisar alcance, arquitectura, validación, seguridad, migraciones y
  verificación funcional.
- Documentar excepciones antes de aprobarlas.

## Plantilla

~~~md
- [ ] El cambio pertenece a la aplicación y no a `yii2-ed`.
- [ ] La validación está en el modelo.
- [ ] No se duplicó autenticación ni respuesta.
- [ ] La migración preserva datos e índices justificados.
- [ ] No hay secretos, SQL inseguro ni logs sensibles.
- [ ] OpenAPI se actualizó si aplica y el flujo afectado fue verificado.
- [ ] Las columnas JSON se responden como objetos o arreglos, no como JSON
  serializado dentro de un string.
- [ ] Una nueva bitácora tiene migración, permiso de visor y exclusión de su
  propia auditoría.
- [ ] TOTP no expone su semilla, exige confirmación para activarse y requiere
  un código vigente para desactivarse.
~~~

## Evitar

- Aprobar sólo porque funciona el caso exitoso.
- Aceptar dependencias o patrones fuera del requisito.
- Tomar legado como norma.

## Referencias

- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
- [06-excepciones.md](./06-excepciones.md)
