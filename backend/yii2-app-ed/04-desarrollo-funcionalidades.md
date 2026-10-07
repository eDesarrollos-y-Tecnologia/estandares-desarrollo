# Desarrollo de funcionalidades

## Cuándo usarlo

Al implementar una funcionalidad que cambia datos, API, permisos, correo o
comandos de una aplicación.

## Reglas

1. Inspeccionar archivos relacionados, reutilizar sus patrones y conservar los
   cambios no relacionados ya presentes en el directorio de trabajo.
2. Explicar el enfoque antes de un cambio que afecte capas, datos,
   autenticación o base de datos; no añadir dependencias ni refactors fuera del
   alcance solicitado.
3. Definir migración y compatibilidad de datos antes del endpoint.
4. Validar en el modelo y autenticar mediante el controlador base.
5. Actualizar OpenAPI si cambia la superficie pública.
6. Verificar manualmente éxito, validación, autorización y errores relevantes.
7. Ejecutar comprobaciones proporcionales: como mínimo `php -l` sobre PHP
   modificado y `git diff --check` cuando haya repositorio Git. En migraciones,
   separar la validación de sintaxis de la confirmación contra una base real.
8. Reportar con precisión qué se verificó y qué depende de servidor,
   credenciales o proveedor externo.

## Plantilla

~~~md
## Funcionalidad: <nombre>

- Modelo y reglas: `<ruta>`
- Migración: `<ruta>`
- Endpoint y permisos: `<ruta>`
- Cambio de OpenAPI: sí / no
- Verificación realizada: `<flujo manual o comando>`
- No verificado: `<entorno, credencial o proveedor externo>`
~~~

## Evitar

- Implementar endpoint sin definir datos y validaciones.
- Mezclar refactors no relacionados.
- Presentar una comprobación local como evidencia de despliegue o producción.

## Referencias

- [01-convenciones-codigo.md](./01-convenciones-codigo.md)
- [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md)
- [05-revision-codigo.md](./05-revision-codigo.md)
