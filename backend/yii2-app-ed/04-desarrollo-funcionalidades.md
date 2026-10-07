# Desarrollo de funcionalidades

## Cuándo usarlo

Al implementar una funcionalidad que cambia datos, API, permisos, correo o
comandos de una aplicación.

## Reglas

1. Buscar una implementación equivalente.
2. Definir migración y compatibilidad de datos antes del endpoint.
3. Validar en el modelo y autenticar mediante el controlador base.
4. Actualizar OpenAPI si cambia la superficie pública.
5. Probar éxito, validación, autorización y errores relevantes.

## Plantilla

~~~md
## Funcionalidad: <nombre>

- Modelo y reglas: `<ruta>`
- Migración: `<ruta>`
- Endpoint y permisos: `<ruta>`
- Cambio de OpenAPI: sí / no
- Pruebas ejecutadas: `<comando y resultado>`
- Pendiente de entorno: `<detalle>`
~~~

## Evitar

- Implementar endpoint sin definir datos y validaciones.
- Mezclar refactors no relacionados.
- Declarar producción comprobada tras una prueba local.

## Referencias

- [01-convenciones-codigo.md](./01-convenciones-codigo.md)
- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
- [05-revision-codigo.md](./05-revision-codigo.md)

