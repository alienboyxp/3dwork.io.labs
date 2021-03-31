---
description: >-
  Se te ha roto el MOSFET del ventilador de capa o necesitas otro ventilador
  controlable?
---

# Control de ventiladores con MOSFET externo

Nuestras placas usan un componente MOSFET para proveer de corriente a los diferentes perifericos que se conectan a ella. Básicamente un MOSFET es usado como un interruptor que actúa como amplificador de la señal recibida.

En el caso de las impresoras 3D este tipo de puertos se usan para ajustar una señal PWM. Esta señal se envía en pulsos más rápidos o lentos para cambiar la potencia de salida del ventilador.

Para poder realizar este tipo de cambios requerimos un MOSFET externo, aunque no esta enfocado al objetivo de la guía normalmente a nuestras impresoras 3D se suelen poner dos tipos de MOSFET externos

* MOSFET para la gestión del hotend o cama calientes, estos normalmente ayudan a proteger la electrónica de la placa y mejoran la potencia y estabilidad suministradas a estos componentes. Son muy aconsejables en el caso de uso de camas calientes ya que generan un alto consumo y desgaste sobre la electrónica.

Lista de pines PWM disponibles

| SKR 1.4 |  |
| :--- | :--- |
| **Z-Stop P1\_27 PWRDET P1\_00 I2C\_SCA1 P0\_00 I2C\_SCL1 P0\_01 SPI P0\_26 \(top left\) \(cercano a power CPU\) \(appears to not be used by SPI\) NEOPIX P1\_24 PROBE P0\_10** |  |

{% tabs %}
{% tab title="SKR 1.4" %}
* Verificados
  * **Z-Stop P1\_27**
  * **PWRDET P1\_00**
  * **I2C\_SCA1 P0\_00**
  * **I2C\_SCL1 P0\_01**
  * **SPI P0\_26 \(top left\) \(cercano a power CPU\) \(parece no usado por el puerto SPI\)**
  * **NEOPIXEL P1\_24**
  * **PROBE P0\_10**
* No verificados pero deberían de funcionar
  * **SERVO P2\_02**
* PWM pero normalmente en uso
  * **E1 Stepper pins,**
    * **E1 Enable P1\_16**
    * **E1 Step P1\_15**
    * **E1 Dir P1\_14**
  * **TFT Pins**
    * **TFT RX0 P0\_03**
    * **TFT TX0 P0\_02**
  * **Wifi Pins**
    * **Wifi TX3 P4\_28**
    * **Wifi RX3 P4\_29**
* PWM pero NO USAR!!!
  * **SPI\_SCK1 P0\_07**
  * **SPI\_MISO1 P0\_08**
  * **SPI\_MOSI1 P0\_09**
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

