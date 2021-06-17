---
description: >-
  Como mejorar nuestras impresiones evitando que estas no finalicen
  correctamente ante un fallo de corriente.
---

# Módulo UPS - Recuperación ante fallos corriente

El lanzar una impresora de muchas horas y descubrir que esta no finalizó correctamente es una de las grades frustracciones de la impresión 3D. 

![](../../../.gitbook/assets/image%20%2872%29.png)

Para solventar este problema hay diferentes alternativas, siempre pensando en el "universo" Marlin, como pueden ser:

* Activado de Power Less Recovery \(PLR en adelante\) en Marlin con log en cambio de capa
* Activado de PLR usando un modulo UPS con detección y control de apagado
* Un SAI/UPS para mantener nuestra impresora funcionando sin suministro durante X minutos

En este caso vamos a optar por una solución intermedia entre el primer y el segundo caso mejorando y solucionando los fallos del primero \(la maquina no se apaga correctamente dañando la impresion en un alto porcentaje de veces\) y evitando el alto gasto de tener un SAI/UPS dedicado aunque sin duda es de las mejores opciones.

{% hint style="danger" %}
IMPORTANTE!!!  
**La función PLR gestionada por Marlin solamente funciona al imprimir desde la SD de la placa no desde un componente externo como pantallas TFT \(que pueden tener su propio sistema PLR\) o Octoprint, Pronterface o similares**
{% endhint %}

## Módulo UPS

Os sugerimos usar los siguientes módulos UPS que son compatibles con la mayoría de placas y la diferencia principal es el acabado del módulo y el cableado a la placa, es importante recalcar que a la hora de comprarlo tengamos bien presente a que voltaje funciona nuestra placa:

{% tabs %}
{% tab title="Amazon" %}
{% embed url="https://amzn.to/3t6l2yC" %}
{% endtab %}

{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_9ixLHJ" %}

{% embed url="https://s.click.aliexpress.com/e/\_9jMxSv" %}
{% endtab %}
{% endtabs %}

Personalmente en cuanto a calidad recomendamos el módulo de MKS pero si buscamos sencillez y menos complicaciones usemos el que mejor se adapte a la marca de nuestra electrónica.

{% hint style="danger" %}
**IMPORTANTE!!!**  
Estos módulos se conectan a la alimentación de nuestra fuente y/o a 220/110v así que es muy importante al manipular tomar todas las medidas de seguridad necesarias para evitar descargas eléctricas, si no estas seguro de lo que haces lleva tu impresora a un técnico.  
También es aconsejable que se coloquen en sitios que eviten el contacto o manipulación fácil además de intentar dejarlos protegidos con algún tipo de cubierta/protección.
{% endhint %}

## Cableado del módulo

A continuación y para los modelos sugeridos os facilitamos los esquemas de conexión que dependiendo de vuestra placa puede variar el pin de control:

{% tabs %}
{% tab title="BTT UPS" %}
![](../../../.gitbook/assets/image%20%2874%29.png)
{% endtab %}

{% tab title="MKS UPS" %}
![](../../../.gitbook/assets/image%20%2875%29.png)
{% endtab %}
{% endtabs %}

La parte más importante de la conexión es la que se hace al pin que tengamos asignado o asignemos a PLR que normalmente usaremos un conector JST-XH de 3 pines prestando atención en la ubicación del pin SGN tanto del módulo como de nuestra placa.   
En la siguiente tabla os indicamos para los modelos más usados por nosotros:

{% tabs %}
{% tab title="SKR" %}
<table>
  <thead>
    <tr>
      <th style="text-align:left">Placa</th>
      <th style="text-align:left">Puerto asignado</th>
      <th style="text-align:center">Pin SGN</th>
      <th style="text-align:left">Num. Pin</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>BTT SKR 1.3</b>
      </td>
      <td style="text-align:left">Ninguno por defecto, elige un endstop de X, Y o Z MAXO</td>
      <td style="text-align:center">
        <p><b>O</b>
        </p>
        <p><b>O</b>
        </p>
        <p><b>X</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><b>X+ P1.28</b>
        </p>
        <p><b>Y+ P1.26</b>
        </p>
        <p><b>Z+ P124</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR 1.4/Turbo</b>
      </td>
      <td style="text-align:left">Al lado de Z- y EXP1</td>
      <td style="text-align:center">
        <p><b>O</b>
        </p>
        <p><b>O</b>
        </p>
        <p><b>X</b>
        </p>
      </td>
      <td style="text-align:left"><b>P1.00</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR PRO</b>
      </td>
      <td style="text-align:left">Ninguno por defecto, elige un sensor de filamentos E0, E1 o E2</td>
      <td
      style="text-align:center">
        <p><b>O</b>
        </p>
        <p><b>O</b>
        </p>
        <p><b>X</b>
        </p>
        </td>
        <td style="text-align:left">
          <p><b>E0 PE15</b>
          </p>
          <p><b>E1 PE10</b>
          </p>
          <p><b>E2 PG5</b>
          </p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR DIP v1.1</b>
      </td>
      <td style="text-align:left">Ninguno por defecto, elige E0 que es el sensor de filamentos</td>
      <td style="text-align:center"><b>OXO</b>
      </td>
      <td style="text-align:left"><b>E0 PC2</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR MINI E3 v2</b>
      </td>
      <td style="text-align:left">Al lado del conector Y-stop</td>
      <td style="text-align:center"><b>XOO</b>
      </td>
      <td style="text-align:left"><b>PC12</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR E3 Turbo</b>
      </td>
      <td style="text-align:left">A la derecha de EXP1</td>
      <td style="text-align:center"><b>XOO</b>
      </td>
      <td style="text-align:left"><b>P1_20</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>BTT SKR E3 RRF</b>
      </td>
      <td style="text-align:left">Al lado de Z-Stop y termistor hot-end</td>
      <td style="text-align:center"><b>XOO</b>
      </td>
      <td style="text-align:left"><b>PE0</b>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="MKS" %}
<table>
  <thead>
    <tr>
      <th style="text-align:left">Placa</th>
      <th style="text-align:left">Puerto asignado</th>
      <th style="text-align:left">Pin SGN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>MKS SGEN_L v1</b>
      </td>
      <td style="text-align:left">Al lado de Z- en el conector Z+</td>
      <td style="text-align:left">
        <p><b>O</b>
        </p>
        <p><b>O</b>
        </p>
        <p><b>X - PB10</b>
        </p>
      </td>
    </tr>
  </tbody>
</table>
{% endtab %}

{% tab title="FYSECT" %}

{% endtab %}

{% tab title="FLY" %}

{% endtab %}
{% endtabs %}

## Configuración en Marlin

Dentro de nuestro fichero configuration\_adv.h buscaremos POWER LOSS RECOVERY dejando habilitada esta función de Marlin eliminando //

```cpp
#define POWER_LOSS_RECOVERY
```

 __Si queremos que PLR esté siempre habilitado deberemos activar PLR ENABLED DEFAULT de la siguiente forma, aunque podemos habilitarlo en el gcode de inicio de nuestro slicer para activarlo solo cuando lo consideremos necesario con el gcode **M413 S1** \(**M413 S0** para deshabilitarlo y **M413** sin ningún parámetro para ver el estado\)

```cpp
#define PLR_ENABLED_DEFAULT   true
```

En nuestro caso y como vamos a instalar un módulo UPS habilitaremos esta funcionalidad que permitirá al PLR poder realizar unas opciones avanzadas:

```cpp
#define BACKUP_POWER_SUPPLY
```

Relacionado con el punto anterior y dado que montaremos el módulo UPS habilitaremos la opción que al detectar un fallo en corriente levante el eje Z unos milímetros para evitar que el nozzle nos estropee nuestra pieza, **es muy importante entender que la cantidad de corriente disponible en el módulo es limitada y tenemos que minimiza el consumo de la misma durante el proceso PLR para que todo el proceso se realice de forma adecuada**, lo haremos habilitando teniendo en cuenta poner un multiplicador del valor de full step para nuestro eje Z:

```cpp
#define POWER_LOSS_ZRAISE       2

Calculo de full step para Ender 3
400 (steps) / 16 (micro steps) = 25 (steps por mm)
1 (step) / 25 ) = 0.04 (mm por cada full step)

2 (mm usados en POWER_LOSS_ZRAISE) / 0.04 (mm por cada full step) = 50 (full steps)
```

Como ya hemos indicado el módulo UPS va conectado a la placa para dar la señal de que la corriente ha fallado y proceda a realizar el proceso de apagado. Para nuestro caso vamos a usar nuestra [SKR E3 RRF ](https://3dwork.qitec.net/guias-impresion-3d/mejoras-upgrades/electronica/skr-e3-rrf)la cual tiene un conector PT-DET \(pin PE0\) para este uso, en las tablas del punto anterior puedes encontrar indicados para las placas que normalmente usamos:

```cpp
#define POWER_LOSS_PIN         PE0
```

Deberemos indicar dependiendo el módulo UPS usado cual es el estado del pin por defecto, en el caso del módulo BTT este será HIGH:

```cpp
#define POWER_LOSS_STATE     HIGH
```

Al igual que el anterior necesitamos indicar si nuestra placa en el pin utilizado es PULLUP o PULLDOWN, en nuestro caso debemos de dejarlo activado para que funcione correctamente:

```cpp
#define POWER_LOSS_PULL
```

El siguiente valor dependerá de nuestro sistema de extrusión en el cual indicaremos cuantos mm de filamento vamos a purgar antes de retomar la impresión para asegurar que tenga un flujo constante de filamento al reanudar la impresión:

```cpp
#define POWER_LOSS_PURGE_LEN   20
```

También para evitar el reflujo de filamento al realizar el apagado controlado podemos ajustar una retracción la cual debería de ser del mismo valor que las opciones de retracción de nuestro slicer en su gcode final que siempre aconsejamos de unos 3-5mm. Este valor lo ajustaremos en 

```cpp
// #define POWER_LOSS_RETRACT_LEN 3
```

En todo caso y dado que ya realizamos una purga al retomar la impresión y por lo comentado que disponemos de una cantidad de corriente limitada NO lo vamos a habilitar en nuestro ejemplo.

Dado que disponemos de un módulo UPS no es necesario tener definido que Marlin guarde el estado de la impresión en cada cambio de capa además de que mejoraremos considerablemente la vida de nuestra SD por lo que dejaremos comentado esta opción:

```cpp
//#define POWER_LOSS_MIN_Z_CHANGE 0.05
```

Pues ya esta!!! a continuación os mostramos como debería quedar nuestra función PLR en Marlin \(2.0.7.2 en este ejemplo\):

```cpp
  #define POWER_LOSS_RECOVERY
  #if ENABLED(POWER_LOSS_RECOVERY)
    #define PLR_ENABLED_DEFAULT   true // Power Loss Recovery enabled by default. (Set with 'M413 Sn' & M500)
    #define BACKUP_POWER_SUPPLY       // Backup power / UPS to move the steppers on power loss
    //#define POWER_LOSS_RECOVER_ZHOME  // Z homing is needed for proper recovery. 99.9% of the time this should be disabled!
    #define POWER_LOSS_ZRAISE       2 // (mm) Z axis raise on resume (on power loss with UPS)
    #define POWER_LOSS_PIN         PE0 // Pin to detect power loss. Set to -1 to disable default pin on boards without module.
    #define POWER_LOSS_STATE     HIGH // State of pin indicating power loss
    #define POWER_LOSS_PULL           // Set pullup / pulldown as appropriate
    #define POWER_LOSS_PURGE_LEN   20 // (mm) Length of filament to purge on resume
    //#define POWER_LOSS_RETRACT_LEN 3 // (mm) Length of filament to retract on fail. Requires backup power.

    // Without a POWER_LOSS_PIN the following option helps reduce wear on the SD card,
    // especially with "vase mode" printing. Set too high and vases cannot be continued.
    //#define POWER_LOSS_MIN_Z_CHANGE 0.05 // (mm) Minimum Z change before saving power-loss data
  #endif
```

