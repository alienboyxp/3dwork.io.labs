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

![](../../../.gitbook/assets/image%20%2853%29.png)

![](../../../.gitbook/assets/image%20%2859%29.png)

## Conectores SKR E3 RRF

### ESP-07S - Módulo Wifi

![](../../../.gitbook/assets/image%20%2843%29.png)

Una de las novedades de esta nueva placa es la inclusión de un módulo Wifi integrado, principalmente para la gestión de la impresora en modo RRF\(Duet\), al que es importante conectar la mini antena que incluye.

### Jumper selección de fuente alimentación

Al contrario de sus hermanas [SKR E3 Turbo](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr_mini_e3_turbo) o [SKR MINI E3 v2](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr-mini-e3-v2). En este modelo han incluido de nuevo el jumper para seleccionar la alimentación a usar entre USB o Fuente que evita desagradables problemas que pueden ocurrir al mezclar alimentaciones diferentes.

![](../../../.gitbook/assets/image%20%2852%29.png)

Es importante para usar la placa con fuente de alimentación de la impresora que el jumper este colocado en el pin E.

![](../../../.gitbook/assets/image%20%2846%29.png)

En el caso de alimentar por USB deveremos de tener colocado el jumper en el pin VUSB. 

{% hint style="info" %}
**Obvia decir que dependiendo la alimentación el jumper debe de poner en uno u otro pin, no en los dos a la vez o en ninguno de ellos.**
{% endhint %}

### Conectores de alimentación de la placa

![](../../../.gitbook/assets/image%20%2851%29.png)

Tal como hemos comentado el orden de algunos cables ha cambiado así que debemos, en especial en estos, prestar mucha atención a la polaridad de estos conectores.

![Conector de alimentaci&#xF3;n de la placa](../../../.gitbook/assets/image%20%2857%29.png)

Normalmente tanto la conexión de los calentadores para la cama como del extrusor no suelen tener polaridad. En todo caso sigue el orden que tengas actualmente o los colores de los cables, en especial si cuentas con un MOSFET externo ya que para estos si que suele importar la polaridad.

En este caso como modelos previos usan un MOSFET integrado WSK220N04 para la gestión de alimentación de la cama

![Conector de la alimentaci&#xF3;n Cama Caliente](../../../.gitbook/assets/image%20%2850%29.png)

### Thermistores





