---
description: >-
  Buscas potencia, expansión y máxima compatibilidad? Esta sin duda puede ser tu
  placa.
---

# FYSETC SPIDER

![](../../../../.gitbook/assets/image%20%2879%29.png)

La Fysetc Spider ha sido diseñada teniendo en mente su uso en impresoras VORON 2.4 o similares, el equipo de VORON ha cooperado estrechamente durante el diseño y desarrollo de la misma.

Con un tamaño relativamente compacto para una paca de 8 drivers además dispone de muchas grandes funcionalidades que os explicaremos a continuación como adaptador de corriente a 12v 5A que nos permitirá alimentar ventiladores silenciosos sin necesidad de componentes externos, otro adapador de 5V 8A para tiras leds, alimentación directa de Raspberry Pi etc...

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de FYSETC [https://t.me/fysetc](https://t.me/fysetc)‌

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

{% embed url="https://s.click.aliexpress.com/e/\_AMdgop" %}

## Diagramas Fysetc Spider

En los siguientes diagramas de placa, cableado y de pines podréis encontrar las inmensas posibilidades y potencia de esta placa.

{% tabs %}
{% tab title="Placa" %}
![](../../../../.gitbook/assets/image%20%2887%29.png)

![](../../../../.gitbook/assets/image%20%28143%29.png)
{% endtab %}

{% tab title="Cableado" %}
![](../../../../.gitbook/assets/image%20%2880%29.png)
{% endtab %}

{% tab title="Pins" %}
![](../../../../.gitbook/assets/image%20%2881%29.png)
{% endtab %}
{% endtabs %}

## Características

* Tamaño compacto 155.3x76.5mm
* **Procesador STM32F446 a 180Mhz... una auténtica bestia!!!**
* **8 drivers con soporte UART y SPI** que en el caso de usar TMC cuentan con pines para una configuración sencilla
* Alimentación 24v con conversores de corriente a 12V/5A, **5V/8A \(especialmente pensado para Raspberry Pi, tiras leds y otros accesorios\)**
* Protección con fusibles de fácil sustitución para protección de la entrada de corriente principal y cama caliente
* Conector especial para sensor de nivelación adaptable a diferentes voltajes \(24v/5v/3.3v\) para la mayor compatibilidad
* 10 salidas PWM controladas por MOSFET!!!  \(1 cama caliente, 3 HotEnd, 3 ventiladores, 3 para tiras RGB\)
* 3 conectores para sensores de temperatura pudiendo usar thermistores o thermoacopladores \(AD597 necesario\)
* **Hasta 8 ventiladores controlables** \(usando solamente un extrusor y sin leds RGB\)!!!, 2 tiras RGB \(12v o 24v\) y **1 tira NeoPixel 5v**
* **Conectores UART para conexión y alimentación de una Raspberry Pi** sin necesitad de cables externos
* 2 conectores para módulos SD
* **Conector SD en la placa**
* **Conector USB C y conector opcional para USB Tipo-B**
* Conectores EXP para pantallas LDC/TFT o USART, I2C, CAN
* Sensores PT100 se pueden usar directamente

## Conexiones Fysetc Spider

### Jumper selección de fuente de alimentación

La Spider dispone de un jumper para seleccionar la alimentación a usar entre USB o adaptador interno 5V \(requiere la fuente de alimentación conectada\) que evita desagradables problemas que pueden ocurrir al mezclar alimentaciones diferentes.

![En el caso de conectar a nuestro ordenador por USB colocar el jumper en la posici&#xF3;n U5V, en el caso de conectar a nuestra fuente de alimentaci&#xF3;n de la impresora dejarlo como en la foto.](../../../../.gitbook/assets/image%20%2877%29.png)

### Conectores de alimentación

Tal como hemos comentado anteriormente esta placa dispone de un arsenal de conexiones y opciones así que la parte de alimentación no podría ser menos.

![](../../../../.gitbook/assets/image%20%2878%29.png)

Disponemos de las siguientes conexiones de alimentación de componentes o de la propia placa las cuales están bien indicadas en la serigrafía así como la polaridad en ellas:

* Alimentación HotEnd, como ya comentamos esta placa puede controlar hasta 3 HotEnd independientes con lo que disponemos de 3 salidas de alimentación para estos \(E0/E1/E2 OUT\)
* Alimentación de nuestra cama caliente \(BED OUT\), si revisamos el esquema en puntos anteriores podremos conectar directamente nuestra cama 24v \(aunque siempre por protección es aconsejable el uso de un MOSFET externo\) o una cama 220v usando un relé SSR
* Alimentación dedicada para cama caliente \(BED IN\), normalmente conectado a nuestra fuente de alimentación en casos especiales con camas con mucho consumo podemos conectar ahí una fuente diferente dedicada para la cama exclusivamente
* Alimentación de la placa \(PWR IN\), no tiene mucho misterio... la alimentación proporcionada por nuestra fuente de alimentación para que toda la electrónica funcione

### Thermistores

![](../../../../.gitbook/assets/image%20%2890%29.png)

La orientación de los **thermistores** normalmente no es importante ya que estos no suelen tener polaridad. En este caso para esta placa contamos para un conector para la cama caliente \(TB\) y tres para  calentadores de hotend \(TE0, TE1 y T2\).

Estos pines cuentan con resistencias PULL-UP de 4k7 que permiten el uso de sondas PT1000 de forma directa.

Es aconsejable usar conectores JST-XH 2.54mm ya que evitan que se produzcan desconexiones y se minimizan los falsos contactos.

### Drivers

Como ya hemos comentado esta placa cuenta con 8 zócalos para drivers lo cual nos ofrece un abanico de posibilidades de ampliación increibles.

![](../../../../.gitbook/assets/image%20%2896%29.png)

Es importante resaltar que para el eje Z disponemos de dos salidas de motor para poder poner doble Z a nuestra máquina usando un solo driver. 

{% hint style="info" %}
En el caso que no lo usemos es importante colocar jumpers en Z1-MOT para que funcionen correctamente dado que el splitter interno es serie.
{% endhint %}

![](../../../../.gitbook/assets/image%20%2883%29.png)

#### Configuración de diferentes drivers

Aunque siempre es aconsejable configurar nuestros drivers en modos "inteligentes" SPI/UART la placa dispone de configuraciones para usar drivers en modo standalone.

{% tabs %}
{% tab title="UART" %}
![](../../../../.gitbook/assets/image%20%2888%29.png)

![](../../../../.gitbook/assets/image%20%28142%29.png)

**Para drivers Bigtreetech deberemos asegurarnos que esten en UART mediante el jumper soldado en la parte de abajo del propio driver.**

![](../../../../.gitbook/assets/telegram-cloud-photo-size-4-5877201890746415314-x.jpg)
{% endtab %}

{% tab title="SPI" %}
![](../../../../.gitbook/assets/image%20%2884%29.png)

**Para drivers que usen SPI como TMC2130/TMC5160-1 o similares**. En el caso que usemos Sensorless \(homing sin finales de carreras físicos\), o no,  no es necesario cortar el pin DIAG ya que la placa cuenta con jumpers para ser habilitada esta función.

![](../../../../.gitbook/assets/image%20%2885%29.png)
{% endtab %}

{% tab title="STANDALONE" %}
![](../../../../.gitbook/assets/image%20%2895%29.png)
{% endtab %}
{% endtabs %}

#### Dual X y Dual Z... Marlin

En el caso que queramos usar doble driver para los ejes de X y Z es importante que \(actualmente en versiones 2.0.8.x\) deberemos de colocar el segundo driver en los zócalos siguientes:

![](../../../../.gitbook/assets/image%20%28139%29.png)

#### Sensorless drivers TMC

Si queremos utilizar esta funcionalidad de los drivers TMC que usa los motores como finales de carrera lo tendremos muy sencillo ya que la placa dispone de unos jumpers específicos para habilitar esta función, de todas formas os aconsejamos revisar nuestra [guía sensorless](../../nivelacion/sensorless-homing-en-marlin.md) para conocer más de esta gran funcionalidad. 

![](../../../../.gitbook/assets/image%20%2885%29.png)

{% hint style="info" %}
**El uso de Sensorless es aconsejable únicamente para ejes X e Y**
{% endhint %}

### Endstops

En la Spider disponemos de 6 conectores para finales de carrera de 3 pines.

![](../../../../.gitbook/assets/image%20%2893%29.png)

De nuevo Fysetc ha implementado mejoras muy interesantes en los conectores de final de carrera donde con mucha frecuencia conectamos sensores de nivelación que pueden requerir de diferentes voltajes así que se han incluido unos jumpers \(mediante puente de soldadura\) para que podamos escoger el voltaje que entregará cada conexión.

![](../../../../.gitbook/assets/image%20%28141%29.png)

Podemos encontrar los siguientes bloques de conectores que pueden usar dos tipos de voltajes:

* Conectores **X+/Y+**, podremos escoger **5v o 3.3v \(defecto\)**... indicado por ejemplo para sensores bltouch de 5v
* Conectores **X-/Y-/Z-,** podremos escoger **24v o 3.3v \(defecto\)**... estos conectores están especialmente indicados para conectar sensores de nivelación inductivos/capacitativos
* Conector **Z+**, esta preparado para la conexión de un sensor de nivelación con servo al contar con el pin PA3 PWM podremos escoger mediante un numper al lado de Z- el voltaje de placa 24v o 5v en el caso que usemos un sensor estilo Bltouch.

#### Sensores de nivelación



### Conexión Raspberry Pi \(Octoprint\)

Nuestra Fysetc Spider cuenta con un conector dedicado para la conexión de una Raspberry Pi normalmente para controlar nuestra impresora con [Octoprint](../../../../octoprint/que_es_octoprint.md) o [Klipper](../../../../klipper/klipper-1.md).

![](../../../../.gitbook/assets/image%20%28140%29.png)

Este conector permite alimentar y conectar por UART nuestra Raspberry Pi, es interesante que la alimentación mediante el conector puede proveer hasta 8A.

Para habilitar el puerto UART en nuestra Raspberry Pi deberemos reconfigurarla mediante raspi-config desde una linea de comandos SSH por ejemplo. 

```bash
sudo raspi-config
=> Interfacing Option
=> Serial
=> NO=> YES
sudo nano /boot/config.txt
=> add this line :dtoverlay=pi3-disable-bt
=> then
sudo rebootsudo 
nano /boot/cmdline.txt
=> remove the word phase "console=serial0,115200" or "console=ttyAMA0,115200"
sudo reboot
```

## Firmware

La Fysetc Spider es una placa muy versátil como ya hemos comentado a nivel de hardware y no iva a ser menos a nivel de software. 

La Spider es compatible con Marlin y Klipper aunque además dispone de una edición especial con más memoria para poder usarse con RRF \(Duet\).

### Marlin



