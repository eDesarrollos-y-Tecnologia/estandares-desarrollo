# Configuración y envío de correo

## Cuándo usarlo

Al configurar SMTP, personalizar la plantilla HTML o habilitar el módulo de
correo programado de `yii2-ed`.

## Reglas

- `config/correo.php` define el componente `mailer`; las credenciales son propias
  del entorno y no se suben al repositorio.
- `viewPath` apunta a `@app/mail`. Al habilitar correo, crear el layout del
  proyecto en `mail/layouts/html.php`; ahí viven las personalizaciones HTML.
- El módulo puede tener un layout de reserva dentro de `vendor`, pero la
  aplicación no debe depender de él ni modificarlo.
- Registrar el módulo `mail` tanto en web como en consola.
- Ejecutar la migración del módulo antes de usar las notificaciones guardadas.
- Reportar la verificación de SMTP sin presentar una configuración local como
  prueba de entrega real.

## Plantilla

Componente de `config/correo.php`:

~~~php
<?php

return [
  'class' => \yii\symfonymailer\Mailer::class,
  'viewPath' => '@app/mail',
  'useFileTransport' => false,
  'transport' => [
    'scheme' => 'smtp',
    'host' => 'smtp.gmail.com',
    'username' => '',
    'password' => '',
    'port' => 587,
  ],
];
~~~

Carga y registro en `config/web.php`:

~~~php
$mailer = require __DIR__ . '/correo.php';

$config = [
  'components' => [
    'mailer' => $mailer,
  ],
  'modules' => [
    'mail' => ['class' => 'eDesarrollos\mail\Module'],
  ],
];
~~~

En `config/consola.php` se registra el mismo módulo:

~~~php
'bootstrap' => ['log', 'mail'],
'modules' => [
  'mail' => ['class' => 'eDesarrollos\mail\Module'],
],
~~~

Crear `mail/layouts/html.php` dentro del proyecto:

~~~php
<?php

use yii\helpers\Html;

/* @var $this \yii\web\View */
/* @var $message \yii\mail\MessageInterface */
/* @var $content string */
?>
<?php $this->beginPage() ?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=<?= Yii::$app->charset ?>" />
  <title><?= Html::encode($this->title) ?></title>
  <?php $this->head() ?>
</head>
<body>
  <?php $this->beginBody() ?>
  <?= $content ?>
  <?php $this->endBody() ?>
</body>
</html>
<?php $this->endPage() ?>
~~~

Migrar y procesar notificaciones:

~~~sh
php yii migrate --migrationPath=@eDesarrollos/mail/migraciones
php yii mail/cron
~~~

## Evitar

- Guardar una contraseña SMTP real en `config/correo.php` o en una guía.
- Usar `useFileTransport` en producción.
- Omitir el registro en consola y esperar que `php yii mail/cron` funcione.
- Crear o modificar el layout dentro de `vendor`.
- Omitir `mail/layouts/html.php` y depender del layout de reserva de la librería.

## Referencias

- `yii2-app-ed/config/correo.php`
- `yii2-app-ed/config/web.php`
- `yii2-app-ed/config/consola.php`
- `yii2-app-ed/mail/layouts/html.php`
- `yii2-ed/src/mail/README.md`
