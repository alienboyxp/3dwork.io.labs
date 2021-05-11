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

En los siguientes diagramas de cableado y de pines podréis encontrar las inmensas posibilidades y potencia de esta placa.

{% tabs %}
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



## Firmware

La Fysetc Spider es una placa muy versátil como ya hemos comentado a nivel de hardware y no iva a ser menos a nivel de software. 

La Spider es compatible con Marlin y Klipper aunque además dispone de una edición especial con más memoria para poder usarse con RRF \(Duet\).

### Marlin



