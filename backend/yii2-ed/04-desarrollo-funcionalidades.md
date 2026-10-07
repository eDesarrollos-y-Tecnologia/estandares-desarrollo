# Desarrollo de funcionalidades

## Cuándo usarlo

Al cambiar una clase, formato, contrato o dependencia de `yii2-ed`.

## Reglas

1. Confirmar reutilización por dos o más aplicaciones.
2. Identificar clases y consumidores afectados.
3. Preferir extensión o valor predeterminado compatible.
4. Actualizar documentación y verificar el contrato en el paquete y, cuando sea
   posible, en un consumidor.
5. Definir versión Composer y migración antes de publicar.

## Plantilla

~~~md
## Cambio compartido: <nombre>

- Contrato afectado: `<clase, método o formato>`
- Consumidores revisados: `<proyectos>`
- Compatible: sí / no
- Versión propuesta: `<mayor, menor o parche>`
- Migración requerida: `<pasos o no aplica>`
- Verificación realizada: `<comando o flujo y resultado>`
- No verificado: `<consumidor, entorno o motivo>`
~~~

## Evitar

- Publicar un cambio verificado sólo en una aplicación sin indicarlo.
- Resolver un caso único con una dependencia global.
- Marcar como parche un cambio incompatible.

## Referencias

- `yii2-ed/CHANGELOG.md`
- `yii2-ed/composer.json`
- [05-revision-codigo.md](./05-revision-codigo.md)
