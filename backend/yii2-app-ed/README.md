# Backend de aplicación: yii2-app-ed

Guías para APIs y aplicaciones construidas con la plantilla yii2-app-ed. La
aplicación contiene el dominio del cliente; yii2-ed contiene infraestructura
reutilizable.

| Guía | Para qué |
| --- | --- |
| [01-convenciones-codigo.md](./01-convenciones-codigo.md) | Estilo PHP, nombres, validación, seguridad y configuración. |
| [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md) | Capas, módulos Yii, API, datos y límites con la librería. |
| [03-patrones-aprobados.md](./03-patrones-aprobados.md) | Patrones permitidos y los que aún no son convención. |
| [04-desarrollo-funcionalidades.md](./04-desarrollo-funcionalidades.md) | Procedimiento para entregar una funcionalidad. |
| [05-revision-codigo.md](./05-revision-codigo.md) | Lista de revisión antes y durante un cambio. |
| [06-excepciones.md](./06-excepciones.md) | Excepciones, deuda técnica y decisiones. |

## Regla de alcance

El dominio, modelos de negocio, permisos particulares, módulos, migraciones y
configuración por entorno viven en la aplicación. Una mejora que pueda servir a
dos o más aplicaciones se evalúa para yii2-ed; no se copia infraestructura de
vendor a la aplicación.
