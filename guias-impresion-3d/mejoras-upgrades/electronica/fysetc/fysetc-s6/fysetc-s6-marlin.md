---
description: Configurando nuestro Marlin para sacar todo el provecho de nuestra Fysetc S6
---

# FYSETC S6 - Marlin

![](../../../../../.gitbook/assets/image%20%28114%29.png)

Os aconsejamos seguir nuestra [guía para "cocinar" vuestro propio Marlin](../../../marlin-guia-compilacion/) que tenemos en la seccion /Marlin de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## platformio.ini 

![](../../../../../.gitbook/assets/image%20%28113%29.png)

En este fichero deberemos indicar el chipset que tiene nuestra placa por lo que deveremos buscar en el inicio del fichero el valor "env\_default" y cambiarlo por el siguiente:

> default\_envs = FYSETC\_S6

Una de las grandes funcionalidades que tiene esta placa que no suelen tener otras es que su bootloader permite su actualización usando diferentes métodos. Uno de ellos es la estándar mediante SD en la pantalla LCD o externa ya que la placa no cuenta con SD o mediante USB poniendo la placa en modo BOOT0.

Por defecto la configuración de esta placa viene  preparada para esta segunda usando DFU lo que no siempre es cómodo teniendo que acceder a la placa, cambiar el pin y conectando el cable USB. 

Para que el proceso no de un fallo si no lo hacemos de esa forma y nos deje el binario para su copia en la SD comentaremos las siguientes lineas en el fichero indicado

```cpp
.../ini/stm32f4.ini

#
# FYSETC S6 (STM32F446RET6 ARM Cortex-M4)
#
[env:FYSETC_S6]
platform          = ${common_stm32.platform}
extends           = common_stm32
platform_packages = tool-stm32duino
board             = marlin_fysetc_s6
build_flags       = ${common_stm32.build_flags}
  -DVECT_TAB_OFFSET=0x10000
  -DHAL_PCD_MODULE_ENABLED
extra_scripts     = ${common.extra_scripts}
  pre:buildroot/share/PlatformIO/scripts/generic_create_variant.py
debug_tool        = stlink
# U1JO upload_protocol   = dfu
# U1JO upload_command    = dfu-util -a 0 -s 0x08010000:leave -D "$SOURCE"
```

## configuration.h 

![](../../../../../.gitbook/assets/image%20%28130%29.png)

Os aconsejamos añadir un comentario a cualquier linea que modifiquemos ya que después  nos será mucho más sencillo encontrar nuestras modificaciones. Para ello podemos añadir: 

```cpp
En el caso que no tenga un comentario ya la linea
... // 3DWORK 
En el caso que ya tenga un comentario
... // 3DWORK ...
```

Comenzaremos por configurar correctamente los **SERIAL** para esta máquina algo clave para el correcto funcionamiento de nuestra conexión TFT, USB o WIFI.

```cpp
#define SERIAL_PORT_2 -1 // 3DWORK FYSETC
```

**BAUDRATE** nos ayuda a definir la velocidad de la comunicación por el puerto **SERIAL** lo normal es usar 115200 o 250000.

```cpp
#define BAUDRATE 115200
```

Ahora definiremos nuestro **MOTHERBOARD** el cual al igual que hicimos el platformio.ini nos indica el tipo de chipset que lleva nuestra placa y que permite a Marlin usar una configuración de pines u otra.

```cpp
#define MOTHERBOARD BOARD_FYSETC_S6_V2_0 // 3DWORK FYSETC
```

Definiremos el **número de extrusores** y **diámetro del filamento** que usa nuestra impresora que básicamente indica el diámetro del mismo. Normalmente impresoras usan filamento del 1.75 aunque otra medida más o menos estándard es 3.

```cpp
// This defines the number of extruders
// :[0, 1, 2, 3, 4, 5, 6, 7, 8]
#define EXTRUDERS 1

// Generally expected filament diameter (1.75, 2.85, 3.0, ...). Used for Volumetric, Filament Width Sensor, etc.
#define DEFAULT_NOMINAL_FILAMENT_DIA 1.75
```

**Thermistores**, en estas variables indicaremos que thermistores tenemos, os aconsejamos que identifiquéis cual es el vuestro o en su defecto uséis uno de valor 1 que suele ser el mas normal.

Es importatísimo indicar el tipo de thermistor correcto para que las medidas que Marlin recibe sean las correctas para evitar problemas de extrusión y/o fallos de lecturas.

En el caso que queramos desactivar o activar la cama caliente comentaremos o activaremos el thermistor de la cama, de esta forma Marlin sabe si tiene o no cama caliente.

En nuestro ejemplo y para esta placa usaremos un hotend y una cama caliente con un sensor de temperatura estándard.

```cpp
#define TEMP_SENSOR_0 1 // TE0
#define TEMP_SENSOR_1 0 // TE1
#define TEMP_SENSOR_2 0 // TE2
#define TEMP_SENSOR_3 0
#define TEMP_SENSOR_4 0
#define TEMP_SENSOR_5 0
#define TEMP_SENSOR_6 0
#define TEMP_SENSOR_7 0
#define TEMP_SENSOR_BED 1 // TB
#define TEMP_SENSOR_PROBE 0 
#define TEMP_SENSOR_CHAMBER 0
#define TEMP_SENSOR_COOLER 0
```

{% hint style="info" %}
**Recordad que esta placa tiene soporte para 3 extrusores y cama caliente con lo que en caso de usarlos deberemos activar el tipo correcto en cada uno de ellos**
{% endhint %}

**PID**, otra parte muy importante es el PID del que podéis obtener más información detallada en la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot) o [en nuestra guía de calibración](../../../../calibracion_3d.md#4-ajuste-pid).

Básicamente el PID ayuda a que Marlin controle de forma adecuada la temperatura sin tener grandes fluctuaciones.

```cpp
#define PIDTEMP // 3DWORK PID
#define PIDTEMPBED // 3DWORK PID
```

**ENDSTOPS o finales de carrera**, es importante verificar cuando realizamos un cambio de electrónica y encendemos por primera vez que la lógica de los finales de carrera es la correcta usando el comando M119 desde un terminal como Pronterface... esto es OPEN en estado normal y TRIGGERED cuando están pulsados. Podremos invertir esta lógica cambiando:

```cpp
#define X_MIN_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define Y_MIN_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define Z_MIN_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define X_MAX_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define Y_MAX_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define Z_MAX_ENDSTOP_INVERTING false // Set to true to invert the logic of the endstop.
#define Z_MIN_PROBE_ENDSTOP_INVERTING false // Set to true to invert the logic of the probe.
```

Por otro lado y aunque lo normal es que las máquinas realicen el home en MIN \(esquina inferior izquierda\) en lugar de MAX \(esquina superior derecha\), usando HOME\_DIR para este cambio -1\(MIN\) 1\(MAX\), debemos recordar que en el caso de cambiar la lógica habilitar los finales de carrera correspondientes:

```cpp
#define USE_XMIN_PLUG
#define USE_YMIN_PLUG
#define USE_ZMIN_PLUG
//#define USE_XMAX_PLUG
//#define USE_YMAX_PLUG
//#define USE_ZMAX_PLUG
```

**Drivers**, como ya hemos comentado esta placa cuenta con 6 zócalos para drivers soportando diferente tipos de drivers y sus configuraciones. Podéis encontrar como realizar la instalación de los mismos en la[ primera parte de la guia](./).

En nuestro ejemplo utilizaremos unos TMC2209 en modo UART \(inteligente\), con doble Z independiente y un solo extrusor:

```cpp
#define X_DRIVER_TYPE  TMC2209 // 3DWORK DRIVERS
#define Y_DRIVER_TYPE  TMC2209 // 3DWORK DRIVERS
#define Z_DRIVER_TYPE  TMC2209 // 3DWORK DRIVERS
//#define X2_DRIVER_TYPE A4988
//#define Y2_DRIVER_TYPE A4988
#define Z2_DRIVER_TYPE TMC2209 // 3DWORK DRIVERS
//#define Z3_DRIVER_TYPE A4988
//#define Z4_DRIVER_TYPE A4988
#define E0_DRIVER_TYPE TMC2209 // 3DWORK DRIVERS
```

## Actualizar firmware

La Fysetc S6 cuenta con varias opciones para poder actualizar su firmware.

### Usando SD 

La opción más sencilla es la de usar una SD para la actualización que es el método por defecto que usa el bootloader de la placa.

{% hint style="info" %}
**Es importante recordar que esta placa no cuenta con una ranura de SD integrada así que necesitaremos una pantalla LCD \(use EXP1/2 para su conexión\) o un módulo de SD conectado en EXP2.**
{% endhint %}

En este caso es muy sencillo y tan solo tenemos que copiar nuestro firmware.bin en la raíz de la **SD formateada en FAT32**, insertar la SD en la ranura del módulo y esperar unos 30seg a que finalice el proceso. Una vez finalizado **el fichero firmware.bin debe quedar renombrado a OLD.BIN** el cual podemos guardar a modo de copia de seguridad \(recordar que un firmware compilado no se puede modificar, ese proceso solamente se puede hacer con los ficheros fuentes de vuestro firmware y vuelto a compilar\).

### Usando modo DFU/USB

Otra forma de actualizar nuestra placa, sobretodo en el caso de no disponer de un módulo o pantalla LCD con SD, es usando el modo DFU.

* [Descargaremos STM32 Cube Programmer](https://www.st.com/zh/development-tools/stm32cubeprog.html) \(Windows, Mac y Linux\) desde su sitio web 
* Abriremos la aplicación

![](../../../../../.gitbook/assets/image%20%28120%29.png)

* Pondremos nuestra placa Fysetc S6 en modo DFU
  * **S6 v1.2**, con la placa completamente apagada **dejaremos pulsado el botón BOOT0 y con el botón pulsado conectaremos mediante el cable USB la placa a nuestro ordenador**, esto hará que entre en el modo DFU. Una vez conectado y en modo DFU soltaremos el botón. 
  * **S6 v2**, con la placa completamente apagada **pondremos el jumper del conector BOOT0 a 3.3v y conectaremos mediante el cable USB la placa a nuestro ordenador**, esto hará que entre en el modo DFU. **RECORDAR!!! una vez finalizado el proceso de actualización de firmware quitar el jumper de BOOT0**

![](../../../../../.gitbook/assets/image%20%28118%29.png)

* Una vez en modo DFU volveremos a la aplicación y realizaremos los siguientes pasos:
  * Pulsaremos el **botón refrescar para que detecte el puerto DFU**
  * Pulsaremos el botón **CONNECT**
  * Seleccionaremos el **firmware.bin** creado
  * En **START ADDRESS** pondremos **0x8010000**
  * Pulsaremos **START PROGRAMMING**

**Si todo ha funcionado correctamente ya tendremos nuestro nuevo firmware en nuestra Fysetc S6!!!**

## LCD Fysetc MINI 12864 2.1 \(Neopixel\)

![](../../../../../.gitbook/assets/image%20%28103%29.png)

Esta pantalla es muy adecuada para usar junto con esta electrónica ya que tiene un formato muy compacto fácilmente integrable y una estética muy buena así como la inclusión de SD, perfecta para la actualización del firmware, así como unos leds controlables desde Marlin que siempre dan mucho juego. 

{% tabs %}
{% tab title="Amazon" %}
{% embed url="https://amzn.to/3eWKKSm" %}
{% endtab %}

{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_AWJJoh" %}
{% endtab %}
{% endtabs %}

## Definición pines FYSETC S6

Marlin/src/pins/stm32f4/pins\_FYSETC\_S6.h 

