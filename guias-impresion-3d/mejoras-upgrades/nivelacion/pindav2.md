---
description: >-
  Tener instalado un sensor de Nivelación nos permite asegurar unas primeras
  capas perfectas sin tener que estar ajustando manualmente nuestra mecánica.
---

# PINDAv2

En este caso usaremos un PINDAv2 que es un sensor que incluye un thermistor que nos permite compensar uno de los puntos débiles de este tipo de dispositivos, la precisión en las lecturas con temperatura.

Otro hándicap es el rango de detección ya que normalmente no son capaces de detectar a través de bases de cristal muy gruesas.

Por el contrario una de sus grandes ventajas es que dado que no disponen de partes móviles como sensores estilo Bltouch son mucho más robustos y menos dados a tener fallos por uso intensivo.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

{% tabs %}
{% tab title="Amazon" %}
{% embed url="https://amzn.to/3yRvSwu" %}
{% endtab %}

{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_At8ATR" %}
{% endtab %}
{% endtabs %}

## Esquemas de conexión

Este tipo de sensores dada su simplicidad se conectan de forma normal en el conector Z- en sustitución del final de carrera. De todas formas y para alguna de las placas que disponemos puede ser aconsejable conectarlas en una ubicación diferente:

{% tabs %}
{% tab title="Fysetc Spider" %}
![](../../../.gitbook/assets/image%20%28151%29.png)

{% hint style="success" %}
Esta placa cuenta con un arsenal de conexiones aunque podemos conectar en sus múltiples pines es aconsejable realizarlo en Z+ dado que dispone de la posibilidad de alimentación con diferentes voltajes de forma sencilla y además de disponer de un conversor en la entrada de señal para su pin que traduce las señales de cualquier voltaje al 3.3v requerido internamente.
{% endhint %}
{% endtab %}
{% endtabs %}

## **Cambios en Marlin**

* Si nuestro sensor de nivelación de cama se encuentre en el conector del final de carreras Z-MIN/Z-STOP dependiendo de la placa habilitaremos lo siguiente: **`#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN // 3DWORK PINDAv2`** _****_En el caso que nuestra placa disponga de un conector especial para el endstop del sensor, necesitemos instalarlo en otro conector y dependiendo de la versión de Marlin usada \(normalmente 2.0.7.x ya soporta la mayoria de placas actuales\) **`#define USE_PROBE_FOR_Z_HOMING // 3DWORK PINDAv2`** Si nuestro fichero pin de nuestra placa no contempla la definicion del PROBE\_PIN deberemos definirla aqui \(XX sera el pin usado\): **`#define Z_MIN_PROBE_PIN XXX // 3DWORK PINDAv2`**
* Indicamos el tipo de sensor de nivelación **`#define FIX_MOUNTED_PROBE // U1JO PINDAv2`**
* Ajustamos los offsets \(localización del sensor de nivelación con respecto a la punta del nozzle\) del sensor de nivelación **`#define NOZZLE_TO_PROBE_OFFSET…`**

![Se aconseja dejar 1-2mm entre el punto Z0 y el sensor PINDAv2](../../../.gitbook/assets/image%20%28156%29.png)

![](https://lh6.googleusercontent.com/MIzOshOTC8vslx7nFkpDWoTm5BKDquaQ_hJwKyiYPGruNr0namQnSjOnOyM8Je0da4-NwVCrvlhtlb4eoG0aHlpLdhnQwWLP0tRkS8zFHY32IVg6mupLJPvBN9IkwKY_fgUOdDQO)

![](https://lh3.googleusercontent.com/LJVKD2x3FXgO03Pb9lZPsqnFR0J55nJIRInl-DhLDFLwkQ6nGK82BTbbNAY3KkeKR1EBlVMzHT2lOkQ1ZxFXA0yS3KEvrPe9_HEjCA4Vf86E_dU6zxTLSzX91XIYGu1vR2NI_gkv)

> Es muy importante una vez subidos los cambios veríficar que los offsets esten correctamente cargados en la EEPROM. Esto lo podemos verificar usando el comando **M503** desde Pronterface/Octoprint o desde la propia pantalla \(segun version y opciones habilitadas en Marlin\) en Configuración/Avanzado/Probe Offsets.   
>
>
> En el caso que no estén cargados correctamente esros valores podemos refrescar los datos desde Pronterface/Octoprint usando **M502** **M500** o desde la pantalla de la impresora **Configuración Reset Load Save Eeprom**.
>
> Si deseamos **cambiarlos sin tener que compilar Marlin podemos desde Pronterface/Octoprint** usar el comando **M851** y despues un **M500** \(guardarlos en EEPROM\) y un **M503** para verificar que se guardaron correctamente. Por ejemplo para un desfase X de -1.7 e Y -1.3 de nuestro PROBE usariamos el siguiente comando
>
> ```text
> M851 X-1.70 Y-1.30
> ```

* Ajustamos el tipo de nivelación automática, inicialmente durante la instalación del sensor de nivelación se aconseja usar BILINEAR pero el aconsejable para dejar en nuestra impresora es UBL que se puede encontrar en esta misma guía el proceso de configuración **`#define AUTO_BED_LEVELING_BILINEAR`**
* En el caso de tener cama de cristal con grapas/clips ajustamos el margen de seguridad para que el sensor no toque con las grapas/clips **`#define MIN_PROBE_EDGE 20 // En este caso decimos 20mm de seguridad`** Para versiones 2.0.6 de Marlin o superiores: **`#define PROBING_MARGIN 20 // En este caso decimos 20mm de seguridad`**
* Para una mayor resolución en las medidas del sensor podemos habilitar que realice diferentes medidas en un mismo punto **`#define MULTIPLE_PROBING 3 // Realizar tres medidas en cada punto`**
* Con la siguiente linea permitiremos poder bajar el eje Z por debajo de 0 algo muy util para poder encontrar el ZOffset pero tambien peligroso ya que no tendremos limites de final de carrera en este eje en movimientos manuales **`#define MIN_SOFTWARE_ENDSTOP_Z // Permite mover el nozzle por debajo de 0 en Z`**
* Dependiendo del tipo de sensor puede ser necesario invertir el funcionamiento de final de carrera **`#define Z_MIN_PROBE_ENDSTOP_INVERTING true`**
* Por seguridad y teniendo en cuenta la localización del sensor es aconsejable que el homing en Z se realice en el medio de la cama **`#define Z_SAFE_HOMING`**
* Es importante habilitar esta linea dado que un G28 desactiva el autonivelado, de esta forma no lo hará **`#define RESTORE_LEVELING_AFTER_G28`**
* Podemos habilitar el comando G26 que nos permite lanzar un test de nivelación directamente sin necesidad de generar ninguno desde nuestro slicer, algo muy útil para un test rápido. Dentro de las opciones del G26 podrás ajustar los parámetros básicos de impresión **`#define G26_MESH_VALIDATION`**
* Es muy útil y necesario habilitar las opciones de nivelación en el LCD **`#define LCD_BED_LEVELING`**
* Seleccionar el método de nivelado a usar, descomentar una de las líneas solamente, en este mismo documento está descrito como realizar el tipo UBL aconsejable para camas de gran tamaño o con problemas: **`//#define AUTO_BED_LEVELING_3POINT //#define AUTO_BED_LEVELING_LINEAR //#define AUTO_BED_LEVELING_BILINEAR // Aconsejable camas menores 235x235 //#define AUTO_BED_LEVELING_UBL // Aconsejable resto de tamaños o con problemas nivelacion //#define MESH_BED_LEVELING`**
* Otro valor interesante es el de ajustar hasta que punto de la impresión se realizarán las correcciones, por defecto Marlin aplica estas correcciones en los primeros 10mm pero dependiendo de las necesidades se puede ajustar asegurando que habilitamos **`#define ENABLE_LEVELING_FADE_HEIGHT`**

![](https://lh3.googleusercontent.com/4gh4NwC0kjGEfpy7iiS9xGEaw8IMMe2kt8OvGRVn5lwH0JM-rWEmFnHI6kY7jUSbpfo4S7trK9iK14NM6DRTs1ysMwH1e5r3G--kzj4V2KMiCUnUKCzArgT5J0-uQpLoOcNQBdP2)

![](https://lh6.googleusercontent.com/k9AzQIyVbu4dEixRRkGHPK8oXlUlGAeaGC9VFd4Fv1tUQCS2Zb2g_Pv0n59xujlY1gp6OVjyjJM7DX9rtSJhmdP7dsvES1l9cZtH9WCgEFo9iiWWEdlNmWrm-VwPTAx9OQy0XUdB)

* Habilitar el homing por eje, muy útil para ver que cada eje funciona correctamente en el proceso de homing **`#define INDIVIDUAL_AXIS_HOMING_MENU`**
* En el caso que nuestro sensor disponga de sonda de temperatura para compensación lo habilitaremos de la siguiente forma **`#define TEMP_SENSOR_PROBE 1`** Tenemos que tener en cuenta que deberemos conectar el pin del thermistor de nuestra sonda a un pin PWM para poder obtener dichas lecturas en el caso que nuestro fichero pins no disponga de el deberemos de crearlo y asignarlo a un pin libre.

![](../../../.gitbook/assets/image%20%28152%29.png)

### **Otros cambios en configuration\_adv.h**

* Es aconsejable activar reintentos en el proceso de generacion de malla G29 para prevenir problemas durante el proceso \(**esta función puede no ser compatible con todos los sistemas de nivelación como por ejemplo UBL**\) **`# define G29_RETRY_AND_RECOVER`**
* Al usar un sensor PINDAv2 activaremos la calibración por temperatura para compensar las desviaciones que la temperatura puede provocar en este tipo de sensores **`#define PROBE_TEMP_COMPENSATION`**

### Resumen de cambios

Para nuestro ejemplo usaremos una placa Fysetc Spider conectando el sensor en el conector Z+ tal como sugiere el fabricante quedando nuestra configuración de la siguiente forma:

```text

```

### **Activar Babystepping**

Esta funcionalidad es muy útil para un ajuste fino durante la impresión de la altura del eje Z  
En **configuration\_adv**:  
**`#define BABYSTEPPING  
#define DOUBLECLICK_FOR_Z_BABYSTEPPING  
#define BABYSTEP_DISPLAY_TOTAL`**

## **Comprobaciones previas**

* **Comprobación del sensor**:
  * **Comprobación de la parte endstop**, la parte endstop funciona como cualquier endstop y esta gestionado por el par de cables negro y blanco que normalmente colocamos en el puerto PROBE o Z-STOP/Z-MIN de la placa. Dependiendo de las versiones de Marlin debera colocarse en uno o en otro si esta soportada la placa o no completamente. Para comprobar que funcione correctamente y tal como hicimos en el paso anterior mediante el menu o comandos desplegamos y recogemos el pin y comprobamos con **M119** que el enstop cambia de **TRIGERED** \(detección de objeto\) a **OPEN** \(no detección de objeto\) correctamente.
  * **Activar Modo Debug nivelado** _Cambios en Marlin \(configuration.h\):_ **`#define DEBUG_LEVELING_FEATURE // 3DWORK Permite obtener información detallada en caso de problemas`** _Comandos gcode para habilitar/deshabilitar el modo Debug:_ **`M111 S38 ; LEVELING, ERRORS, INFO M111 S0 ; Disable debug`**
  * **Activar Test de precisión** Permite habilitar un menú dentro del LCD para realizar tests de repetibilidad y mostrar el rango de precisión del sensor: **`#define Z_MIN_PROBE_REPEATABILITY_TEST`**

## **Ajuste del Z-Offset - WIZARD**

Desde la version 2.0.7.2 Marlin incluye un nuevo asistente para encontrar el valor Z Offset de una forma sencilla. Para activarlo iremos a configuration\_adv y habilitaremos:

**`#define PROBE_OFFSET_WIZARD`**

Con esto dispondremos de un nuevo menu en Configuración/Avanzado/Probe Offsets

{% embed url="https://www.youtube.com/watch?v=2gRfU26aTDs" %}

## **Ajuste del Z-Offset - Pronterface**

Desde un cliente terminal como Pronterface:

1. **Ajuste manual de la cama 4 esquinas y folio tradicional**
2. Realizaremos un **G28**
3. Realizaremos un **pre-calentado de cama y nozzle** que normalmente usemos
4. Realizaremos una malla usando **G29 P1** desde el terminal o desde las herramientas UBL de la pantalla
5. Una vez finalizado podemos usar **G29 T** para visualizar los valores de la malla. Puedes usar estas herramientas web para verlo graficamente: [https://mkdev.co.uk/mesh-visualizer/](https://mkdev.co.uk/mesh-visualizer/) [http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/](http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/) [https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/](https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/)
6. Por la ubicación del sensor de nivelación en muchas ocasiones Marlin no va a llegar a todos los puntos de la malla con lo que en el paso anterior veremos coordenadas sin valores solamente con un ".". Para realizar el relleno de todos los puntos que no pudieron medirse lanzaremos un **G29 P3** \(**G29 P3 T** mejor ya que calcula todos los puntos que falten\) este comando calcula extrapolando valores que si que se pudieron verificar a los que no pudieron medirse.
7. Una vez aparezcan todos los puntos de la malla con un valor, fuera medido o extrapolado por el punto anterior, almacenaremos la malla en la posición 1 de la EEPROM con **G29 S1**
8. Nos aseguramos de que el autonivelado quede activo con **G29 A**
9. Ahora que ya tenemos todos los valores básicos correctos lanzaremos un **M500** para guardarlos en la EEPROM.

{% hint style="warning" %}
**IMPORTANTE!!! cada vez que la EEPROM sea borrada o reseteada se tiene que volver a realizar este proceso si no almacenamos este valor en nuestros fuentes de Marlin!!**!
{% endhint %}

## Calibración del sensor de temperatura

Este proceso puede llegar a durar mucho tiempo, cerca de 1hora, con este proceso nos ayudará a mejorar la precisión de las lecturas, realizaremos el siguiente proceso desde nuestro cliente terminal favorito:

1. **G76** -  inicia la calibración, tarda alrededor de 1h
2. **M871** - muestra los valores de las lecturas
3. **M500** - almacena en la EEPROM las lecturas de compensación de temperatura

{% hint style="warning" %}
**IMPORTANTE!!! cada vez que la EEPROM sea borrada o reseteada se tiene que volver a realizar este proceso si no almacenamos este valor en nuestros fuentes de Marlin!!**!
{% endhint %}

