# Backend compartido: yii2-ed

Guías para la librería que provee infraestructura común a las aplicaciones Yii2.
Todo cambio aquí puede modificar contratos que consumen múltiples proyectos.

| Guía | Para qué |
| --- | --- |
| [01-convenciones-codigo.md](./01-convenciones-codigo.md) | Estilo y compatibilidad del código público. |
| [02-convenciones-arquitectura.md](./02-convenciones-arquitectura.md) | Límites, extensiones y contratos compartidos. |
| [03-patrones-aprobados.md](./03-patrones-aprobados.md) | Patrones existentes de infraestructura. |
| [04-desarrollo-funcionalidades.md](./04-desarrollo-funcionalidades.md) | Desarrollo y publicación segura de cambios. |
| [05-revision-codigo.md](./05-revision-codigo.md) | Revisión con impacto en consumidores. |
| [06-excepciones.md](./06-excepciones.md) | Excepciones y decisiones de compatibilidad. |

## Regla de alcance

Sólo entra a la librería infraestructura estable que sirve a dos o más
aplicaciones. El dominio de un cliente, sus permisos, modelos, módulos,
migraciones y configuración permanecen en su aplicación.
