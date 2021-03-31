---
description: >-
  Aunque añadir unas tiras de led en nuestra impresora pueda parecer algo más
  estético puede aportar algunas mejoras útiles
---

# Leds Neopixel

Aunque añadir unas tiras de led en nuestra impresora pueda parecer algo más estético puede aportar algunas mejoras útiles que podemos usar como, por ejemplo, iluminar nuestra impresora para supervisarla, mostrar el estado de la misma, etc....

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

Importante antes de comenzar, es importante identificar el voltaje de trabajo de la tira led a usar. Es aconsejable, por simplicidad, el uso de tiras de 5v que podamos alimentar directamente desde la placa y siempre contemplando el consumo de la tira y la corriente que pueda entregar el conector de tu placa. En el caso de otros voltajes o un número excesivo de leds deberemos usar una alimentacion externa.

Algunas placas como SKR cuentan con un añadido para poder disponer de más potencia como el módulo DCDC que podéis ver en la siguiente imagen.

![](https://telegra.ph/file/a9f9c8ddc4c343a046b79.png)

## Cableado

El cableado de los leds es muy simple ya que consta de 3 conexiones donde una es el voltaje, otro el de masa y el tercero el de señal.

![](https://telegra.ph/file/a0709da80ad04af9f7c03.png)

Es aconsejable el uso de cable 26 AGW para la conexión DI \(señal\) y de alimentación de los leds montados en un conector JST o dupont que suelen ser usados en los conectores de las placas, aquellas que tengan un conector dedicado como SKR 1.4.

Para placas que no lleven como la SKR 1.3 deberemos de usar un puerto compatible con señales Neopixel, en este caso el puerto SERVO sería compatible, si tenemos un Bltouch podemos mover el sensor de nivelación a un final de carrera y el Neopixel en el SERVO... este cambio va a requerir modificar el fichero pins de tu placa.

Es muy importante tener en cuenta que estas tiras leds tienen una dirección en la serigrafía que deberemos de tener en cuenta para la conexión de los cables.

Además normalmente estas tiran leds pueden cortarse para adaptarse a la medida necesaria a cubrir.

## Cambios en el Firmware

Buscaremos en configuration.h //\#define NEOPIXEL\_LED el cual descomentaremos para habilitarlo.  
El siguiente cambio sería definir el pin usado en NEOPIXEL\_PIN como ejemplo quedaría así para una SKR 1.4 \(que cuenta con un puerto dedicado para Neopixel\):

`#define NEOPIXEL_PIN P1_24`

![](https://telegra.ph/file/d83f7a08ce17bacb8a266.png)

A parte del pin de señal deberemos de indicar el tipo de led que ponemos en la siguiente linea de Marlin \(si no la conocemos debemos ir probando aunque el más común es el indicado\):

`#define NEOPIXEL_TYPE NEO_GRB`

Otro cambio necesario es indicar el número de pixeles en la siguiente linea:

`#define NEOPIXEL_PIXELS 25`

También aconsejable habilitar la animación de leds al arrancar para verificar que todo funcione correctamente en la siguiente linea:

`#define NEOPIXEL_STARTUP_TEST`

En las últimas versiones de Marlin también contamos con la opción de poder gestionar dos tiras Neopixel:

`#define NEOPIXEL_LED  
#if ENABLED(NEOPIXEL_LED)  
#define NEOPIXEL_TYPE NEO_GRB  
#define NEOPIXEL_PIN P1_24  
//#define NEOPIXEL2_TYPE NEOPIXEL_TYPE  
//#define NEOPIXEL2_PIN 5  
#define NEOPIXEL_PIXELS 25  
//#define NEOPIXEL_IS_SEQUENTIAL  
#define NEOPIXEL_BRIGHTNESS 255  
#define NEOPIXEL_STARTUP_TEST  
//#define NEOPIXEL_BKGD_LED_INDEX 0  
//#define NEOPIXEL_BKGD_COLOR { 255, 255, 255, 0 }  
#endif`

Pasaremos a configuration\_adv.h donde habilitaremos el menú para poder controlar desde la pantalla los leds de una forma sencilla:

`#define LED_CONTROL_MENU`

Podemos personalizar el color por defecto después del test de inicio si lo habilitamos en usando LED\_USER\_PRESET\_STARTUP y personalizarlo usando LED\_COLOR\_PRESETS los cuales son bastante intuitivos de entender en el código de Marlin

`#define LED_CONTROL_MENU  
#if ENABLED(LED_CONTROL_MENU)  
#define LED_COLOR_PRESETS  
#if ENABLED(LED_COLOR_PRESETS)  
#define LED_USER_PRESET_RED 255  
#define LED_USER_PRESET_GREEN 255  
#define LED_USER_PRESET_BLUE 255  
#define LED_USER_PRESET_WHITE 255  
#define LED_USER_PRESET_BRIGHTNESS 255  
#define LED_USER_PRESET_STARTUP  
#endif  
#endif`

## Controlando tus leds por Gcodes

Podemos controlar nuestra tira led usando el gcode M150, lo que nos permite poder personalizar como actúa durante la impresión mediante los scripts gcode de nuestro fileteador:

#### Usage <a id="Usage"></a>

`M150 [B<intensity>] [I<index>] [P<intensity>] [R<intensity>] [U<intensity>] [W<intensity>]`

**Parameters**

`[B<intensity>]`Blue component from 0 to 255

`[I<index>]`Index from 0 to n \(Requires `NEOPIXEL_LED`\)

`[P<intensity>]`Brightness from 0 to 255 \(Requires `NEOPIXEL_LED`\)

`[R<intensity>]`Red component from 0 to 255

`[U<intensity>]`Green component from 0 to 255

`[W<intensity>]`White component from 0 to 255 \(`RGBW_LED` or `NEOPIXEL_LED` only\)

