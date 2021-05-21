---
description: >-
  Mejora la calidad de tus impresionas a la vez que evitas saturación en el
  flujo de datos
---

# Arc-Welder



{% embed url="https://plugins.octoprint.org/plugins/arc\_welder/" %}

Este potentísimo plugin promete mejorar la calidad de tus impresiones además de ser muy muy adecuado para el uso de tu impresora desde Octoprint.  
Básicamente se encarga de mejorar el gcode laminado para sustituir movimientos lineales por arcos que son más optimos con un descenso drástico de la comunicación de gcodes entre Octoprint y tu impresora \(que ayudará a evitar cuellos de botella\), mejorar la calidad \(reducción la cantidad de movimientos y reduciendo las extrusiones/retracciones\) además de reducir considerablemente el tamaño de tus archivos.  
No es una tecnología para nada nueva pero con el procesamiento de la electrónica actual esta comenzando a ser una referencia.

{% embed url="https://youtu.be/18uYYXecH5g" %}

Su uso es muy sencillo, puedes ponerlo en modo automático para que cuando subes un nuevo gcode automático lo transforme \(añadirá uno nuevo con .aw.gcode en su nombre\) o hacerlo manualmente desde un nuevo icono \(flechas\) en el listado de ficheros.  
Cuenta con un interfaz que te informa del estado de transformación además que te da datos de estadísticas de las mejoras aplicadas.

![](../../.gitbook/assets/image%20%2832%29.png)

Tienes una parte de configuración que puedes personalizar a tu gusto para generar el nuevo gcode optimizado para tu máquina.

![](../../.gitbook/assets/image%20%2839%29.png)

Es importante recalcar que para que funcione correctamente el Marlin de tu impresora debe tener habilitado ARC\_SUPPORT \(configuration\_adv.h\) que desde la versión 2.0.6 ha sido mejorado enormemente. Si no sabes si lo tienes o no habilitado puedes lanzar un comando M115 desde tu cliente terminal favorito a tu impresora para ver el estado de esta opción.  
Por supuesto es también compatible con otros firmwares pero deberás comprobar la compatibilidad en cada caso.

