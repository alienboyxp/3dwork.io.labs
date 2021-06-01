---
description: >-
  Descubre esta gran placa que nos aporta potencia y muchas opciones de
  ampliación.
---

# FYSETC S6

![](../../../../../.gitbook/assets/image%20%28100%29.png)

La **Fysetc S6** es una controladora para nuestras impresoras 3D o CNC que nos añadirá opciones de ampliación y funcionalidad increíbles. 

Con uno de las MCUs más potentes \(STM32F446 32b\) no te quedarás atrás en potencia o en opciones de uso de cualquier firmware actual.

Soportando 6 zócalos para drivers en un formato compacto, máxima compatibilidad con los más importantes firmwares y muchas otras mejoras que revisaremos a continuación.

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de FYSETC [https://t.me/fysetc](https://t.me/fysetc)‌

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## Donde comprarla?

{% tabs %}
{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_9jiDPf" %}
{% endtab %}

{% tab title="Amazon" %}
![](../../../../../.gitbook/assets/image%20%28102%29.png)
{% endtab %}
{% endtabs %}

## Diagramas Fysetc S6

En los siguientes diagramas de cableado y de pines podréis encontrar las inmensas posibilidades y potencia de esta placa.

{% tabs %}
{% tab title="Hardware" %}
![](../../../../../.gitbook/assets/image%20%28105%29.png)
{% endtab %}

{% tab title="Pines" %}
Podemos encontrar toda la definición de pines en la serigrafía de la parte superior y trasera, en cualquier caso en los siguientes esquemas podéis encontrar una referencia a ellos.

![](../../../../../.gitbook/assets/image%20%28107%29.png)

![](../../../../../.gitbook/assets/image%20%28113%29.png)
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

![](../../../../../.gitbook/assets/image%20%2898%29.png)

### Conectores de alimentación

Tal como hemos comentado anteriormente esta placa dispone de un arsenal de conexiones y opciones así que la parte de alimentación no podría ser menos contando con las 3 salidas para extrusores, otra más para la cama... y con dos entradas de corriente una general y otra para la cama.

![](../../../../../.gitbook/assets/image%20%28111%29.png)

También contamos con otras salidas de alimentación de 24v para la alimentación de periféricos u otros componentes:

![](../../../../../.gitbook/assets/image%20%28129%29.png)

### Endstops \(finales de carrera\)

En esta ocasión contamos con los típicos conexiones para endstops.

![](../../../../../.gitbook/assets/image%20%28118%29.png)

De nuevo Fysetc ha implementado mejoras muy interesantes en los conectores de final de carrera donde con mucha frecuencia conectamos sensores de nivelación que pueden requerir de diferentes voltajes así que se han incluido unos jumpers \(mediante puente de soldadura\) para que podamos escoger el voltaje que entregará cada conexión.

![](../../../../../.gitbook/assets/image%20%28136%29.png)

Donde tenemos dos bloques de conectores que pueden usar dos tipos de voltajes:

* Conectores **X+/Y+/Z+**, podremos escoger **5v o 3.3v \(defecto\)**... indicado por ejemplo para sensores bltouch de 5v
* Conectores **X-/Y-/Z-,** podremos escoger **24v o 3.3v \(defecto\)**... estos conectores están especialmente indicados para conectar sensores de nivelación inductivos/capacitativos

Otro aspecto importante sobre los conectores endstops es que disponemos de varios con funciones ADC \(punto morado\) y PWM \(punto rojo\) muy útiles para el uso de servos, leds o similares. En el siguiente esquema podremos ver el tipo de cada uno:

![](../../../../../.gitbook/assets/image%20%28135%29.png)

#### Bltouch

La instalación de un sensor de nivelación del tipo Bltouch es muy sencilla y la podemos realizar usando el siguiente esquema:

![](../../../../../.gitbook/assets/image%20%28109%29.png)

Os aconsejamos revisar [nuestra guía de Bltouch](../../../nivelacion/bltouch-3dtouch.md) para más información sobre su instalación y configuración.

### Ventiladores

La Fysetc S6 cuenta con conexiones para 3 ventiladores gestionables. Además de esta gran mejora al poder gestionar estos 3 ventiladores y tal como comentábamos en las características de esta placa podemos, sin necesidad de ningún convertidor de corriente, configurar para que las salidas del ventilador sean de 24v o 12v para aquellos casos que usemos ventiladores silenciosos para nuestra electrónica o hotend. 

Para ello deberemos poner el jumper correspondiente entre FAN0/1/2 Voltage y el voltaje que queramos usar. En el siguiete diagrama podremos verlo fácilmente.

![](../../../../../.gitbook/assets/image%20%28120%29.png)

### Drivers

La Fysetc S6 dispone de una extensa configuración de jumpers/pins \(JP1 y JP6\) para poder soportar una gran variedad de drivers.

![](../../../../../.gitbook/assets/image%20%28134%29.png)

{% tabs %}
{% tab title="UART" %}
Para el uso de **drivers TMC2208/2225 o TMC2209/2226 en modo UART** \(inteligente\) solamente necesitaremos colocar un jumper en **JP1** el resto de configuración ser realizará directamente mediante nuestro firmware \(microsteps, vref, modo, etc...\).

![](../../../../../.gitbook/assets/image%20%28122%29.png)
{% endtab %}

{% tab title="SPI" %}
Para **drivers TMC2130/5160/5161 en modo SPI** necesitamos colocar **4 jumpers en el conector JP6 \(1 2 3 4-9 10 11 12\)**.

![](../../../../../.gitbook/assets/image%20%28106%29.png)
{% endtab %}

{% tab title="STANDALONE" %}
Para la configuración de drivers en modo STANDALONE \(simple\) necesitamos usar los pines del conector JP6 \(1234-5678\) para configuración de microsteps y alguna funcionalidad en drivers TMC según se muestra en el esquema anterior.

![](../../../../../.gitbook/assets/image%20%28132%29.png)
{% endtab %}
{% endtabs %}

Una funcionalidad muy interesante para drivers UART/SPI inteligentes, que lo soporten, es **SensorLess** que permite usar el propio motor como final de carrera ahorrando en componentes, puntos extra de fallo y cableado. Podéis tener más información y detalle en nuestra [guía de Sensorless](../../../nivelacion/sensorless-homing-en-marlin.md) para Marlin.

![](../../../../../.gitbook/assets/image%20%28128%29.png)

En el caso de la Fysetc S6 disponemos de unos pines que colocando el correspondiente jumper nos permiten enviar la señal DIAG del driver al endstop. Recordar que al activar este jumper deberemos tener correctamente configurado nuestro firmware.

{% hint style="warning" %}
**El uso de Sensorless es aconsejable únicamente para ejes X e Y**
{% endhint %}

### Conectores para LCD

Contamos con dos conectores EXP compatibles con la inmensa totalidad de pantallas de este tipo. En el siguiente esquema podremos ver el esquema de pines disponibles:

![](../../../../../.gitbook/assets/image%20%28131%29.png)

### Conector UART/Serial

Contamos con **hasta seis puertos USART/Serial** para la conexión de dispositivos serial como pantallas TFT, módulos Wifi, adaptador USB para actualización de firmware o similares.

* USART1 -RX1 y TX1- \(PA10 y PA9 en UART\)
* USART2 -RX2 y TX2- \(PA3 y PA2 en Z-MAX y Y-MAX\)
* USART3 -RX3 y TX3- \(PC11 y PC10 en EXP1 y conector FPC\)
* USART4 -RX4 y TX4- \(PA1 y PA0 en X-MAX y Z-MIN\)
* USART5 -RX5 y TX5- \(PD2 y SDA2 en EXP1\)
* USART6 -RX6 y TX6- \(PC7 y PC6 en EXP2\)

![Esquema de posibles conexiones UART para Fysetc S6](../../../../../.gitbook/assets/image%20%28115%29.png)

Para definir estos USART/Serial en Marlin, recordar que a partir de Marlin 2.0.8 podemos usar hasta 3 puertos USART/Serial:

```cpp
#define SERIAL_PORT -1 // 3DWORK USART USB
#define SERIAL_PORT_2 1 // 3DWORK USART1
#define SERIAL_PORT_3 3 // 3DWORK USART3 Example
```

Puedes tener mas detalle en la siguiente tabla:

{% tabs %}
{% tab title="USART1 " %}
![](../../../../../.gitbook/assets/image%20%28127%29.png)
{% endtab %}

{% tab title="USART2" %}
![](../../../../../.gitbook/assets/image%20%28125%29.png)
{% endtab %}

{% tab title="USART3" %}
![](../../../../../.gitbook/assets/image%20%28130%29.png)
{% endtab %}

{% tab title="USART4" %}
![](../../../../../.gitbook/assets/image%20%28138%29.png)
{% endtab %}

{% tab title="USART5&6" %}
![](../../../../../.gitbook/assets/image%20%28133%29.png)

![](../../../../../.gitbook/assets/image%20%28114%29.png)
{% endtab %}
{% endtabs %}

![Ejemplo de conexi&#xF3;n de m&#xF3;dulo USB para actualizaci&#xF3;n de firmware. ](../../../../../.gitbook/assets/image%20%28108%29.png)

## Firmware

Fysetc S6 es compatible con los firmwares más importantes, a continuación os dejamos un link a nuestras guías de configuración:

{% page-ref page="fysetc-s6-marlin.md" %}

{% page-ref page="fysetc-s6-klipper.md" %}



