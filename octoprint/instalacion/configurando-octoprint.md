# Configurando Octoprint

Si seguimos los pasos anteriores ya tendremos acceso al interfaz web de Octoprint y podremos comenzar a personalizarlo para nuestra impresora/uso.

![](https://telegra.ph/file/ad0d13fdeb73414b91280.png)

* Seguiremos el asistente de configuración que es muy sencillo e intuitivo, no tiene mucho misterio.
* Una vez finalizada la configuración inicial ya estaremos listos para conectar a nuestra impresora donde idealmente poniendo el puerto/velocidad en AUTO y teniendo el cable USB conectado entre nuestra Pi e impresora debería conectar y poder empezar a tener lecturas de temperaturas.
* Para comprobar que todo esta correcto iremos a la pestaña Control y desde ahi probaremos a realizar un homing o mover nuestros ejes, es un proceso muy intuitivo.
* También podemos comprobar que podemos ajustar las temperaturas de nuestro hotend y cama desde la pestaña Temperature.
* Ya está!!! tienes tu impresora con Octoprint para poder gestionarla remotamente!!! puedes arrastrar tus gcode directamente a la web de Octoprint para subirlos y dar la orden de imprimir.
* También puedes conectar una cámara USB o GPIO, de las soportadas que suelen ser prácticamente todas.
* Añadir más plugins para mejorar y vitaminar tu Octoprint como DisplayLayerProcess \(que te permite enviar estado de la impresión a la pantalla de tu impresora\), PrintTimeGenious \(que te da estimaciones reales del tiempo de finalización de una impresión\), Cancel Objects \(que te permite, si tu firmware Marlin lo tiene habilitado, cancelar piezas que esten saliendo mal sin parar toda la impresión\), IPConnect \(que te muestra la IP asignada y por si esta cambia\).
* Por otro lado y para una gestión remota de tu impresora cuando no estás en casa te aconsejamos los plugin de Telegram o [Octoeverywhere](https://octoeverywhere.com/) \(podras ver el interfaz completo de Octoprint desde cualquier lado y de forma muy sencilla\) que tendrás .
* Hay muchísimos plugins con este tipo de mejoras, no dudes en revisarlos!!!

