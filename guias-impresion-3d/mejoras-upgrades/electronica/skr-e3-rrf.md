---
description: >-
  Modelo especialmente diseñado para Ender 3 con la versatilidad de poder usarse
  con firmwares Marlin y RRF (Duet).
---

# SKR E3 RRF

![](../../../.gitbook/assets/image%20%2844%29.png)

Al igual que la [SKR E3 Turbo](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr_mini_e3_turbo) esta SKR E3 RRF tiene algunos cambios en la orientación de cables con respecto a su hermana [SKR MINI E3 v2](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr-mini-e3-v2). Especialmente en la zona de alimentación.

Esta placa esta diseñada como reemplazo directo para impresoras Ender 3 o similares.

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de SKR [@SKR\_board\_32bits](https://t.me/SKR_board_32bits).‌

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## Diagramas SKR E3 RRF

![](../../../.gitbook/assets/image%20%2858%29.png)

![](../../../.gitbook/assets/image%20%2864%29.png)

## Conectores SKR E3 RRF

### ESP-07S - Módulo Wifi

![](../../../.gitbook/assets/image%20%2843%29.png)

Una de las novedades de esta nueva placa es la inclusión de un módulo Wifi integrado, principalmente para la gestión de la impresora en modo RRF\(Duet\), al que es importante conectar la mini antena que incluye.

### Jumper selección de fuente alimentación

Al contrario de sus hermanas [SKR E3 Turbo](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr_mini_e3_turbo) o [SKR MINI E3 v2](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr-mini-e3-v2). En este modelo han incluido de nuevo el jumper para seleccionar la alimentación a usar entre USB o Fuente que evita desagradables problemas que pueden ocurrir al mezclar alimentaciones diferentes.

![](../../../.gitbook/assets/image%20%2856%29.png)

Es importante para usar la placa con fuente de alimentación de la impresora que el jumper este colocado en el pin E.

![](../../../.gitbook/assets/image%20%2846%29.png)

En el caso de alimentar por USB deveremos de tener colocado el jumper en el pin VUSB. 

{% hint style="info" %}
**Obvia decir que dependiendo la alimentación el jumper debe de poner en uno u otro pin, no en los dos a la vez o en ninguno de ellos.**
{% endhint %}

### Conectores de alimentación de la placa

![](../../../.gitbook/assets/image%20%2853%29.png)

Tal como hemos comentado el orden de algunos cables ha cambiado así que debemos, en especial en estos, prestar mucha atención a la polaridad de estos conectores.

Una mejora introducida en este model o es que ahora disponemos de un conector de facil extracción para el calentador de extrusión que facilita mucho el proceso de manipulacion.

![Conector de alimentaci&#xF3;n de la placa](../../../.gitbook/assets/image%20%2862%29.png)

Normalmente tanto la conexión de los calentadores para la cama como del extrusor no suelen tener polaridad. En todo caso sigue el orden que tengas actualmente o los colores de los cables, en especial si cuentas con un MOSFET externo ya que para estos si que suele importar la polaridad.

En este caso como modelos previos usan un MOSFET integrado WSK220N04 para la gestión de alimentación de la cama

![Conector de la alimentaci&#xF3;n Cama Caliente](../../../.gitbook/assets/image%20%2852%29.png)

### Thermistores

La orientación de los **thermistores** normalmente no es importante ya que estos no suelen tener polaridad. En este caso para esta placa contamos para un conector para la cama caliente \(TB\) y otro para el calentadore del hotend \(TH0\).

Es aconsejable usar conectores JST-XH 2.54mm como los de la siguiente imagen ya que evitan que se produzcan desconexiones y se minimizan los falsos contactos así como fijarlos con pegamento/silicona caliente.

![](../../../.gitbook/assets/image%20%2854%29.png)

### Drivers TMC2209

Esta placa cuenta con cinco **drivers** TMC2209 integrados y pre-configurados en el firmware que lleva instalado por defecto \(recordemos que para una Ender 3\) en modo UART.

Otra gran mejora en este modelo es el nuevo disipador que mejora considerablemente su cometido quedando bien fijado y dando una gran superficie de disipación de calor.

![](../../../.gitbook/assets/image%20%2855%29.png)

Al disponer estos drivers podemos aprovechar la funcionalidad de **Sensorless/Stallguard** que nos permite eliminar los sensores de final de carrera físicos, es aconsejable su uso solamente en los ejes X e Y.

Normalmente en otras placas para usar esta funcionalidad es necesario el pin DIAG siendo necesario quitarlo/doblarlo para usar finales de carrera físicos. En esta placa como en la SKR Mini E3 TURBO, v2 o SKR 1.3 cuenta con unos pines para poder hacer esta función de una forma cómoda.Jumpers DIAG para la gestión de sensorless o finales de carreras físicos

![](../../../.gitbook/assets/image%20%2857%29.png)

### Sensor de Filamentos

En esta placa como en sus hermanas contamos con un conector dedicado para la detección de falta de filamentos en el conector E-STOP. Si vas a instalarlo presta especial atencion en el pineado de tu sensor y de la placa

### Detección de fallo de corriente

Tambien contamos con un conector dedicado a la detección, mediante una placa externa como por ejemplo BTT UPS 24V, de fallos de alimentación que permite detectar un fallo de corriente... enviar una señal a la placa que informe al firmware que ha sucedido un problema en la alimentación y que inicie el proceso asociado para permitir recuperarla una vez la corriente se restablezca.

![](../../../.gitbook/assets/image%20%2849%29.png)

**Conector LCD**

Al estar preparado para impresoras Ender 3 la placa cuenta con un solo conector de 10 pines para el LCD que irá conectado entre el EXP1 de la placa y el EXP3 del LCD. En este caso de usar el firmware RepRap no será especialmente necesario ya que el manejo de la impresora suele hacerse en remoto.

En este aspecto esta placa estará limitado el número de pantallas compatibles a aquellas que usen conectores EXP3 o deberemos de modificar el fichero pins y/o realizar un adaptador para nuestro LCD no compatible.Conector EXP3 pantallas LCD Ender 3

![](https://telegra.ph/file/05e656e06301a03de263e.png)

**Conector TFT**

Este tipo de pantallas normalmente táctiles son un "ente" externo a Marlin que es el sistema operativo que gestiona nuestra placa y se comunica con el mediante una conexión serial a través del conector TFT.

Las pantallas SKR están preparadas con el correcto orden en el cableado \(RESET-RX-TX-GND-+5V\) pero podemos usar cualquier otra pantalla serial prestando atención al pineado de la misma y el de nuestra placa para adaptarlo.

![](../../../.gitbook/assets/image%20%2847%29.png)

**Ventilador de Capa**

