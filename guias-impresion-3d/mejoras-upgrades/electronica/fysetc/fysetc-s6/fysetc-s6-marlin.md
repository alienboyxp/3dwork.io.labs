---
description: Configurando nuestro Marlin para sacar todo el provecho de nuestra Fysetc S6
---

# FYSETC S6 - Marlin

![](../../../../../.gitbook/assets/image%20%28117%29.png)

Os aconsejamos seguir nuestra [guía para "cocinar" vuestro propio Marlin](../../../marlin-guia-compilacion/) que tenemos en la seccion /Marlin de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## platformio.ini 

![](../../../../../.gitbook/assets/image%20%28116%29.png)

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

![](../../../../../.gitbook/assets/image%20%28137%29.png)

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

Por otro lado recordaros que si disponéis de drivers TMC en modo UART podéis utilizar, si el driver lo soporta, el uso de SensorLess que permitirá prescindir de los finales de carrera físicos liberando de componentes y cables. Podéis encontrar información de como configurar vuestra Fysetc S6 [aquí](./#drivers) y como activar SensorLess en nuestra [guía](../../../nivelacion/sensorless-homing-en-marlin.md) para Marlin.

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

**STEPS o pasos de motor**, tos valores indicaremos a Marlin los pasos de motor para realizar movimientos. En este ejemplo usaremos los que una impresora Ender lleva por defecto, en todo caso de nuevo os remitimos a la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot) o [aquí](../../../../calibracion_3d.md#3-ajuste-de-los-pasos-de-los-motores-ejes) para el proceso calibrar estos pasos para nuestro extrusor y ejes de movimiento.

```cpp
#define DEFAULT_AXIS_STEPS_PER_UNIT { 80.00, 80.00, 400.00, 93 } // 3DWORK pasos para Ender3 ejes X, Y, Z,Extrusor
```

**S\_CURVE\_ACCELERATION**, esta funcionalidad ofrece una gran mejora en la calidad de impresion ya que suaviza la aceleración o deceleracion cuando el cabezal de impresión está en movimiento.

```cpp
#define S_CURVE_ACCELERATION // 3DWORK MEJORAS
```

{% hint style="info" %}
**Nota importate!!**! es que dependiendo de nuestra versión de Marlin y si usamos Linear Advance no son compatibles. En la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot) o en nuestra [guía de calibración](../../../../calibracion_3d.md#12-linear-advance-opcional) podréis encontrar más información detallada de Linear Advanced
{% endhint %}

**PROBE** o sensor de nivelación, ya hemos comentado hay una [guía detallada de como instalar un sensor BLTouch](../../../nivelacion/bltouch-3dtouch.md) vamos a realizar una revisión de las partes más importantes a tener en cuenta en el caso que querámos instalar un sensor de nivelación, algo muy aconsejable para asegurarnos que nuestras impresiones siempre tienen una primera capa lo más perfecta posible.

Comenzaremos por indicar donde está conectado nuestro sensor de nivelación, en este caso lo conectaremos en el [conector final de carrera de Z-](./#bltouch)

```cpp
#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN // 3DWORK PROBE indicamos a Marlin que nuestro sensor esta conectado en Z-STOP
```

**Tipo de sensor BLTouch o Inductivo**, existen diferentes tipos de sensores. Dependiendo del usado deberemos descomentar uno u otro.

```cpp
En el caso de un sensor Inductivo:
#define FIX_MOUNTED_PROBE // 3DWORK PROBE
En el caso de un sensor tipo BLTouch:
#define BLTOUCH // 3DWORK PROBE
```

**Offsets del sensor**, es muy importante decirle a Marlin donde se encuentra exactamente instalado nuestro sensor de nivelación... esto lo haremos definiendo los PROBE OFFSET.

El Offset es la distancia entre la punta del nozzle y la punta del sensor de nivelación que dependiendo de donde esté deberán ser valores positivos o negativos, puedes usar el siguiente gráfico como ejemplo de que valor poner en cada caso.

![](https://telegra.ph/file/e28dfb5d468ada9c88b4a.png)

En nuestro caso y como ejemplo nuestro sensor esta desplazado hacia la izquierda del nozzle en X 30mm y 10mm en Y ya que está esa distancia hacia el fondo de la impresora.

```cpp
#define NOZZLE_TO_PROBE_OFFSET { -30, 10, 0 } // 3DWORK PROBE
```

**PROBING\_MARGIN** \(en versiones anteriores **MIN\_PROBE\_EDGE**\), es muy normal que tengamos en nuestras impresoras un cristal como base de impresión aunque personalmente nos gustan más las láminas PEI magnéticas que aunque ser un poco mas susceptibles a marcarse con golpes tienen una excelente adherencia, no te limita el tamaño de la cama y retirar las piezas es extremadamente sencillo y rápido.

En cualquier caso ya tengamos cristal u otra superficie es aconsejable que el sensor este alejado de cualquier objeto que pueda molestarle durante el proceso de nivelación, ya sean grapas para fijar el cristal o el propio desajuste del inicio y final de cama que haga que el sensor nopueda medir.

Para esto usaremos este **PROBING\_MARGIN** en el cual le diremos cuantos mm se tiene que alejar de los bordes de la cama.

En nuestro caso usando un BLtouch y una plataforma PEI lo dejamos por defecto.

```cpp
#define PROBING_MARGIN 10 // 3DWORK PROBE
```

**Dirección de motores**, otro problema muy común a la hora de poner en marcha una impresora es que el cableado de las bobinas de nuestros motores no coincidan y se pueden dar dos casos:

1. que estén mezclados los conectores de nuestras bobinas, es muy fácil de identificar ya que el motor no girará correctamente e irá a trompicones. Para solucionarlo es aconsejable revisar el pinout de la antigua placa y/o del motor y compararlo con el de nuestra nueva placa para asegurarnos que coinciden
2. que el motor gire en la dirección incorrecta. En este caso de nuevo tenemos dos opciones... la primera y aconsejable es la de modificar en Marlin el giro del motor y la segunda es la de invertir el cableado de una de las bobinas del motor

En nuestro caso optamos por la primera modificando las siguientes lineas en Marlin cambiando el valor actual por el contrario... en este caso invertimos el eje Z.

```cpp
#define INVERT_X_DIR true
#define INVERT_Y_DIR true
#define INVERT_Z_DIR false // 3DWORK Invertimos el giro del eje Z
```

**Tamaño de impresión**, si hemos usado un Marlin genérico es importante verificar que las dimensiones de impresión son las correctas para nuestra máquina.

Para el tamaño de cama en el ejemplo usamos una Ender 3 a su máximo tamaño, es aconsejable verificar que puedes llegar físicamente a estas medidas y ajustarlo de ser necesario:

```cpp
#define X_BED_SIZE 235 // 3DWORK BED
#define Y_BED_SIZE 235 // 3DWORK BED
```

También definiremos el tamaño de nuestro eje Z:

```cpp
#define Z_MAX_POS 250 // 3DWORK BED
```

**Sensor de filamentos**, te aconsejamos revisar en la seccion /Mejoras/Sensor de filamentos de nuestro bot de ayuda en Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)  o en nuestra [guía](../../../otras-mejoras/sensor-de-filamentos.md) podréis encontrar más información detallada acerca de como instalar y configurar un sensor de filamentos.

```cpp
#define FILAMENT_RUNOUT_SENSOR // 3DWORK SENSOR FILAMENTOS
```

Al igual que con los finales de carrera es aconsejable revisar con un **M119** el estado del sensor de filamentos el cual tiene que aparecer **TRIGGERED con filamento** y **OPEN sin**. En el caso que parezca invertido deberemos de jugar con el valor de \(LOW/HIGH\):

```cpp
#define FIL_RUNOUT_STATE LOW // 3DWORK SENSOR FILAMENTOS
```

También podemos indicar la distancia de filamento que queremos desde que el sensor detecta el fallo el sensor en:

```cpp
#define FILAMENT_RUNOUT_DISTANCE_MM 25 // 3DWORK SENSOR FILAMENTOS
```

En el caso que nuestro sensor de filamento funcione por detección de flujo de filamento \(como el sensor de BigtreeTech Smart Filament Sensor\) deberemos habilitar lo siguiente:

```cpp
#define FILAMENT_MOTION_SENSOR // 3DWORK SENSOR FILAMENTOS
```

**Autonivelación**, tal como os comentábamos anteriormente en la sección /Nivelacion de nuestro bot [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)  teneis más información detallade de como activar [autonivelación UBL](../../../nivelacion/nivelado-ubl-unified-bed-leveling.md) \(la más avanzada y aconsejable para camas de cierto tamaño o con problemas\) y [MESH](../../../nivelacion/nivelado-manual-mesh.md) que permite tener una malla sin sensor haciendo el proceso manualmente.

En todo caso para este ejemplo habilitaremos BILINEAR.

```cpp
#define AUTO_BED_LEVELING_BILINEAR // 3DWORK PROBE 
```

Es importante decirle a Marlin que habilite la autonivelación \(G29\) después de un homing \(G28\) aunque en versiones actuales ya no es necesario pero aconsejamos habilitarlo.

```cpp
#define RESTORE_LEVELING_AFTER_G28 // 3DWORK PROBE
```

En el caso de BILINEAR especificaremos el número de puntos a realizar por malla en la siguiente parte de Marlin \(por defecto lleva 3 puntos por eje, 3x3=9 puntos de comprobación\).

```cpp
if EITHER(AUTO_BED_LEVELING_LINEAR, AUTO_BED_LEVELING_BILINEAR)
// Set the number of grid points per dimension.
#define GRID_MAX_POINTS_X 3 // 3DWORK PROBE
```

Otra función que por seguridad debemos de habilitar es la de que realice un homing en el centro de la cama, esto nos ayuda a dos cosas... la primera a **asegurar que el sensor tiene una superficie donde detectar el final de carrera** y el segundo **verificar visualmente que nuestros Offsets están de forma correcta ya que el sensor de nivelación tiene que quedar perfectamente posicionado en el centro de nuestra cama**.

```cpp
#define Z_SAFE_HOMING // 3DWORK PROBE
```

**EEPROM**, en la EEPROM Marlin guarda los valores de configuración de diferentes funciones para que estas puedan usarse por el sistema y ser cambiadas por el usuario sin necesidad de tener que modificar ficheros fuentes de Marlin, compilar y subir en cada cambio el firmware. Es muy aconsejable activarla y con nuestra Fysetc S6 no tendremos problemas de memoria.

```cpp
#define EEPROM_SETTINGS // 3DWORK EEPROM
#define EEPROM_CHITCHAT // 3DWORK EEPROM
```

**Nozzle Park**, en el ejemplo que estamos explicando hemos añadido un sensor de filamentos. Para que este funcione deberemos activar **NOZZLE\_PARK\_FEATURE** que básicamente nos permite "aparcar" el cabezal de impresión en una zona segura para realizar este cambio de filamentos aunque si no tenemos sensor de filamentos es muy aconsejable tenerlo activo ya que nos permite mejorar las funciones de pausado.

```cpp
#define NOZZLE_PARK_FEATURE // 3DWORK SENSOR FILAMENTOS
```

**SD**, si queremos que nuestro Marlin pueda leer de nuestra SD ya sea el de la placa o en una pantalla que sea compatible \(ojo que las Touch no lo son\) deberemos activar el soporte de SD.

```cpp
#define SDSUPPORT // 3DWORK SD
```

**Menu Homing Avanzado**, no es necesario pero si muy aconsejable en el caso de tealizar tests habilitar esta función que nos permite poder realizar un homing por separado de cada eje.

```cpp
#define INDIVIDUAL_AXIS_HOMING_MENU // 3DWORK MEJORAS
```

## **configuration\_adv.h**

![](../../../../../.gitbook/assets/image%20%28112%29.png)

Ya hemos finalizado la primera parte y ahora tendremos que pasar a la configuración de las opciones avanzadas.

**Scroll y nombres ficheros largos**, es interesante activar esta función para poder mostrar el nombre completo de los ficheros a imprimir.

Al igual que el anterior y para que funcione correctamente tenemos que activar la siguiente opción.

```cpp
#define LONG_FILENAME_HOST_SUPPORT // 3DWORK MEJORAS
#define SCROLL_LONG_FILENAMES // 3DWORK MEJORAS
```

**Conexión SD**, en configuration.h habilitamos el soporte para SD y en configuration\_adv.h definiremos de donde Marlin intentará usar esa SD.

Disponemos de tres opciones, LCD donde usaremos la de la pantalla Marlin... ONBOARD donde usaremos la de la placa y CUSTOM\_CABLE donde le indicaremos que usamos un lector externo conectado a la placa

Tal como comentamos previamente no todas las pantallas que dispongan de SD van a funcionar correctamente.

En nuestro caso usaremos la de la pantalla dado que la Fysetc S6 no cuenta con conector SD integrado en la placa.

```cpp
#define SDCARD_CONNECTION LCD // 3DWORK SD
```

**Babystepping**, una función casi imprescindible tener es baby stepping. Babystepping nos permite ajustar la altura de Z durante una impresión para ajustar de forma manual esta altura a nuestro gusto.

En nuestro ejemplo habilitaremos que se pueda activar con un doble click en el botón/encoder, que siempre este disponible y nos muestre mejoras gráficas en el ajuste para entender la dirección del ajuste desde la pantalla.

```cpp
#define BABYSTEPPING // 3DWORK MEJORAS
#define DOUBLECLICK_FOR_Z_BABYSTEPPING // 3DWORK MEJORAS
#define BABYSTEP_ALWAYS_AVAILABLE // 3DWORK MEJORAS
#define BABYSTEP_ZPROBE_GFX_OVERLAY // 3DWORK MEJORAS
```

**Reintentos en caso de fallo en la nivelación**, otra función muy útil es la de poder reintentar el proceso de homing en caso de fallo. En ocasiones nuestro sensor de nivelación puede tener un fallo puntual y mediante esta función nos permitirá volver a intentarlo un número finito de veces.

```cpp
#define G29_RETRY_AND_RECOVER // 3DWORK MEJORAS
#define G29_MAX_RETRIES 4 // 3DWORK MEJORAS
```

{% hint style="info" %}
**En caso de usar** [**nivelación UBL**](../../../nivelacion/nivelado-ubl-unified-bed-leveling.md#1-cambios-en-marlin-configuration-h) **esta opción no es compatible.**
{% endhint %}

**Soporte ARC**, una mejora sustancial en la calidad de nuestras impresiones y también cuando imprimimos a través de dispositivos serial como TFT, Octoprint, etc...  
Permite simplificar los comandos gcodes minimizando el numero de movimientos y haciendolos más fluidos evitando también un gran número de retracciones.

![](https://telegra.ph/file/c9fa979e6ddc3e9d6a65d.png)

Para usarlo debemos de configurar nuestro fileteador/slicer para que aplique estos cambios o en el caso de Octoprint podemos usar un plugin.  
En cuanto a los cambios en en Marlin:

```cpp
#define ARC_SUPPORT // 3DWORK MEJORAS
```

Es importante recalcar que en el caso de habilitar ARC para su uso es aconsejable, en el caso que también lo tengamos habilitado, la compatibilidad con S\_CURVE. Tenéis más información en la [guía de calibración](../../../../../octoprint/plugins-mejoras-octoprint/arc-welder.md).

**Pausado avanzado**, tal como comentamos anteriormente en este ejemplo usamos un sensor de filamentos. Para que funcione correctamente tenemos que habilitar **ADVANCED\_PAUSE\_FEATURE** que habilita el comando gcode **M600** para el cambio de filamentos.

Esto puedes hacerlo con o sin sensor de nivelación y requiere **NOZZLE PARK** para su correcto funcionamiento.

No entraremos en la configuración detallada ya que la tienes dentro de /Mejoras/Sensor Filamentos dentro de nuestro bot o directamente en la [guía](../../../otras-mejoras/sensor-de-filamentos.md).

```cpp
#define ADVANCED_PAUSE_FEATURE // 3DWORK SENSOR FILAMENTOS
```

**Configuración drivers UART/SPI**, con nuestros drivers en UART/SPI tenemos que configurar correctamente los parametros avanzados que encontraremos en la sección **HAS\_TRINAMIC\_CONFIG** 

Micropasos, aunque podemos configurar desde 16 a 256 micropasos no os aconsejamos para nada usar otro valor diferente al de 16. Con esos micropasos la definición que podríamos alcanzar mecánicamente es prácticamente imposible de alcanzar con impresoras normales además de que los drivers TMC ya realizan ellos mismo la interpolación de estos para siempre ofrecer la máxima fiabilidad y precisión.

Hablando de fiabilidad es otro aspecto importante a la hora de los micropasos, los motores de pasos que llevan nuestras impresoras pierden torque \(y pasos\) a altas revoluciones así que aumentar esta definición de pasos aumentará exponencialmente el porcentaje de fiabilidad y por lo tanto que nuestras piezas no salgan correctamente.

En el ejemplo siguiente podremos ver como tenemos 16 micropasos para nuestro eje X.

```cpp
#if AXIS_IS_TMC(X)
#define X_CURRENT 650 // (mA) RMS current
#define X_CURRENT_HOME X_CURRENT
#define X_MICROSTEPS 16 // 0..256
#define X_RSENSE 0.11
#define X_CHAIN_POS -1
#endif
```

Otro valor importante de los valores anteriores es el de **CURRENT** donde ajustaremos la corriente de nuestros motores. También puedes ajustarlos **desde el LCD desde Configuración/Avanzado/TMC** junto con otras opciones que explicaremos a continuación.

**STEALTHCHOP**, es importante habilitarlo si queremos que nuestros motores sean "silenciosos" ya que si no estarán funcionando en modo **SpreadCycle** que es el modo pontencia.

```cpp
#define STEALTHCHOP_XY // 3DWORK DRIVERS
#define STEALTHCHOP_Z // 3DWORK DRIVERS
#define STEALTHCHOP_E // 3DWORK DRIVERS
```

Aunque es raro algunos extrusores pueden trabajar mejor con el modo **StealthChop** desabilitado. Si es nuestro caso comentaremos la siguiente linea.

```cpp
//#define STEALTHCHOP_E // 3DWORK DRIVERS
```

**CHOPPER TIMMING**, es usado por StealthChop para la sincronización de señales a los motores. Este tiene que coincidir con el voltaje usado en nuestra impresora, en el caso de Ender 3 y normalemente en todas las impresoras a 24v.

```cpp
#define CHOPPER_TIMING CHOPPER_DEFAULT_24V // 3DWORK DRIVERS
```

**Monitor Status**, esta función nos permite que Marlin pueda controlar en detalle todos los parámetros de los drives para poder ajustar por ejemplo la temperatura de los mismos para evitar problemas más graves.

```cpp
#define MONITOR_DRIVER_STATUS // 3DWORK DRIVERS
```

**Hybrid Thresold**, otra interesante función es Hybrid Thresold que nos permite que Marlin cambie entre **StealthChop** \(silencio\) o **SpreadCycle** \(potencia\) dinámicamente en base a la velocidad de movimientos.

```cpp
#define HYBRID_THRESHOLD // 3DWORK DRIVERS
#define X_HYBRID_THRESHOLD 100 // [mm/s]
#define Y_HYBRID_THRESHOLD 100
#define Z_HYBRID_THRESHOLD 15
```

En nuestro caso usamos los valores indicados para una Ender que son 100mm/s para X Y y de 15 para Z aunque lo podemos ajustar a nuestras necesidades.

**TMC Debug**, en este caso es un requerimiento de **MONITOR\_DRIVER\_STATUS** que deberemos de habilitar y nos permitirá tener accesible el comando **M122** muy útil para obtener información avanzada del estado y configuración de nuestros drivers.

```cpp
#define TMC_DEBUG // 3DWORK DRIVERS
```

## Fiochero pins de nuestra FYSETC S6

A continuación tienes los ficheros pins que contienen las referencias para esta placa.

```cpp
.../Marlin/src/pins/stm32f4/pins_FYSETC_S6.h
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

![](../../../../../.gitbook/assets/image%20%28123%29.png)

* Pondremos nuestra placa Fysetc S6 en modo DFU
  * **S6 v1.2**, con la placa completamente apagada **dejaremos pulsado el botón BOOT0 y con el botón pulsado conectaremos mediante el cable USB la placa a nuestro ordenador**, esto hará que entre en el modo DFU. Una vez conectado y en modo DFU soltaremos el botón. 
  * **S6 v2**, con la placa completamente apagada **pondremos el jumper del conector BOOT0 a 3.3v y conectaremos mediante el cable USB la placa a nuestro ordenador**, esto hará que entre en el modo DFU. **RECORDAR!!! una vez finalizado el proceso de actualización de firmware quitar el jumper de BOOT0**

![](../../../../../.gitbook/assets/image%20%28121%29.png)

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

### configuration.h Fysetc MINI 12864

```cpp
# Definición de pantalla
#define FYSETC_MINI_12864_2_1 // Type A/B. Neopixel RGB Backlight
...
# Definición Neopixel de pantalla
// Support for Adafruit Neopixel LED driver
#define NEOPIXEL_LED
#if ENABLED(NEOPIXEL_LED)
#define NEOPIXEL_TYPE NEO_GRBW // NEO_GRBW / NEO_GRB - four/three channel driver type (defined in Adafruit_NeoPixel.h)
//#define NEOPIXEL_PIN 4 // LED driving pin
//#define NEOPIXEL_PIXELS 30 // Number of LEDs in the strip
//#define NEOPIXEL_IS_SEQUENTIAL // Sequential display for temperature change - LED by LED. Disable to change all LEDs at once.
//#define NEOPIXEL_BRIGHTNESS 127 // Initial brightness (0-255)
#define NEOPIXEL_STARTUP_TEST // Cycle through colors at startup
// Use a single Neopixel LED for static (background) lighting
// #define NEOPIXEL_BKGD_LED_INDEX 0 // Index of the LED to use - Do not 
...
// Use a single Neopixel LED for static (background) lighting
#define NEOPIXEL_BKGD_LED_INDEX 0 // Index of the LED to use - Do not change this setting
#define NEOPIXEL_BKGD_COLOR { 0, 255, 0, 0 } // R, G, B, W - We have set green background color as standard
#endif
```

### configuration\_adv.h Fysetc MINI 12864

```cpp
 # Habilitar menus de control LED
 #define LED_CONTROL_MENU
```

### conditionals\_post.h Fysetc MINI 12864

Estas modificaciones se deben realizar si tu pantalla no aparecen letras de una forma clara/definica \(puedes intuirlas viendo en diagonal\)

```cpp
#elif ENABLED(FYSETC_MINI_12864)
#define _LCD_CONTRAST_MIN 255
#define _LCD_CONTRAST_INIT 255

.../Marlin/src/inc/Conditionals_post.h
```



## Pues ya hemos terminado!!! 

ahora solamente queda compilar, ajustar cualquier error que nos reporte el compilado y meterlo en nuestra placa!!!

Como siempre os aconsejamos verificar inicialemente la impresora de la siguiente manera:

* revisar los finales de carrera siempre que sea posible usando un terminal \(Pronterface por ejemplo\) con el comando M119
* revisar individualmente el homing de cada uno de los ejes de movimiento en el siguiente orden X Y Z E y en caso necesario invertir el giro del motor
* revisar que nuestros thermistores reportan correctamente la temperatura
* revisar que nuestros calentadores de cama y de hotend funcionan correctamente
* realizar la guia de Calibración Inicial que podéis encontrar en nuestro bot en el apartado /Calibracion

