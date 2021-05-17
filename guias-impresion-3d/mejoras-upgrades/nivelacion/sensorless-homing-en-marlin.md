---
description: >-
  Esta funcionalidad es usada desde Marlin para realizar la función de endstop o
  final de carrera.
---

# Sensorless Homing en Marlin

![](https://telegra.ph/file/dfcaa134fa8e2766de7ca.png)

Algunos drivers TMC pueden detectar el choque contra un obstáculo que evita que se pueda mover normalmente. Esta funcionalidad es usada desde Marlin para realizar la función de endstop o final de carrera.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

Es muy importante recordar/recomendar que esta función se habilite solamente en los ejes X e Y ya que el nivel de precisión dista del requerido en un eje Z donde se aconsejan otros métodos como sensores de nivelación automática.

En esta guía no vamos a entrar en la configuración hardware para poder habilitar fisicamente la impresora y su electrónica si no la parte de configuración de Marlin. En cualquier caso recordar que es imprescindible que los drivers esten instalados y configurados en modo UART y dependiendo de la placa es necesario habilitar el jumper dedicado para DIAG/Sensorless.

{% tabs %}
{% tab title="SKR v2" %}
![](../../../.gitbook/assets/image%20%2873%29.png)
{% endtab %}

{% tab title="SKR 1.3" %}
![](https://telegra.ph/file/adbac297d5197536c1595.jpg)
{% endtab %}

{% tab title="SKR E3 TURBO" %}
![](https://telegra.ph/file/4866b596132d65ee54dcb.jpg)
{% endtab %}

{% tab title="SKR MINI E3 v2" %}
![](https://telegra.ph/file/764231febc9c167d4b9d4.jpg)
{% endtab %}
{% endtabs %}

## Configuración en Marlin

Una vez ya tengamos la parte hardware correctamente configurada/instalada tendremos que habilitar la función Sensorless en Marlin.  
Empezaremos por veríficar que los la lógica de los finales de carrera de X e Y están correctos en configuration.h

> \#define X\_MIN\_STOP\_INVERTING false  
> \#define Y\_MIN\_STOP\_INVERTING false

Continuaremos los cambios en configuration\_adv.h habilitaremos lo siguiente:

> \#define SENSORLESS\_HOMING // StallGuard capable drivers only

Una parte clave para que el funcionamiento de Sensorless sea óptimo es tener en cuenta los siguientes puntos que trataremos a continuación con detalle:

* Velocidad de Homing
* Corriente del driver
* Sensibilidad

> **Es importante recordar que la funcionalidad de Sensorless solo funciona correctamente con los drivers en modo stealChop habilitado**

### **Velocidad de Homing**

Marlin dispone de una configuración de velocidades específicas para el Homing que se pueden encontrar dentro de configuration.h:

> `#define HOMING_FEEDRATE_XY (15*60)  
> #define HOMING_FEEDRATE_Z (4*60)`

Valores más bajos hacen que el proceso de Homing sea más lento pero mejoran la detección y precisión si usamos Sensorless

### Corriente del driver

Como comantamos anteriormente esta función de los drivers TMC permiten detectar el choque contra un objeto y esto lo hacen comprobando el consumo de corriente del driver. Durante un movimiento normal del motor el consumo, a una velocidad estable, suele ser siempre el mismo... ahora bien cuando encuentra algún obstáculo el motor aumenta el consumo para continuar su camino y es en ese punto donde los drivers TMC interpretan como un choque y por tanto como un final de carrera/endstop en nuestras impresoras.

Como norma lo ideal es usar el minimo de corriente sin que nuestro motor pierda pasos en este caso Marlin dispone de una configuración especialmente útil para Sensorless que podemos encontrar en configuration\_adv.h

> \#define X\_CURRENT\_HOME X\_CURRENT  
> \#define Y\_CURRENT\_HOME Y\_CURRENT

Esta opción nos permite ajustar la corriente en ese punto crítico para Sensorless, normalmente bajando un poco la corriente normal por poner un ejemplo podriamos reducir en 20 unidades ese valor para mejorar las lecturas

> \#define X\_CURRENT\_HOME \(X\_CURRENT - 20\)  
> \#define Y\_CURRENT\_HOME \(Y\_CURRENT - 20\)

{% hint style="info" %}
En el caso de usar **TMC2130 en modo SPI** es aconsejable en caso de problemas habilitar:

**\#define SPI\_ENDSTOPS**
{% endhint %}

{% hint style="info" %}
Para **impresoras CoreXY** o similares \(el movimiento de los ejes X e Y se realice mediante el uso de dos motores\) es aconsejable habilitar la siguiente función de Marlin para que los movimientos del homing no realicen movimientos diagonales que hagan funcionan ambos motores a la vez

**\#define QUICK\_HOME**
{% endhint %}

### **Sensibilidad**

Dependiendo de la mecánica, motores, etc de nuestra impresora los valores para detectar esos obstáculos no es siempre la misma, es por eso que en Marlin permiten configurar unos umbrales llamados comunmente sensibilidad

> `#define X_STALL_SENSITIVITY 100  
> #define Y_STALL_SENSITIVITY 100`

Estos valores van desde el valor más bajo donde no detectará ningún cambio en la corriente hasta el valor máximo en el cual cualquier variación en el consumo de corriente disparará la señal de final de carrera.

Este valor máximo varia dependiendo del tipo de TMC que tengamos:

* TMC2209 donde el valor más bajo es 0 y el más alto es 255
* Otros TMC donde el valor más bajo es +63 y el más alto -64

Relacionado con el ajuste de sensibilidad hay otros dos valores de Marlin que deberemos ajustar siempre que habilitemos Sensorless:

* \#define SENSORLESS\_BACKOFF\_MM { 2, 2 } el cual siempre hara una retracción de 2mm en el lado opuesto del Homing para asegurar que el movimiento de homing no es en una posición de bloqueo
* \#define HOMING\_BUMP\_MM { 0, 0, 2 } con este y al igual que la anterior le decimos que una vez hecha la primera aproximación no retroceda para la segunda aproximación

## Comprobaciones y ajustes

* Una vez subido el nuevo Marlin con Sensorless activo hay que asegurarse que se refrescan los valores de la EEPROM, esto lo podemos hacer desde Pronterface \(u otro cliente terminal\) lanzando M502 y M500 o desde la pantalla en modo emulación Marlin desde Configuracion realizar un Restaurar, Cargar y Salvar EEPROM
* Desde el terminal Pronterface lanzar M122 que nos indicará los valores UART de nuestros drivers, en este caso hay que prestar atención a que la fila stallGuard para la columna del eje X e Y tengan los valores configurados en nuestro firmware Marlin
* Si al realizar una prueba de Homing no lo hace correctamente parando o no parando en el lugar deseado o con la fuerza incorrecta podremos usar la pantalla en modo emulación Marlin Configuracion/Avanzado/TMC para ir a la sección de sensibilidad de homing y ajustarla hasta encontrar la más adecuada. Recordar guardar los datos en la EEPROM \(M500 o Configuración/Salvar EEPROM\) además de anotarlos en nuestros ficheros de Marlin para futuras modificaciones.

