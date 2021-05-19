# FYSETC S6 - Marlin

## platformio.ini modificaciones

```cpp
.../ini/stm32f4.ini
# upload_protocol = dfu
# upload_command = dfu-util -a 0 -s 0x08010000:leave -D "$SOURCE"
```

## configuration.h modificaciones

## Actualizar firmware

### Usando una SD

copiar firmware.bin -&gt; OLD.BIN

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

