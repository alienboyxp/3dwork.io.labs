---
description: No estropees tus impresiones por falta o atasco de filamentos!!!
---

# Sensor de filamentos

El uso de un sensor de filamentos puede salvarnos más de una impresión, además si aplicaste la configuración SensorLess/stallGuard explicada en este documento probablemente dispongas de varios finales de carrera que puedes usar para este propósito sin gastarte nada, en Thingiverse dispones de varios adaptadores para ello.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

## Algunos sensores a modo de referencia

{% tabs %}
{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_ACJBgV" %}

{% embed url="https://s.click.aliexpress.com/e/\_99kqNx" %}

{% embed url="https://s.click.aliexpress.com/e/\_AfFxXT" %}
{% endtab %}

{% tab title="Amazon" %}
{% embed url="https://amzn.to/3e3GKyJ" %}

{% embed url="https://amzn.to/3e5D93u" %}
{% endtab %}
{% endtabs %}

## Instalación del sensor

Dependiendo del tipo de sensor contaremos con 2 normalmente simples endstops o 3 conexiones dependiendo de si este dispone de cierta electrónica o led de control.

En cualquier caso las tres conexiones normalmente hacen referencia a V\(voltaje\), S \(señal\) y G \(negativo\).

![Conector BTT Smart Filament Sensor](../../../.gitbook/assets/image%20%2882%29.png)

### Conectando sensor a nuestra placa

Es la opción recomendada dado que el sensor estará controlado directamente por la maquina y no por un ente externo como pueda ser una pantalla TFT o una Raspberry Pi.

Es importante que revisemos el orden de pines tanto en la parte del sensor como en el de la placa para que coincidan perfectamente para evitar cualquier posible corto y que podamos estropear placa o sensor.

Para la conexión en la placa si estas disponen de un conector dedicado suele llamarse E0-STOP, FT-STOP y en caso de no llevar suele usar el del final de carrera + que, tal como explicamos más adelante en los cambios Marlin, deberemos de verificar en el fichero pins de nuestra placa. En cualquier caso tenéis a continuación una tabla con algunos de las placas más representativas:

{% tabs %}
{% tab title="SKR" %}
<table>
  <thead>
    <tr>
      <th style="text-align:left">Electr&#xF3;nica</th>
      <th style="text-align:left">Localizaci&#xF3;n E0-STOP</th>
      <th style="text-align:left">Pineado</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">BTT SKR 1.4/Turbo</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">
        <p>V</p>
        <p>G</p>
        <p>S</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR 1.3</td>
      <td style="text-align:left">Utilizaremos endstop X+ ya que esta predefinido en nuestro fichero pins
        para esta funci&#xF3;n</td>
      <td style="text-align:left">
        <p>V</p>
        <p>G</p>
        <p>S</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR PRO</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">
        <p>V</p>
        <p>G</p>
        <p>S</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR E3 DIP</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">G S V</td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR MINI E3 v1.2</td>
      <td style="text-align:left">Dispone de E0-STOP pero no tiene toma de voltaje, mejor usar PT-DET si
        nuestro sensor necesita voltaje</td>
      <td style="text-align:left">S G V</td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR MINI E3 v2</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">S G V</td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR E3 Turbo</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">S G V</td>
    </tr>
    <tr>
      <td style="text-align:left">BTT SKR E3 RRF</td>
      <td style="text-align:left">Dispone de E0-STOP</td>
      <td style="text-align:left">S G V</td>
    </tr>
  </tbody>
</table>
{% endtab %}
{% endtabs %}

### Conectando sensor a nuestra pantalla TFT

Otra forma de instalar nuestro sensor, y siempre que esta lo soporte, es usando nuestra pantalla TFT que tiene como ventaja que no es necesario tocar Marlin pero por otro su funcionamiento suele estar restringido a usar la TFT para gestionar tus impresiones.

Es importante comentar que en estos casos es muy aconsejable que nuestro Marlin, además de las opciones requeridas por nuestra pantalla \([para pantallas SKR revisa esta guía](../electronica/pantallas-bigtreetech-skr.md#cambios-en-marlin)\), tener habilitado

```cpp
 #define EMERGENCY_PARSER
 #define M114_DETAIL
```

A continuación tienes un listado de pantallas con esta opción:

| TFT | Conector | Pineado |
| :--- | :--- | :--- |
| BTT TFT35 v3 | Dispone de FIL-DET | S G V |
| TFT35 v2 | Dispone de DLJC | V G S |
| TFT35 E3 v3 | Dispone de FIL-DET | S G V |

## **Cambios en Marlin:**

* Activar el sensor de filamentos `#define FILAMENT_RUNOUT_SENSOR`

> Es aconsejable revisar antes de comenzar a imprimir comprobar desde el terminal con un M119 si el sensor de filamentos funciona de la forma correcta y en caso de hacerlo que la lógica de detección de filamento \(con filamento TRIGGERED\) es la correcta. Si no puedes cambiarla aquí o desde el propio LCD puedes cambiar la lógica así como habilitar o deshabilitar el sensor si lo deseas:  
> \#define FIL\_RUNOUT\_INVERTING true  
> o para 2.0.6 o superiores  
> \#define FIL\_RUNOUT\_STATE HIGH o LOW

> Algunas placas disponen de un conector dedicado para este sensor de filamentos, otras lo ubican normalmente en X\_MAX pero en cualquier caso no está de más **verificar el archivo de PINS de tu placa** para identificar en dónde está habilitado.  
>   
> Revisa la siguiente tabla para las SKR, **en el caso que no esté definido puedes añadirlo tú mismo indicando un pin de un endstop que no esté en uso** o en el csso que quieras cambiar el pin por defecto a otro porqué el original ha fallado \(en este caso intercambiar la definición de pines si hay mas de uno o si es ussdo en otra funcion\) ejemplo:  
>   
> Ejemplo:  
> SKR 1.4 / 1.4 Turbo - \#define FIL\_RUNOUT\_PIN P1\_26 \(E0DET\) o P1\_25 \(E1DET\)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Electr&#xF3;nica</th>
      <th style="text-align:left">Archivo pins</th>
      <th style="text-align:left">Pins</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>SKR v1.4 Turbo</b>
        </p>
        <p><b>SKR v1.4</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>lpc1768\pins_BTT_SKR_V1_4.h</p>
        <p>lpc1768\pins_BTT_SKR_V1_4.h</p>
      </td>
      <td style="text-align:left"><b>FIL_RUNOUT_PIN P1_26 (E0DET)<br />FIL_RUNOUT_PIN2 P1_25 (E1DET)</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>SKR v1.3</b>
      </td>
      <td style="text-align:left">lpc1768\pins_BTT_SKR_V1_3.h</td>
      <td style="text-align:left"><b>FIL_RUNOUT_PIN P1_28</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>SKR MINI E3 v2</b>
      </td>
      <td style="text-align:left">stm32f1\pins_BTT_SKR_MINI_E3_V2_0.h</td>
      <td style="text-align:left"><b>FIL_RUNOUT_PIN PC15 (E0-STOP)</b>
      </td>
    </tr>
  </tbody>
</table>

> **En caso de falsos positivos se puede jugar a habilitar o deshabilitar esta función ya que en según que escenarios puede ocasionar ciertos problemas:  
> \#define FILAMENT\_RUNOUT\_DISTANCE\_MM**  
> **También aconsejan en el caso de usar Sensorless/stallGuard doblar/desoldar/cortar el pin DIAG del driver para evitar falsos positivos.**

* En el caso de disponer de un sensor como el **BTT Smart Filament Sensor** que tiene función de detección de atascos deberemos asegurarnos de activar **`#define FILAMENT_MOTION_SENSOR // habilita sensores detección de atascos con encoder #define FILAMENT_RUNOUT_DISTANCE_MM 7 // 7mm es lo aconsejado por el fabricante y podemos ajustarlo en el caso de falsos positivos o dependiendo de nuestra máquina`**
* Algunos finales de carrera requieren de eliminar el PULLUP de la configuración de Marlin para operar correctamente. Si es tu caso, debes editar esta linea y eliminar los comentarios: \#define FIL\_RUNOUT\_PULLUP o viceversa dependiendo del tipo de placa \#define FIL\_RUNOUT\_PULLDOWN. Recuerda que solamente una de ellas debe estar habilitada.
* Activación y el parking del cabezal para el cambio de filamentos necesario para habilitar el sensor de filamentos y las opciones en pantalla para realizar el pausado y cambio de filamento **`#define ADVANCED_PAUSE_FEATURE - en configuration_adv.h #define NOZZLE_PARK_FEATURE - en configuration.h`**

## Comprobar que todo funciona correctamente

Usando Pronterface, Octoprint u otro cliente terminal para conectar a nuestra impresora comprobaremos que el funcionamiento del sensor es el adecuado enviando el comando M119 que nos indicará el estado del sensor.

![](../../../.gitbook/assets/image%20%2861%29.png)

Si tenemos nuestro sensor con filamento deberá aparecer TRIGGERED, si por el contrario no tenemos filamento aparecerá OPEN. En el caso que no sea correcto tendrás que cambiar la lógica cambiando estas lineas

> \#define FIL\_RUNOUT\_INVERTING true o false  
> o para 2.0.6 o superiores  
> \#define FIL\_RUNOUT\_STATE HIGH o LOW

## **Cómo ajustar el comando M600 para el cambio de filamentos:**

Cuando hacemos un cambio de filamento el comando M600 \(función ADVANCED\_PAUSE\_FEATURE\) es el que se encarga del proceso. En configuration\_adv.h podemos encontrar algunas funciones interesantes para personalizarlo a nuestra máquina, aunque por norma general y salvo contadas excepciones nos funcionará sin problemas como viene de serie.

* FILAMENT CHANGE UNLOAD LENGTH con él controlaremos qué distancia de filamento haremos retroceder. Normalmente usaremos la distancia entre la salida del nozzle y los engranajes de nuestro extrusor más un poco de margen para asegurar que sale completamente del engranaje.
* Es importante que si esta distancia es superior a EXTRUDE\_MAXLENGTH \(configuration.h\) deberemos ajustar este parámetro al que necesitemos para nuestra retracción de cambio de filamento.
* FILAMENT CHANGE SLOW LOAD FEEDRATE con este parámetro indicaremos con qué velocidad realizara la carga inicial de filamentos.
* FILAMENT CHANGE SLOW LOAD LENGTH al igual que para la retracción aquí le indicaremos los mm de filamento a mover en la carga inicial de filamentos, normalmente el valor por defecto es el correcto
* FILAMENT CHANGE FAST LOAD FEEDRATE en este caso indicaremos a la velocidad que realizará la carga del filamento hasta el nozzle
* FILAMENT CHANGE FAST LOAD LENGTH usaremos una medida similar a la de UNLOAD menos SLOW LOAD LENGTH

Estos son los valores más importantes a tener en cuenta.

