---
description: >-
  By JJR @ https://3dwork.io/ - Invitame a un cafe si te fue de ayuda
  paypal.me/alienboyxp
---

# SKR MINI E3 Turbo

![](https://telegra.ph/file/85fe91922a41fed2626a4.png)

La SKR Mini E3 Turbo no ha cambiado mucho con respecto a la SKR Mini E3 v2 pero si añadió interesantes mejoras en la extrusión añadiendo soporte de un nuevo hotend o un procesador mucho más potente que en anteriores MINI, en cualquier caso podemos encontrar algunas diferencias en la orientación de los cables.

Recordamos que todas las placas de la gama E3 están especialmente diseñadas para el uso o reemplazo de la electrónica de una impresora Creality Ender 3 aunque puede ser usada en otras impresoras sin problema atendiendo al cableado y características específicas de cada una.

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de SKR @SKR\_board\_32bits.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

## **Diagramas SKR E3 TURBO**

![](https://telegra.ph/file/00b3e98db0c398210952e.jpg)

## **Conexiones de alimentación**Prestar atención a la polaridad

![](https://telegra.ph/file/c4f4716827eff0d52876c.png)

Como otras placas SKR Mini E3 el conector de alimentación se encuentra en la parte izquierda de la placa y acepta alimentación de 12v o 24v.Detalle del conector de alimentación de la placa

![](https://telegra.ph/file/a506852efc85f51a693f2.png)

**Conector cama caliente**, esta placa cuenta con el mismo MOSFET usado en la SKR Mini E3 V2, un WSK220N04. Es crítico fijarse en la polaridad del mismo si conectamos un MOSFET externo o una cama con polaridad en especial estos que disponen de un led incorporado.Conector alimentación cama caliente

![](https://telegra.ph/file/cec9b5a8ca35b4661e9f1.png)

**Calentador hotend**, como en la cama caliente normalemente no es necesario fijarse en la polaridad por norma general ya que estos componentes no suelen tener polaridad.

Una importante diferencia de esta placa con respecto a sus hermanas es que cuenta con una segunda salida para otro hotend además de incorporar conectores rápidos para ellos lo cual facilita el uso de los mismos.Conector hotend

![](https://telegra.ph/file/986e0a9c575f8e7eb9dd8.png)

La orientación de los **thermistores** normalmente no es importante ya que estos no suelen tener polaridad. En este caso para esta placa contamos para un conector para la cama caliente \(TB\) y dos para ambos calentadores de hotend \(TH0 y TH1\).

Es aconsejable usar conectores JST-XH 2.54mm como los de la siguiente imagen ya que evitan que se produzcan desconexiones y se minimizan los falsos contactos.

![](https://telegra.ph/file/b2d39bdddb033cfd7fb7e.png)

Esta placa cuenta con cinco **drivers** TMC2209 integrados y pre-configurados en el firmware que lleva instalado por defecto \(recordemos que para una Ender 3\) en modo UART.

Al disponer estos drivers podemos aprovechar la funcionalidad de **Sensorless/Stallguard** que nos permite eliminar los sensores de final de carrera físicos, es aconsejable su uso solamente en los ejes X e Y.

Normalmente en otras placas para usar esta funcionalidad es necesario el pin DIAG siendo necesario quitarlo/doblarlo para usar finales de carrera físicos. En esta placa como en la SKR Mini E3 v2 o SKR 1.3 cuenta con unos pines para poder hacer esta función de una forma cómoda.Jumpers DIAG para la gestión de sensorless o finales de carreras físicos

![](https://telegra.ph/file/00fe01ee7c5d66aafb175.png)

Conectores de final de carrera están preparados para ser usados en una Ender 3 por lo que si lo usamos para otra impresora deberemos fijarnos, sobretodo si disponemos finales de carrera de 3 cables, en conectar en el orden correcto GND y SGN.Conector final de carrera

![](https://telegra.ph/file/34a102dba7122964c7ca4.png)

**Conector LCD**, al estar preparado para impresoras Ender 3 la placa cuenta con un solo conector de 10 pines para el LCD que irá conectado entre el EXP1 de la placa y el EXP3 del LCD.

En este aspecto esta placa estará limitado el número de pantallas compatibles a aquellas que usen conectores EXP3 o deberemos de modificar el fichero pins y/o realizar un adaptador para nuestro LCD no compatible.Conector EXP3 pantallas LCD Ender 3

![](https://telegra.ph/file/05e656e06301a03de263e.png)

**Conector TFT**, este tipo de pantallas normalmente táctiles son un "ente" externo a Marlin que es el sistema operativo que gestiona nuestra placa y se comunica con el mediante una conexión serial a través del conector TFT.

Las pantallas SKR están preparadas con el correcto orden en el cableado \(RESET-RX-TX-GND-+5V\) pero podemos usar cualquier otra pantalla serial prestando atención al pineado de la misma y el de nuestra placa para adaptarlo.

![](https://telegra.ph/file/0e42f09e1916c830af8cb.png)

**Ventiladores**, otra diferencia de esta placa con respecto a la SKR Mini E3 v2 es que dispone de la posibilidad de un segundo extrusor añadiendo también un segundo conector de ventilador gestionable.

En este caso contaremos con FAN0 que será la salida de ventilador de capa para el primer hotend y FAN1 que lo hará para el segundo hotend. En cualquier caso contamos con una tercera salida de ventilador \(FAN2\) que no es controlable como las anteriores y normalmente es el usado para refrigeracion del hotend.

Recordar de nuevo que en este caso es muy muy importante la polaridad al conectar los ventiladores a la placa.

![](https://telegra.ph/file/ba4b707e23a42d93e280c.png)

También disponemos de otra salida de corriente para conectar los ventiladores de la electrónica que es más que aconsejable para mantener los drivers y la placa en una temperatura de trabajo adecuada para prevenir problemas.

![](https://telegra.ph/file/302197282037bf9850447.png)

**Sensor de nivelación automática BLTouch o similares**, esta placa cuenta con un conector dedicado para este tipo de sensor de nivelación de los más versátiles y fiables que podemos instalar hoy en día.Cableado sensor BLTouch originalCableado sensor tipo BLTouch clon

![](https://telegra.ph/file/0fbb8132d560f17f3a11e.png)

![](https://telegra.ph/file/ab63d2f34e9f39227d1bf.png)

Dependiendo de la configuración que realicemos en Marlin la parte de final de carrera del sensor \(cables negro y blanco normalmente\) pueden ser instalados tanto en el puerto dedicado del Z-PROBE o en el final de carrera Z-STOP.Conexión alternativa del sensor de nivelación dependiendo de nuestra configuración de Marlin

![](https://telegra.ph/file/957b525f418e2760f6935.png)

**Conector Neopixel** dedicado, contamos con una guía dedicada a este tema en Mejoras así que solamente comentar que disponemos de un conector dedicado para estos menesteres.

## **Instalación del firmware**

Como ya hemos comentado esta placa está por defecto pensada para usar en una Ender 3 y el firmware preinstalado acorde a esta máquina. En el caso que queráis usarla para otra máquina diferente os aconsejamos pasar por la sección Marlin para poder "cocinar" vuestro propio Marlin ya sea usando como base los preconfigurados de vuestra impresora o quizás mejor usando los preconfigurados para una Ender 3 con esta placa y adaptando las características de nuestra máquina.

Si es vuestro caso y no os queréis complicar compilando vuestra propia versión podréis obtener las últimas versiones actualizadas en el Github de BigtreeTech en el siguiente link [https://github.com/bigtreetech/BIGTREETECH-SKR-E3-Turbo/tree/master/firmware](https://github.com/bigtreetech/BIGTREETECH-SKR-E3-Turbo/tree/master/firmware) .

En la siguiente captura podéis ver los ficheros que podéis descargar para directamente ponerlo en la SD y actualizar.

![](https://telegra.ph/file/7d2190ae13fd73fe36806.png)

> **NOTA**: Los nombres de los firmware tal como se descargan tienen que ser renombrados a firmware.bin si no tiene ya ese nombre antes de ser copiadas a la SD para actualizar

**firmware.bin**: Firmware original para Ender 3, no soporta BLTouch

**firmware-bltouch.bin**: Firmware para Ender 3 con soporte para conexión normal a BLTouch

**firmware-bltouch-for-z-homing.bin**: Firmware para Ender 3 con soporte BLTouch usado para Z-homming.

## Configuración de Marlin para SKR MINI E3 TURBO

![](https://telegra.ph/file/8d2608160b7b5e0e41374.png)

Tal como comentábamos en el punto anterior os aconsejamos seguir nuestra guía para "cocinar" vuestro propio Marlin que tenemos en la seccion /Marlin de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot

### **platformio.ini**

![](https://telegra.ph/file/c17de435c3b0bd853d282.png)

En este fichero deberemos indicar el chipset que tiene nuestra placa por lo que deveremos buscar en el inicio del fichero el valor "env\_default" y cambiarlo por el siguiente:

> env\_default = LPC1769

### **configuration.h**

![](https://telegra.ph/file/d53e4782e6a13a490349c.png)

Comenzaremos por configurar correctamente los **SERIAL** para esta máquina algo clave para el correcto funcionamiento de nuestra conexión TFT, USB o WIFI.

> \#define SERIAL\_PORT -1 // USB  
> \#define SERIAL\_PORT\_2 0 // TFT

**BAUDRATE** nos ayuda a definir la velocidad de la comunicación por el puerto **SERIAL** lo normal es usar 115200 o 250000.

> \#define BAUDRATE 115200

Ahora definiremos nuestro MOTHERBOARD el cual al igual que hicimos el platformio.ini nos indica el tipo de chipset que lleva nuestra placa y que permite a Marlin usar una configuración de pines u otra.

> \#define MOTHERBOARD BOARD\_BTT\_SKR\_E3\_TURBO

Definiremos el tipo de filamento que usa nuestra impresora que básicamente indica el diámetro del mismo. Normalmente impresoras normales usan filamento del 1.75 aunque otra medida más o menos estándard es 3.

> \#define DEFAULT\_NOMINAL\_FILAMENT\_DIA 1.75

**Thermistores**, en estas variables indicaremos que thermistores tenemos, os aconsejamos que identifiquéis cual es el vuestro o en su defecto uséis uno de valor 1 que suele ser el mas normal.

Es importatísimo indicar el tipo de thermistor correcto para que las medidas que Marlin recibe sean las correctas para evitar problemas de extrusión y/o fallos de lecturas.

En el caso que queramos desactivar o activar la cama caliente comentaremos o activaremos el thermistor de la cama, de esta forma Marlin sabe si tiene o no cama caliente.

En nuestro ejemplo y para esta placa usaremos un hotend y una cama caliente con un sensor de temperatura estándard.

> \#define TEMP\_SENSOR\_0 1  
> \#define TEMP\_SENSOR\_BED 1

**PID**, otra parte muy importante es el PID del que podéis obtener más información detallada en la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot.

Básicamente el PID ayuda a que Marlin controle de forma adecuada la temperatura sin tener grandes fluctuaciones.

En nuestro caso habilitaremos la calibración de estos PID ya que dependen de muchas variables y los que vienen por defecto no siempre son la mejor opción, podéis obtener más información detallada en la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot para ver el proceso de calibración del PID.

> \#define PIDTEMP  
> \#define PIDTEMPBED

**ENDSTOPS o finales de carrera**, es importante verificar cuando realizamos un cambio de electrónica y encendemos por primera vez que la lógica de los finales de carrera es la correcta usando el comando M119 desde un terminal como Pronterface... esto es OPEN en estado normal y TRIGGERED cuando están pulsados.

En este ejemplo y dado que vamos a montar nuestra placa con BLTouch necesitaremos cambiar el endstop del probe para que se ajuste a este sensor. En todo caso podéis obtener más información detallada en la seccion /Nivelacion de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot teneis una guia dedicada a la instalación de este tipo de sensor de nivelación.

> \#define Z\_MIN\_PROBE\_ENDSTOP\_INVERTING true

Drivers, tal como hemos comentado en el inicio de la guía esta placa lleva integrados unos drivers TMC2209 los cuales definiremos en Marlin de la siguiente forma:

> \#define X\_DRIVER\_TYPE TMC2209  
> \#define Y\_DRIVER\_TYPE TMC2209  
> \#define Z\_DRIVER\_TYPE TMC2209  
> //\#define X2\_DRIVER\_TYPE TMC2209  
> //\#define Y2\_DRIVER\_TYPE A4988  
> //\#define Z2\_DRIVER\_TYPE A4988  
> //\#define Z3\_DRIVER\_TYPE A4988  
> //\#define Z4\_DRIVER\_TYPE A4988  
> \#define E0\_DRIVER\_TYPE TMC2209  
> //\#define E1\_DRIVER\_TYPE A4988

**STEPS o pasos de motor**, tos valores indicaremos a Marlin los pasos de motor para realizar movimientos. En este ejemplo usaremos los que una impresora Ender lleva por defecto, en todo caso de nuevo os remitimos a la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot para ver el para calibrar estos pasos para nuestro extrusor y ejes de movimiento.

> \#define DEFAULT\_AXIS\_STEPS\_PER\_UNIT { 80.00, 80.00, 400.00, 93 }  
> \#pasos para Ender3 ejes X, Y, Z,Extrusor

S\_CURVE\_ACCELERATION, esta funcionalidad ofrece una gran mejora en la calidad de impresion ya que suaviza la aceleración o deceleracion cuando el cabezal de impresión está en movimiento.

Nota importate es que dependiendo de nuestra versión de Marlin y si usamos Linear Advance no son compatibles. En la seccion /Calibracion/Calibracion Inicial de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot podréis encontrar más información detallada de Linear Advanced.

> \#define S\_CURVE\_ACCELERATION

**PROBE** activando nuestro sensor de nivelación, aunque como ya hemos comentado hay una guía detallada de como instalar un sensor BLTouch vamos a realizar una revisión de las partes más importantes a tener en cuenta en el caso que querámos instalar un sensor de nivelación, algo muy aconsejable para asegurarnos que nuestras impresiones siempre tienen una primera capa lo más perfecta posible.

Comenzaremos por indicar donde está conectado nuestro sensor de nivelación, en este caso y el mas normal/aconsejable es conectarlo en el final de carrera de Z Z-STOP, también contamos con la opción de usar el puerto dedicado que cuenta la placa, en nuestro caso usaremos el Z-STOP para el ejemplo.

> \#define Z\_MIN\_PROBE\_USES\_Z\_MIN\_ENDSTOP\_PIN  
> \# indicamos a Marlin que nuestro sensor esta conectado en Z-STOP  
> //\#define Z\_MIN\_PROBE\_PIN P1\_22  
> \# comentamos esta linea al conectar el sensor en Z-STOP

**Tipo de sensor BLTouch o Inductivo**, existen diferentes tipos de sensores. Dependiendo del usado deberemos descomentar uno u otro.

> En el caso de un sensor Inductivo:  
> \#define FIX\_MOUNTED\_PROBE  
> En el caso de un sensor tipo BLTouch:  
> \#define BLTOUCH

**Offsets del sensor**, es muy importante decirle a Marlin donde se encuentra exactamente instalado nuestro sensor de nivelación... esto lo haremos definiendo los PROBE OFFSET.

El Offset es la distancia entre la punta del nozzle y la punta del sensor de nivelación que dependiendo de donde esté deberán ser valores positivos o negativos, puedes usar el siguiente gráfico como ejemplo de que valor poner en cada caso.

![](https://telegra.ph/file/e28dfb5d468ada9c88b4a.png)

En nuestro caso y como ejemplo nuestro sensor esta desplazado hacia la izquierda del nozzle en X 30mm y 10mm en Y ya que está esa distancia hacia el fondo de la impresora.

> \#define NOZZLE\_TO\_PROBE\_OFFSET { -30, 10, 0 }

**PROBING\_MARGIN** \(en versiones anteriores MIN\_PROBE\_EDGE\), es muy normal que tengamos en nuestras impresoras un cristal como base de impresión aunque personalmente nos gustan más las láminas PEI magnéticas que aunque ser un poco mas susceptibles a marcarse con golpes tienen una excelente adherencia, no te limita el tamaño de la cama y retirar las piezas es extremadamente sencillo y rápido.

En cualquier caso ya tengamos cristal u otra superficie es aconsejable que el sensor este alejado de cualquier objeto que pueda molestarle durante el proceso de nivelación, ya sean grapas para fijar el cristal o el propio desajuste del inicio y final de cama que haga que el sensor nopueda medir.

Para esto usaremos este PROBING\_MARGIN en el cual le diremos cuantos mm se tiene que alejar de los bordes de la cama.

En nuestro caso usando un BLtouch y una plataforma PEI lo dejámos por defecto.

> \#define PROBING\_MARGIN 10

**Dirección de motores**, otro problema muy común a la hora de poner en marcha una impresora es que el cableado de las bobinas de nuestros motores no coincidan y se pueden dar dos casos:

1\) que estén mezclados los conectores de nuestras bobinas, es muy fácil de identificar ya que el motor no girará correctamente e irá a trompicones. Para solucionarlo es aconsejable revisar el pinout de la antigua placa y/o del motor y compararlo con el de nuestra nueva placa para asegurarnos que coinciden

2\) que el motor gire en la dirección incorrecta. En este caso de nuevo tenemos dos opciones... la primera y aconsejable es la de modificar en Marlin el giro del motor y la segunda es la de invertir el cableado de una de las bobinas del motor

En nuestro caso optamos por la primera modificando las siguientes lineas en Marlin cambiando el valor actual por el contrario... en este caso invertimos el eje Z.

> \#define INVERT\_X\_DIR true  
> \#define INVERT\_Y\_DIR true  
> \#define INVERT\_Z\_DIR false

**Tamaño de impresión**, si hemos usado un Marlin genérico es importante verificar que las dimensiones de impresión son las correctas para nuestra máquina.

Para el tamaño de cama en el ejemplo usamos una Ender 3 a su máximo tamaño, es aconsejable verificar que puedes llegar físicamente a estas medidas y ajustarlo de ser necesario:

> \#define X\_BED\_SIZE 235  
> \#define Y\_BED\_SIZE 235

También definiremos el tamaño de nuestro eje Z:

> \#define Z\_MAX\_POS 250

**Sensor de filamentos**, te aconsejamos revisar en la seccion /Mejoras/Sensor de filamentos de nuestro bot de ayuda en Telegram @ThreeDWorkHelpBot podréis encontrar más información detallada acerca de como instalar y configurar un sensor de filamentos

> \#define FILAMENT\_RUNOUT\_SENSOR

Al igual que con los finales de carrera es aconsejable revisar con un M119 el estado del sensor de filamentos el cual tiene que aparecer TRIGGERED con filamento y OPEN sin. En el caso que parezca invertido deberemos de jugar con el valor de \(LOW/HIGH\):

> \#define FIL\_RUNOUT\_STATE LOW

También podemos indicar la distancia de filamento que queremos desde que el sensor detecta el fallo el sensor en:

> \#define FILAMENT\_RUNOUT\_DISTANCE\_MM 25

También en el caso que nuestro sensor de filamento funcione por detección de flujo de filamento \(como el sensor de BigtreeTech Smart Filament Sensor\) deberemos habilitar lo siguiente:

> \#define FILAMENT\_MOTION\_SENSOR

**Autonivelación**, tal como os comentábamos anteriormente en la sección /Nivelacion de nuestro bot tenés más información detallade de como activar autonivelación UBL \(la más avanzada y aconsejable para camas de cierto tamaño o con problemas\) y MESH que permite tener una malla sin sensor haciendo el proceso manualmente.

En todo caso para este ejemplo habilitaremos BILINEAR.

> \#define AUTO\_BED\_LEVELING\_BILINEAR

Es importante decirle a Marlin que habilite la autonivelación \(G29\) después de un homing \(G28\).

> \#define RESTORE\_LEVELING\_AFTER\_G28

En el caso de BILINEAR le diremos los puntos a realizar por malla en la siguiente parte de Marlin \(por defecto lleva 3 puntos por eje, 3x3=9 puntos de comprobación\).

> if EITHER\(AUTO\_BED\_LEVELING\_LINEAR, AUTO\_BED\_LEVELING\_BILINEAR\)  
>  // Set the number of grid points per dimension.  
>  \#define GRID\_MAX\_POINTS\_X 3

Otra función que por seguridad debemos de habilitar es la de que realice un homing en el centro de la cama, esto nos ayuda a dos cosas... la primera a asegurar que el sensor tiene una superficie donde detectar el final de carrera y el segundo verificar visualmente que nuestros Offsets están de forma correcta ya que el sensor de nivelación tiene que quedar perfectamente posicionado en el centro de nuestra cama.

> \#define Z\_SAFE\_HOMING

**EEPROM**, una de las mejoras de esta placa vs su hermana menor es que incluye una EEPROM dedicada y estos valores no los guarda en la memoria del procesados permitiéndonos tener más funciones de Marlin activadas.

> \#define EEPROM\_SETTINGS  
> \#define EEPROM\_CHITCHAT

**Nozzle Park**, en el ejemplo que estamos explicando hemos añadido un sensor de filamentos. Para que este funcione deberemos activar NOZZLE\_PARK\_FEATURE que básicamente nos permite "aparcar" el cabezal de impresión en una zona segura para realizar este cambio de filamentos.

> \#define NOZZLE\_PARK\_FEATURE

**SD**, si queremos que nuestro Marlin pueda leer de nuestra SD ya sea el de la placa o en una pantalla que sea compatible \(ojo que las Touch no lo son\) deberemos activar el soporte de SD.

> \#define SDSUPPORT

**Menu Homing**, no es necesario pero si muy aconsejable en el caso de tealizar tests habilitar esta función que nos permite poder realizar un homing por separado de cada eje.

> \#define INDIVIDUAL\_AXIS\_HOMING\_MENU

**LCD**, or recordamos que estas placas están destinadas a su uso en Ender 3 por lo que su compatibilidad con pantallas LCD es limitada y sería necesario hacer adaptadores y cambios en Marlin para el uso de otras pantallas.

> \#define CR10\_STOCKDISPLAY

En el caso de ser una Ender 3 v2 deberemos habilitar una diferente aunque a tener en cuenta que esta pantalla es una pantalla serial y deberiamos conectarla al puerto TFT.

> \#define DWIN\_CREALITY\_LCD

### **configuration\_adv.h**

![](https://telegra.ph/file/8f81ecb0d4caaee527445.png)

Ya hemos finalizado la primera parte y ahora tendremos que pasar a la configuración de las opciones avanzadas.

**Scroll y nombres ficheros largos**, es interesante activar esta función para poder mostrar el nombre completo de los ficheros a imprimir.

Al igual que el anterior y para que funcione correctamente tenemos que activar la siguiente opción.

> \#define LONG\_FILENAME\_HOST\_SUPPORT  
> \#define SCROLL\_LONG\_FILENAMES

**Conexión SD**, en configuration.h habilitamos el soporte para SD y en configuration\_adv.h definiremos de donde Marlin intentará usar esa SD.

Disponemos de tres opciones, LCD donde usaremos la de la pantalla Marlin... ONBOARD donde usaremos la de la placa y CUSTOM\_CABLE donde le indicaremos que usamos un lector externo conectado a la placa

Tal como comentamos previamente no todas las pantallas que dispongan de SD van a funcionar correctamente.

En nuestro caso usaremos la de la placa.

> \#define SDCARD\_CONNECTION ONBOARD

**Babystepping**, una función casi imprescindible tener es baby stepping. Babystepping nos permite ajustar la altura de Z durante una impresión para ajustar de forma manual esta altura a nuestro gusto.

En nuestro ejemplo habilitaremos que se pueda activar con un doble click en el botón/encoder, que siempre este disponible y nos muestre mejoras gráficas en el ajuste para entender la dirección del ajuste desde la pantalla.

> \#define BABYSTEPPING  
> \#define DOUBLECLICK\_FOR\_Z\_BABYSTEPPING  
> \#define BABYSTEP\_ALWAYS\_AVAILABLE  
> \#define BABYSTEP\_ZPROBE\_GFX\_OVERLAY

**Reintentos en caso de fallo en la nivelación**, otra función muy útil es la de poder reintentar el proceso de homing en caso de fallo. En ocasiones nuestro sensor de nivelación puede tener un fallo puntual y mediante esta función nos permitirá volver a intentarlo un número finito de veces.

> \#define G29\_RETRY\_AND\_RECOVER  
> \#define G29\_MAX\_RETRIES 4

**Soporte ARC**, una mejora sustancial en la calidad de nuestras impresiones y también cuando imprimimos a través de dispositivos serial como TFT, Octoprint, etc...  
Permite simplificar los comandos gcodes minimizando el numero de movimientos y haciendolos más fluidos evitando también un gran número de retracciones.

![](https://telegra.ph/file/c9fa979e6ddc3e9d6a65d.png)

Para usarlo debemos de configurar nuestro fileteador/slicer para que aplique estos cambios o en el caso de Octoprint podemos usar un plugin.  
En cuanto a los cambios en en Marlin:

> \#define ARC\_SUPPORT

Es importante recalcar que en el caso de habilitar ARC para su uso es aconsejable, en el caso que también lo tengamos habilitado, la compatibilidad con S\_CURVE.

**Pausado avanzado**, tal como comentamos anteriormente en este ejemplo usamos un sensor de filamentos. Para que funcione correctamente tenemos que habilitar ADVANCED\_PAUSE\_FEATURE que habilita el comando gcode M600 para el cambio de filamentos.

Esto puedes hacerlo con o sin sensor de nivelación y requiere NOZZLE PARK para su correcto funcionamiento.

No entraremos en la configuración detallada ya que la tienes dentro de /Mejoras/Sensor Filamentos dentro de nuestro bot.

> \#define ADVANCED\_PAUSE\_FEATURE

**Configuración drivers TMC2209 UART**, nuestra placa lleva integrados unos TMC2209 y tenemos que configurar correctamente los parametros avanzados que encontraremos en la sección HAS\_TRINAMIC\_CONFIG de configuration\_adv.h

Micropasos, aunque podemos configurar desde 16 a 256 micropasos no os aconsejamos para nada usar otro valor diferente al de 16. Con esos micropasos la definición que podríamos alcanzar mecánicamente es prácticamente imposible de alcanzar con impresoras normales además de que los drivers TMC2209 ya realizan ellos mismo la interpolación de estos para siempre ofrecer la máxima fiabilidad y precisión.

Hablando de fiabilidad es otro aspecto importante a la hora de los micropasos, los motores de pasos que llevan nuestras impresoras pierden torque \(y pasos\) a altas revoluciones así que aumentar esta definición de pasos aumentará exponencialmente el porcentaje de fiabilidad y por lo tanto que nuestras piezas no salgan correctamente.

En el ejemplo siguiente podremos ver como tenemos 16 micropasos para nuestro eje X.

> \#if AXIS\_IS\_TMC\(X\)  
> \#define X\_CURRENT 650 // \(mA\) RMS current  
> \#define X\_CURRENT\_HOME X\_CURRENT  
> \#define X\_MICROSTEPS 16 // 0..256  
> \#define X\_RSENSE 0.11  
> \#define X\_CHAIN\_POS -1  
> \#endif

Otro valor importante de los valores anteriores es el de CURRENT donde ajustaremos la corriente de nuestros motores. También puedes ajustarlos desde el LCD desde Configuración/Avanzado/TMC junto con otras opciones que explicaremos a continuación.

**STEALTHCHOP**, es importante habilitarlo si queremos que nuestros motores sean "silenciosos" ya que si no estarán funcionando en modo SpreadCycle que es el modo pontencia.

> \#define STEALTHCHOP\_XY  
> \#define STEALTHCHOP\_Z  
> \#define STEALTHCHOP\_E

Aunque es raro algunos extrusores pueden trabajar mejor con el modo StealthChop desabilitado. Si es nuestro caso comentaremos la siguiente linea.

> //\#define STEALTHCHOP\_E

**CHOPPER TIMMING**, es usado por StealthChop para la sincronización de señales a los motores. Este tiene que coincidir con el voltaje usado en nuestra impresora, en el caso de Ender 3 y normalemente en todas las impresoras a 24v.

> \#define CHOPPER\_TIMING CHOPPER\_DEFAULT\_24V

**Monitor Status**, esta función nos permite que Marlin pueda controlar en detalle todos los parámetros de los drives para poder ajustar por ejemplo la temperatura de los mismos para evitar problemas más graves.

> \#define MONITOR\_DRIVER\_STATUS

**Hybrid Thresold**, otra interesante función es Hybrid Thresold que nos permite que Marlin cambie entre StealthChop \(silencio\) o SpreadCycle \(potencia\) dinámicamente en base a la velocidad de movimientos.

> \#define HYBRID\_THRESHOLD  
> \#define X\_HYBRID\_THRESHOLD 100 // \[mm/s\]  
> \#define Y\_HYBRID\_THRESHOLD 100  
> \#define Z\_HYBRID\_THRESHOLD 15

En nuestro caso usamos los valores indicados para una Ender que son 100mm/s para X Y y de 15 para Z aunque lo podemos ajustar a nuestras necesidades.

**TMC Debug**, en este caso es un requerimiento de MONITOR\_DRIVER\_STATUS que deberemos de habilitar.

> \#define TMC\_DEBUG

### **Fichero pins SKR MINI E3 TURBO**

Al igual que otras placas SKR sus pines se definen con dependencias, a continuación tienes los ficheros pins que contienen las referencias para esta placa.

> Marlin/src/pins/lpc1769/pins\_BTT\_SKR\_E3\_TURBO.h

### **Error escritura en EEPROM SKR MINI E3 TURBO**
Dependiendo de la versión de Marlin la definición de la EEPROM puede ocasionar errores en la escritura de esta. Si tú versión está afectada por este problema deberías modificar lo siguiente:

Marlin/src/HAL/LPC1768/HAL.h
//#define I2C_ADDRESS(A) uint8_t((A) << 1)

#### Pues ya hemos terminado!!! ahora solamente queda compilar, ajustar cualquier error que nos reporte el compilado y meterlo en nuestra placa!!! <a id="Pues-ya-hemos-terminado!!!-ahora-solamente-queda-compilar,-ajustar-cualquier-error-que-nos-reporte-el-compilado-y-meterlo-en-nuestra-placa!!!"></a>

Como siempre os aconsejamos verificar inicialemente la impresora de la siguiente manera:

* revisar los finales de carrera siempre que sea posible usando un terminal \(Pronterface por ejemplo\) con el comando M119
* revisar individualmente el homing de cada uno de los ejes de movimiento en el siguiente orden X Y Z E y en caso necesario invertir el giro del motor
* revisar que nuestros thermistores reportan correctamente la temperatura
* revisar que nuestros calentadores de cama y de hotend funcionan correctamente
* realizar la guia de Calibración Inicial que podéis encontrar en nuestro bot en el apartado /Calibracion

