# Backend PHP / Yii2

Guías para los dos repositorios base de backend. Cada repositorio mantiene su
propio índice porque sus responsabilidades no son intercambiables.

| Repositorio | Para qué |
| --- | --- |
| [yii2-app-ed](./yii2-app-ed/README.md) | Plantilla de aplicaciones, dominio y API del proyecto. |
| [yii2-ed](./yii2-ed/README.md) | Infraestructura reutilizable y contratos compartidos. |

Para crear una funcionalidad de aplicación, empezar en `yii2-app-ed`. Para
cambiar autenticación, respuestas, serialización, formatos o clases base,
consultar primero `yii2-ed` y revisar consumidores.
