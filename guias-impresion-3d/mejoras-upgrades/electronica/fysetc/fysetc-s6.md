---
description: >-
  Descubre esta gran placa que nos aporta potencia y muchas opciones de
  ampliación.
---

# FYSETC S6

![](../../../../.gitbook/assets/image%20%28100%29.png)

La **Fysetc S6** es una controladora para nuestras impresoras 3D o CNC que nos añadirá opciones de ampliación y funcionalidad increíbles. 

Con uno de las MCUs más potentes \(STM32F446 32b\) no te quedarás atrás en potencia o en opciones de uso de cualquier firmware actual.

Soportando 6 zócalos para drivers en un formato compacto, máxima compatibilidad con los más importantes firmwares y muchas otras mejoras que revisaremos a continuación.

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de FYSETC [https://t.me/fysetc](https://t.me/fysetc)‌

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## Donde comprarla?

{% tabs %}
{% tab title="Amazon" %}
![](../../../../.gitbook/assets/image%20%28102%29.png)
{% endtab %}

{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_9jiDPf" %}
{% endtab %}
{% endtabs %}

## Diagramas Fysetc S6

En los siguientes diagramas de cableado y de pines podréis encontrar las inmensas posibilidades y potencia de esta placa.

{% tabs %}
{% tab title="Hardware" %}
![](../../../../.gitbook/assets/image%20%28104%29.png)
{% endtab %}
{% endtabs %}

## Características

* Tamaño compacto 117x87mm
* **Procesador STM32F446 a 180Mhz... una auténtica bestia!!!**
* **6 drivers con soporte UART y SPI** que en el caso de usar TMC cuentan con pines para una configuración sencilla
* Alimentación 24v con conversores de corriente a 12V/5A, **5V/8A \(especialmente pensado para Raspberry Pi, tiras leds y otros accesorios\)**
* Protección con fusibles de fácil sustitución para protección de la entrada de corriente principal y cama caliente
* Conector especial para sensor de nivelación adaptable a diferentes voltajes \(24v/5v/3.3v\) para la mayor compatibilidad
* 10 salidas PWM controladas por MOSFET!!!  \(1 cama caliente, 3 HotEnd, 3 ventiladores, 3 para tiras RGB\)
* 3 conectores para sensores de temperatura pudiendo usar thermistores o thermoacopladores \(AD597 necesario\)
* **Hasta 8 ventiladores controlables** \(usando solamente un extrusor y sin leds RGB\)!!!, 2 tiras RGB \(12v o 24v\) y **1 tira NeoPixel 5v**
* **Conectores UART para conexión y alimentación de una Raspberry Pi** sin necesitad de cables externos
* 2 conectores para módulos SD
* **Conector SD en la placa**
* **Conector USB**
* Conectores EXP para pantallas LDC/TFT o USART, I2C, CAN
* Bootloader para actualizar firmware por SD o por USB

## Conexiones Fysetc S6

### Jumper selección de fuente de alimentación

La Spider dispone de un jumper para seleccionar la alimentación a usar entre USB o adaptador interno 5V \(requiere la fuente de alimentación conectada\) que evita desagradables problemas que pueden ocurrir al mezclar alimentaciones diferentes.

![](../../../../.gitbook/assets/image%20%2898%29.png)

### Conectores de alimentación

Tal como hemos comentado anteriormente esta placa dispone de un arsenal de conexiones y opciones así que la parte de alimentación no podría ser menos.

