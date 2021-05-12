---
description: >-
  La puesta en marcha de una impresora así como el calibrado preventivo es muy
  importante para un correcto funcionamiento y precisión de nuestra impresora.
---

# Calibración inicial impresora 3D

![](../.gitbook/assets/image%20%2816%29%20%281%29.png)

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

{% embed url="https://www.buymeacoffee.com/jjr3d" %}

Aconsejamos el uso de SuperSlicer \(un fork de PrusaSlicer donde integran opciones avanzadas antes de incorporarlas\) ya que tiene unas estupendas opciones para generar los tests de calibración.

Puedes descargar SuperSlicer desde aqui:

{% embed url="https://github.com/supermerill/SuperSlicer/releases" %}

PrusaSlicer desde aqui :

{% embed url="https://github.com/prusa3d/PrusaSlicer/releases" %}

También si usas Cura puedes instalar este plugin que te permite generar y personalizar algunos de los tests de esta guía:

{% embed url="https://marketplace.ultimaker.com/app/cura/plugins/5axes/CalibrationShapes" %}

Por otro lado y recientemente han sacado un servicio online [3DOptimizer](https://3doptimizer.com/), de pago pero con posibilidad durante 7 dias para hacer los tests, que con unos tests te permite encontrar los valores y ajustes para tu slicer de forma bastante sencilla... muy aconsejable para hacer unos primeros ajustes a tu maquina.

{% embed url="https://www.youtube.com/watch?v=F31cEHaH4yo" %}

## 1. Ajuste vref Motores

VREF es simplemente el voltaje regulado que entregan nuestros drivers al motor. Un ajuste correcto del VREF nos permitirá una calibración precisa de nuestros motores.

Si nuestro VREF es muy bajo nuestro motor no tendrá suficiente torque y puede causar la temida pérdida de pasos en forma de pérdida de precisión y el efecto de capas desplazadas en nuestras impresiones.  
Si por el contrario nuestro VREF es muy alto nuestros motores puedes sobrecalentarse ocasionando efectos similares al punto anterior e incluso hacer fallar los motores o dañarlos.

### Cálculo de nuestro VREF óptimo

**En el caso que tengamos una maquina comercial y siempre que no modifiquemos sustancialmente su mecánica/cinemática, motores o no tengamos los problemas citados anteriormente \(perdida de pasos o sobrecalentamiento/fallo de los motores\) no es normalmente necesario realizar este ajuste.**

El calculo de VREF depende principalmente de dos factores... el tipo de drivers que usemos y los A soportados por nuestros motores. 

{% hint style="info" %}
Es muy importante que en el caso de cambio de drivers o motores tengamos en cuenta las características de los mismos y las capacidades de los drivers.   
Como ejemplo revisa la siguiente lista sobre los A soportados por estos tipos de drivers listando algunos a modo de ejemplo:

* A4988 -&gt; 2A
* DRV8825 -&gt; 2A
* TMC2208 -&gt; 1.2A
* TMC2209 -&gt; 1.7A

Debemos intentar tener en concordancia los A de nuestros motores con las capacidades de nuestros drivers para evitar problemas
{% endhint %}

Con estos datos y para que sea sencillo el cálculo os aconsejamos el uso de una calculadora online donde indicar los A de vuestro motor y el % de seguridad el cual es aconsejable colocar entre 80-90%

{% embed url="https://mvo46.csb.app/" %}

Ajustar el VREF en nuestra impresora

Para el ajuste del VREF tenemos dos opciones dependiendo de como nuestros drivers esten instalados/configurados:

#### En modo STANDALONE... usando potenciómetro y disponible en la mayoria de drivers aunque a día de hoy no tan aconsejable

Para drivers en STANDALONE depende del driver deberemos medir en unos puntos con nuestro tester/multimetro, a modo de ejemplo podéis consultar la siguiente tabla donde se incluye una descripción de diferentes drivers, sus caracteristicas/configuracion y como medir el VREF:

![](../.gitbook/assets/image%20%2869%29.png)

{% hint style="info" %}
**MUY IMPORTANTE!!! teniendo el tamaño y accesibilidad de los puntos donde medir ir con extremo cuidado, usar un destornillador cerámico para el ajuste del tornillo y girar este en pasos muy muy pequeños.**
{% endhint %}

#### **En modo UART... ajuste recomendable si nuestros drivers/placa lo soportan**

El modo UART o modo gestionado dinámico de drivers es el más adecuado para poder aprovechar al máximo nuestros drivers siempre que estos y nuestra placa los soporten y estén configurados en nuestro Marlin.

Para ajustar el VREF en este caso es muy sencillo y básicamente tenemos 3 opciones:

* **Usando nuestra pantalla LCD/TFT**, en pantallas LCD modo Marlin dispondremos de un menu Configuracion/Avanzado/TMC donde ajustar la corriente antes calculada o simplemente ajustarla a nuestro gusto. Para pantallas TFT depende de cada fabricante en que parte del interfaz dispongan de esta configuración. En cualquier caso es importante salvar a EEPROM una vez hecho el cambio \(modo Marlin menú Configuracion/Salvar EEPROM\) para evitar que este se pierda al reiniciar la impresora.
* **Mediante gcode usando un cliente terminal** como Pronterface/Octoprint o algunas pantallas TFT. Para ello enviaremos el comando M906. M906 X5 Y5 Z5 por ejemplo ajustariá la corriente de los ejes X Y Z a 5ma \(500\)... al igual que el punto anterior es importante salvar los cambios en EEPROM con un M500 
* **En los ficheros de Marlin**, en configuration\_adv y dependiendo de nuestros drivers tenemos la configuración... os aconsejamos revisar la documentación de vuestra placa para encontrar donde se ubican exactamente los cambios

En este caso es muy aconsejable realizar un M503 y/o un M122 para verificar que los datos estan correctmente.

## 2. Extrusión

Una parte crítica en una impresora 3D es el control del filamento extruido.  
Este proceso no es en un eje de movimiento si no de extrusión así que se aconseja que el test se realice extruyendo filamento por el nozzle.  
El proceso básicamente es calentar el Nozzle a temperatura que aconseja el fabricante y a partir de un punto de referencia, normalmente a la entrada del filamento en el extrusor, realizar dos marcas... una a 100mm y otra a 120mm.

![](../.gitbook/assets/image%20%281%29.png)

Una vez realizadas estas marcas extruimos 100mm de filamento utilizando los menus de Movimiento en pantalla o mediante Pronterface.  
Al finalizar la extrusión mediremos desde el punto de referencia hasta la marca de 120mm y ese valor lo usaremos en la calculadora \(_**pulsa sobre el icono Run Pen**_\):

{% embed url="https://codepen.io/alienboyxp/embed/BaQeXPO" %}

Tambien teneis este vídeo del Sr. Ferrete donde explica el proceso de forma sencilla.

{% embed url="https://youtu.be/VA\_jvXyGvhU" %}

{% hint style="info" %}
Si en lugar de realizar el test extruyendo en caliente queréis hacerlo al "aire"/frio sin extruir deberéis desconectar el extrusor del hotend o desmontar este y usar el comando M301 P1 o S0 para poder extruir en frio.
{% endhint %}

El ajuste de los pasos los puedes realizar de diferentes formas:

* desde la pantalla en modo Marlin en Configuración/Avanzado/Steps
* mediante gcode desde un terminal como Pronterface con M92 Exxx \(donde xxx serían los pasos calculados previamente\).

> _**Recuerda, si tienes un Marlin cocinado por ti anotar estos valores, y si modificaste por pantalla ir a Configuración/Salvar EEPROM o si lo hiciste por terminal lanzar un M500.**_

## 3. Ajuste de los pasos de los motores/ejes

Los cubos de calibración son simples objetos geométricos \(cubos normalmente\) que ayudan en la calibracion fina de tu impresora 3D permitiendo conseguir la mayor precisión en tus impresiones.

![](../.gitbook/assets/image%20%2821%29.png)

Una vez tu extrusor está calibrado, hay diferentes cosas que puedes comprobar mediante un cubo de calibración.  
En este paso lo usaremos principalmente para calibrar los pasos de nuestros motores.  
Al finalizar el resto de pasos de ests guía es aconsejable volver s realizar este test para verificar o corregir cualquier desviación.

Puedes generar estos cubos de forma sencilla desde SuperSlicer en su menú de calibración.  
En todo caso te adjuntamos algunos otros de ejemplo: 

{% embed url="https://www.thingiverse.com/thing:3911284" %}

{% embed url="https://www.thingiverse.com/thing:1545913" %}

Puedes usar calculadoras de pasos para proceso de cálculo o verificado de pasos \(_**pulsa sobre el icono Run Pen**_\):

{% embed url="https://codepen.io/alienboyxp/pen/BaQeXPO" %}

https://blog.prusaprinters.org/calculator\_3416/\#steppermotors

El ajuste de los pasos los puedes realizar de diferentes formas:

* desde la pantalla en modo Marlin en Configuración/Avanzado/Steps
* mediante gcode desde un terminal como Pronterface con M92 Xxxx Yxxx Zxxx \(donde xxx serían los pasos calculados previamente\).

> **Recuerda si tienes un Marlin cocinado por ti anotar estos valores, y si modificaste por pantalla ir a Configuración/Salvar EEPROM o si lo hiciste por terminal lanzar un M500.**

## 4. Ajuste PID

Esta calibración es independiente del resto, dado que no requiere que se hayan reslizado otros tests previamenteEjemplo de impresora con los valores PID ajustados incorrectamente.

![](../.gitbook/assets/image%20%2820%29.png)

Antes de meternos en lo que es la calibración en sí, sería bueno entender que es el PID, lo cual básicamente es un algoritmo que ayuda a la máquina a mantener los valores de temperatura deseados tanto en el Nozzle como en la Cama caliente, para ello tiene una serie de valores que ajustan la cantidad de corriente que llega a los calentadores de estos y la varía en función de las lecturas recogidas por los termistores \(sensores de temperatura\).

En el caso de la cama os aconsejamos ver el siguiente video para entender la importancia de usar o no PID en la cama.

{% embed url="https://www.youtube.com/watch?v=9JyydfcOcD0" %}

Para realizar este ajuste podemos realizarlo de dos formas distintas:

* Desde el LCD dentro del menú Temperatura o Configuración/Avanzado

> Recordar habilitar en Marlin las siguientes opciones:  
> \#define PID\_EDIT\_MENU  
> \#define PID\_AUTOTUNE\_MENU

* Desde terminal como Pronterface M303 E0 S200 C8 - hotend M303 E-1 S60 C8 - cama caliente donde Sxxx es la temperatura Cxx es el número de ciclos de test

> En el caso que no deje realizar el PID de la cama \(M303 E-1\) es muy probable que no esté habilitado en Marlin:  
> \#define PIDTEMPBED

Idealmente realizar el PID con la temperatura normal de impresión y ventilación de capa activada al 100%, la cama a la temperatura normal de impresion y la algura de Z a 5mm.

Recordar que los valores obtenidos deberían de ser anotados en tu Marlin cocinado o desde gcodes  
M301 P19.56 I0.71 D134.26 - hotend  
M304 P1 I2 D3 - cama

![](../.gitbook/assets/image%20%2813%29.png)

Y recordar siempre un M500 para almacenar los valores en la EEPROM.

En caso que por protección térmica no permita hacer el autoPID deshabilitar momentáneamente:

```cpp
#define THERMAL_PROTECTION_HOTENDS
#define THERMAL_PROTECTION_BED
```

En el caso de alarmas por protección de temperaturas que no se solucione con un PID se tendrán que variar los parámetros de tiempo e histéresis de dicha protección.

{% embed url="https://3dprinting.stackexchange.com/questions/8466/what-is-thermal-runaway-protection" %}

## 5. Nivelación de la cama

El nivelado de cama es crítico para poder disponer de una buena base para nuestras impresiones.

![](../.gitbook/assets/image%20%286%29.png)

Podemos usar este test de nivelación o uno similar [https://www.thingiverse.com/thing:34558](https://www.thingiverse.com/thing:34558) y usando babystepping ajustar correctamente el nivelado de la cama para una correcta primera capa.

De nuevo desde SuperSlicer y su menú de calibración podemos generar este test de forma sencilla o desde [https://teachingtechyt.github.io/calibration.html\#firstlayer](https://teachingtechyt.github.io/calibration.html#firstlayer)

![](../.gitbook/assets/image%20%2811%29.png)

En el caso de ser necesario ajustar el ZOffset a partir del valor obtenido \(en el caso de disponer de sensor de nivelación o MESH como sistema de nivelación\) sumaremos el vslor de babbystepping a nuestro ZOffset.

Podéis ver una explicación en forma de video aqui 

{% embed url="https://youtu.be/1V6TZ7fDiU4" %}

## 6. Torre de temperatura

Las torres de temperaturas nos permiten encontrar la temperatura óptima para un determinado filamento.  
Es importante hacer este test con cada bobina que usemos para ajustar o verificar que los valores son los correctos.

![](../.gitbook/assets/image%20%283%29.png)

Es aconsejable utilizar/generar el test de Temperatura de SuperSlicer o desde el [generador de Teaching Tech](https://teachingtechyt.github.io/calibration.html#temp)  
Otros ejemplos de torres de temperaturas:

{% embed url="https://www.thingiverse.com/apps/customizer/run?thing\_id=2491884" %}

{% embed url="https://www.thingiverse.com/thing:2729076/files" %}

Ajustar con el customizer de Thinkiverse o manualmente en el slicer

![](../.gitbook/assets/image%20%287%29.png)

Para interpretar las torres de temperaturas deberemos fijarnos en:

* Acabado de los puentes, que no queden descolgados
* Definición de los números de la temperatura, estos deben quedar lo mas legibles posible además del efecto "olas" alrededor de los mismos
* Figuras cónicas que esten bien definidas sin restos de material o deformaciones

## 7. Ajuste Flujo/Flow

El ajuste de flujo permite ajusta la cantidad de plástico extruído por la impresora. Una correcta calibración del flujo/flow permite solucionar problemas de falta o sobre extrusión además de mejorar los valores de retracción, ayudar a mejorar las esquinas y el efecto costura en nuestras impresiones.

![](../.gitbook/assets/image%20%2822%29.png)

Como paso previo al ajuste de flujo/flow es imprescindible que previamente tengamos correctamente ajustasdos los pasos de nuestros motores, encontrada la temperatura adecuada para nuestro filamento y el PID.

Para realizar este ajuste podemos realizarlo mediante un cubo de calibración de flujo/flow en este caso necesitaremos un calibre digital a ser posible o podemos usar el test de flujo/flow de SuperSlicer que no es tan exacto pero sirve perfectamente para realizar este ajuste normalmente.

Un paso previo muy importante para una correcta calibración es realizar una comprobación del diámetro de nuestro filamento. Aunque el filamento que compramos normalmente indique 1,75mm en la realidad depende de la calidad del fabricante que este sea así cuando lo normal seria una desviación de 0.02mm en buenas marcas nos podemos encontrar con desviaciones de hasta/o más del 0.05mm.  
Para realizar esta comprobación mediremos 5 secciones de filametos con una distancia de unos 10cm en cada una y realizaremos una media.

![](../.gitbook/assets/image%20%2829%29.png)

Una vez tengamos este valor medio para este filamento lo ajustaremos en nuestro perfil de filamento en nuestro fileteador/slicer.  
**PrusaSlicer/SuperSlicer: Filament Settings -&gt; Filament -&gt; Diameter  
Cura: Preferences -&gt; Printers -&gt;** _**your printer**_ **-&gt; Machine Settings -&gt; Extruder 1**

El siguiente paso es imprimir un cubo "hollow" \(hueco\), os sugerimos usar este...

{% embed url="https://www.thingiverse.com/thing:3397997" %}

...ya que cuenta con ejemplos de practicamente todas las medidas de nozzle que hay y es muy rápido de hacer.

![](../.gitbook/assets/image%20%2815%29.png)

1- Escogeremos de la los diferentes modelos el que coincida con el diámetro de nuestro nozzle  
2- Configuraremos los siguientes valores básicos de la impresión:  
`Altura capa - 0.2mm (para un nozzle de 0.4)  
Perimetros - 2  
Capas superiores - 0  
Capas inferiores - 1  
Relleno - 0%  
Velocidad - 50 mm/s (puedes adaptarlo a tu maquina)  
Flow/Flujo/Multiplicador Extrusion - 1 o 100% dependiendo del fileteador/slicer`

Una vez impreso mediremos el grosor de las paredes del cubo.

![](../.gitbook/assets/image%20%2812%29.png)

Estas deberían medir lo más cercano al doble de la medida del nozzle que tengamos o seleccionamos al hacer el cubo. Es importante no aplicar mucha presión al calibre y realizar la medida en diferentes partes del cubo y en la parte alta del cubo realizando una media de todas las medidas.

Con estos valores realizaremos la siguente formula:  
\(A/B\)\*F= Nuevo valore de flujo/flow  
A= Medida deseada, para el caso nozzle 0.4 debería de ser 0.8  
B= Medida real, media de las diferentes medidas  
F= Valor de flujo/flow aplicado en el test... en el caso de % si el valor era 100% usaremos 1, 98% seria 0.98...

{% embed url="https://codepen.io/alienboyxp/pen/bGByXXJ" %}

Una vez tenemos el valor nuevo volvemos a repetir el test con el nuevo valor de flujo/flow y volvemos a medir hasta conseguir las medidas más exactas posible.

El valor de flujo/flow en nuestro fileteador/slicer lo podemos encontrar en...  
**PrusaSlicer/SuperSlicer - Extrusion Multiplier  
Cura - Flow**

Ultimas recomendaciones sobre el flujo/flow...

* A veces es muy difícil obtener unas correctas dimensiones de los muros, dada la naturaleza de la impresión FDM es complicado obtener unos valores exactos y repetibles
* El test de flujo no es necesario en cada bobina de un mismo tipo/color de filamento pero si seria aconsejable realizarlo en cada tipo y color de filamento para tenerlo como referencia ya que los valores de flow pueden cambiar significativamente sobretodo por tipo de filamento \(PLA/PLA+/SPLA/PETG/TPU/ABS/etc...\)

## 8. Retracciones

La retracción es el movimiento de retroceso del filamento necesario para evitar goteos de material durante los movimientos y desplazamientos que realiza el extrusor en vacío durante la impresión 3D.

Los parámetros que configuran a la retracción son:

* **Distancia de retracción**: Longitud de material que retrocede en el proceso de retracción. Varía en función del tipo de material, el tipo de sistema de extrusión \(Directo o Bowden\) y del tipo de HotEnd. Para materiales flexibles, sobre todo para los tipo TPE \(Filaflex\), se debe desactivar la retracción para evitar que el filamento se enrolle en el piñón del extrusor.
* **Velocidad de retracción**: Velocidad a la que el motor del extrusor hace retroceder al filamento. Con este parámetro hay que tener mucho cuidado si se utilizan velocidades altas \(mayores a 70 mm/s\) porque puede mellar \(marcar\) el filamento de tal modo que quede inservible para continuar la impresión 3D.
* **Desplazamiento mínimo**: Longitud mínima a partir de la cual se quiere que se realice la retracción.
* **Enable combing**: Al activar este parámetro, que se encuentra en apartado de opciones avanzadas de retracción del programa de laminación que se utilice \(PrusaSlicer/SuperSlicer,Cura, Simplify3D, etc.\), aparte de realizar la retracción, se evita que el HotEnd se mueva por encima de orificios o huecos. Con esto se evitan restos de material en las caras vistas de partes internas de las piezas.
* **Elevación del eje Z al retraerse \(Lift z\)**: A la vez que se produce la retracción, el HotEnd se mueve en el eje z a la distancia indicada. Esta elevación solo es necesaria en caso de realizar piezas con muchos detalles y con zonas pequeñas de mucho detalle para evitar que queden restos de material justo en esa zona. En caso de necesitar utilizar este parámetro recomendamos utilizar la misma distancia que la altura de capa.
* **Humedad en el filamento**: algo crítico para este parámetro ya que es muy importante para el acabado final que nuestro filamento esté a 10% de humedad. Esto lo podemos conseguir con un correcto almacenado del filamento y el uso de una deshidratadora de alimentos por ejemplo.

Desgraciadamente no existe una fórmula para encontrar el valor exacto, si no que cada impresora 3D y cada extrusor necesita un valor particular. 

La siguiente tabla contiene unos valores aconsejados de longitud y velocidad de retracción para la impresora 3D en función del tipo de extrusor que se utilice, los cuales son un buen punto de partida para ajustarlos a vuestro caso particular:

![](../.gitbook/assets/image%20%2838%29.png)

Puedes utilizar el generador online [https://teachingtechyt.github.io/calibration.html\#retraction](https://teachingtechyt.github.io/calibration.html#retraction) o desde [SuperSlicer](https://github.com/supermerill/SuperSlicer/releases) añadieron en sus últimas versiones el test de retracciones que facilita mucho el proceso ademss de simplificar el ajuste de parámetros a la distancia de retracción y temperatura ya que la velocidad de retracción tiene un menor impacto en el resultado final.

![](../.gitbook/assets/image%20%2826%29.png)

Si quieres generar el tuyo propio a mano puedes usar 

{% embed url="https://www.thingiverse.com/thing:3542985" %}

## 9. Test de tolerancia horizontal

El test de tolerancia horizontal permite ajustar el valor de compensación para minimizar la expansión del material y que las piezas que son encajables lo hagan correctamente sin necesidad de rehacer el diseño o modificar las dimensiones de las piezas y su tolerancia.

![](../.gitbook/assets/image%20%288%29.png)

Puedes utilizar este test muy rápido para encontrar los valores adecuados de tu filamento.

{% embed url="https://www.thingiverse.com/thing:1662342" %}

Dependiendo del slicer este ajuste se llama Expansión horizontal o Compensación XY.  
También slicers como PrusaSlicer/SuperSlicer o Cura disponen de opciones avanzadas de ajustes para que esta compensación se aplique al contorno \(outer\) o en partes internas \(inner/hole\). Para no afectar a la precisión en medidas de la pieza, si nuestro ajustes de pasos/ejes están correctos, es mas aconsejable ajustar solamente ls compensación interna \(inner/hole\).

## 10. Ajuste de voladizos/puentes

Este ajuste es doblemente importante, por una lado nos permite obtener unos mejores resultados en el aspecto de nuestras impresiones y por otro el ahorro de tiempo minimizando el uso de soportes.

![](../.gitbook/assets/image%20%2824%29.png)

Es aconsejable tener correctamente ajustados todos los pasos previos de esta guía ya que algunos de ellos afectan considerablemente al resultado del mismo.

Como comentabamos los voladizos permiten reducir el numero de soportes, para ello deberemos encontrar el valor del ángulo que nuestra impresora puede imprimir sin que se produzcan aberraciones o caídas en las capas.  
Para ello a parte del test específico es bueno recordar dos puntos importantes...  
... reducir el ancho de línea mejora el resultado final por lo que nozzles de tamaño standard/pequeño 0.4-0.2 darán un mejor resultado que aquellos más grandes  
... ventilación de capa es otro punto muy importante, no solo el flujo de aire que podamos lanzar sobre la creación de voladizos si no que la tobera que dirige dicho flujo lo haga orientada al punto exacto y desde al menos 2 o más direcciones  
... optimizar las piezas en el diseño si el objetivo es la impresión FDM, esto es el uso de chaflanes 45 grados en lugar de angulos rectos  
... en situaciones más extremas el cortal la pieza en varias partes puede darnos buen resultado

Para realizar el test os sugerimos el uso de esta torre de voladizos

{% embed url="https://www.thingiverse.com/thing:2972495" %}

El valor que encontremos deberemos configurarlo en nuestro fileteador/slicer. En PrusaSlicer podrás encontrarlo en Overhang thresold.

![](../.gitbook/assets/image%20%2810%29.png)

Relacionado con los voladizos es interesante ajustar los parámetros de nuestro fileteador/slicer para poder imprimir puentes sin que estos queden de forma incorrecta.

El siguiente test nos puede ayudar para 

{% embed url="https://www.thingiverse.com/thing:546688" %}

Este test realiza puentes de 10mm hasta 100mm, el objetivo seria imprimirlos a diferentes velocidades para ver cual es el que mejor se adapta a las caracteristicas de nuestra impresora.  
Entre estas caracteristicas estarian la de velocidad al realizar puentes... normalmente los valores ideales dependiendo de la impresoras estarían entre 30-80 mm/seg siendo 40mm/seg un buen punto de partida:

![Opciones SuperSlicer](../.gitbook/assets/image%20%2823%29.png)

Otro valor a revisar es flujo de aire del ventilador en puentes, normalmente es aconsejable 100% pero en algún tipo de material como el PETG es aconsejable bajarlo un poco para evitar atascos.

## 11. Test de Soportes

Normalmente imprimimos piezas complejas las cuales por simple física requieren imprimir al "aire" capas que es imposible que puedan sustentarse.

Para solventar este problema los slicers permiten la generación de soportes que ayudan a mantener estas capas impresas al aire en el sitio adecuado.

Estos soportes sin una correcta configuración pueden dar dos problemas, el primero que sean tan complicados de extraer que dañen la figura y el segundo que la pieza no quede uniforme en esas partes.

![](../.gitbook/assets/image%20%282%29.png)

Que podemos hacer para mejorar estos soportes::

Imprimir a una temperatura correcta es clave para que el acabado general sea óptimo y en especial el de las partes que necesiten soporte. Así que es ideal que revisemos la temperatura optima de nuestro filamento.

![](../.gitbook/assets/image%20%2827%29.png)

Otro punto importante son las capas que hacen de interfaz entre las torres de los soportes y la pieza a imprimir, normalmente podemos especificar el numero de capas de la misma y tendremos que encontrar el balance perfecto para nuestra pieza donde normalmente a mas gruesa es más difícil que estas se adhieran a la pieza.

![](../.gitbook/assets/image%20%284%29.png)

También importante es la distacia de estas capas de interfaz hasta la pieza donde nos encontramos que a menor distancia entre ellas obtenemos un mejor acabado pero más complicado el poder extraerlas incluso que acaben dañando a la pieza.

![](../.gitbook/assets/image%20%2814%29.png)

Como es coger los mejores valores? no podemos dar unos valores universales pero si un posible punto de partida el cual podremos acabar de identificar los valores correctos con tests específicos:

* distancia de las capas interfaz de soportes 90% de altura de capa tanto superior como inferior
* patrón rectilineo con 1.5-2.5mm de espaciado dependiendo de la pieza
* separación XY entre 70-100% altura capa
* número de capas de interfaz 3-5

Os aconesjamos el siguitente test que podéis utilizar como referencia ya que permite comprobar de una forma rápida nuestros valores tanto de soportes en cama como aquellos que descansan sobre la propia pieza.

{% embed url="https://www.thingiverse.com/thing:2755063" %}

![](../.gitbook/assets/image%20%285%29.png)

## 12.Linear Advance \(opcional\)

Es una característica muy interesante de Marlin, que mantiene constante la presión del filamento dentro del nozzle.

![](../.gitbook/assets/image%20%2825%29.png)

Al tener una presión constante, no le afectan los cambios de velocidad durante la impresión, por lo que las zonas conflictivas salen mejor. Se nota una gran mejoría en la pieza en general, pero sobre todo en las esquinas.

Otra cosa buena de esta característica, teniendo bien configurado el Linear Advance, te puedes olvidar del jerk/junction \(una característica relacionada con la aceleración\) y puedes imprimir a más velocidad sin teóricamente perder calidad.

Por último, otra de las ventajas es que las dimensiones de las piezas que imprimáis teóricamente serán más exactas.

Es importante recordar que esta funcionalidad, por lo menos hasta la versión 2.0.7.x donde asegurate de habilitar **\#define EXPERIMENTAL\_SCURVE**, no se lleva muy bien con S\_CURVE que son unos algoritmos que mejoran arcos y circunferencias.

Activarlo es muy sencillo, sólo tenéis que ir al archivo configuration\_adv.h de vuestro Marlin y descomentar esta línea:

![](../.gitbook/assets/image%20%289%29.png)

Con la característica activa, vamos a ver como se configura el parámetro K, que es que el gestiona la presión dentro del nozzle.

Para ello, en la propia página de Marlin, hay una [herramienta](http://marlinfw.org/tools/lin_advance/k-factor.html) para generar un patrón de configuración/test.

Desde cualquiera de las herramientas anteriores podéis generar el Gcode correspondiente, lo bajáis y lo imprimís.

Al imprimirlo obtendréis algo parecido a esto:

![](../.gitbook/assets/image%20%2828%29.png)

A la derecha os aparece el valor de la K, y debéis elegir la línea que sea más constante a lo largo de toda la longitud de la misma. En mi caso elegí una K de 140, puesto que debe ser lo menor posible.

Como la K cambia dependiendo del material utilizado, lo mejor es cambiar la K mediante un Gcode cuando vas a imprimir. Para ello en el slicer que uses, se añade el Gcode que la configura en el script inicial de la impresión o en el script de filamentos \(la mejor opción si tu slicer lo tiene, PrusaSlicer/SuperSlicer por ejemplo\).  
M900 K140  
Donde 140 es el valor elegido del test anterior.

Podeis ver mas información en el siguiente vídeo 

{% embed url="https://youtu.be/\_BiqlXPPfu4" %}

## 13. Ajustes de Aceleración y Jerk \(opcional\)

Permiten encontrar el punto óptimo para tu impresora en aceleración y Jerk/Junction.

![](../.gitbook/assets/image%20%2817%29.png)

Te aconsejamos crearlo desde este generador online [https://teachingtechyt.github.io/calibration.html\#accel](https://teachingtechyt.github.io/calibration.html#accel) o puedes usar este otro y personalizarlo [ttps://www.thingiverse.com/thing:4169896](https://www.thingiverse.com/thing:4169896)

##  14. Ajuste de Extrusión Volumétrica \(Opcional\)

El extrusor de una impresora 3D puede fundir una cantidad finita de filamento en x tiempo. Esta velocidad de filamento máximo extruído limita las capacidades de nuestra impresora lo que a veces se traduce en que durante una impresión en determinadas partes nuestro extrusor pierde pasos y la calidad de la impresión no es la óptima.

Los fileteadores/slicers actuales como PrusaSlicer/SuperSlicer permiten usar este valor junto con la máxima velocidad mecánica de la cinemática de una impresora 3D para ajustar dinámicamente la velocidad de impresión para obtener siempre el máximo rendimiento y calidad.

![](https://telegra.ph/file/bd32544eefb6474392690.png)

![](https://telegra.ph/file/e4af023a8bf18c6d9d9bb.png)

**Procedimiento para encontrar nuestro MVS**

Nos conectaremos con nuestro cliente terminal favorito como Pronterface o Octoprint para poder lanzar comandos gcode.

1. Podremos el extrusor en modo relativo y ajustaremos la temperatura del nozzle a la temperatura adecuada para el filamento a usar, es aconsejable antes elevar el eje Z hasta media altura de su eje M83 M109 S200
2. Seguidamente lanzaremos el comando para extruir 60mm de filamento a diferentes velocidades G1 E60 F300 G1 E60 F400
3. Incrementaremos la velocidad de 100mm/min \(el valor Fxxx\) hasta que escuchemos que nuestro extrusor empieza a perder pasos
4. Al notar que perdamos pasos bajaremos la velocidad en 50mm/min
5. Iremos incrementando 10mm/min hasta que volvamos a escuchar perder pasos de nuevo
6. Bajaremos 5mm/min sobre el valor anterior

Una vez ya hemos obtenido el valor de máxima velocidad de extrusión sin pérdida de pasos del extrusor realizaremos un par de pruebas para verificar que es el valor correcto y no perdemos pasos.  
Con ese valor realizaremos los siguientes cálculos:

* Dividiremos la velocidad Fxxx óptima para nuestro filamento/extrusor/temperatura por 60 para convertir de mm/min a mm/seg
* Multiplicaremos la velodidad de extrusion máxima del punto anterior por la sección de nuestro filamento, ejemplo 1.75 ya que es el más común para obtener el valor de mm3/seg
* Por seguridad reduciremos los mm3/seg en .5 para dejar algo de márgen de hardware y de variaciones de filamento

Como referencia aquí podéis consultar algunas medidas:

![](https://telegra.ph/file/b89c6a19210b8e5e5331f.png)

![](https://telegra.ph/file/8c789de93e68e6608ab2d.png)

![](https://telegra.ph/file/ae82a823d6dd3bbb13825.png)

Ajustando PrusaSlicer/SuperSlicer para usar MVS

Como ya comentamos este potente ajuste es muy sencillo de aplicar a nuestro slicer/fileteador. En el caso de PrusaSlicer/SuperSlicer:

* En nuestro perfil de filamentos ajustaremos Max Volumetric Speed

![](https://telegra.ph/file/60e11d1db5810a6001304.png)

* En nuestro perfil de impresion ajustaremos la máxima velocidad de nuestra mecánica/cinemática

![](https://telegra.ph/file/085feffeac9637e524680.png)

Ya está!!! ahora al realizar un fileteado podemos ver en la preview el resultado viendo el preview de velocidad y volumetríaPreview Volumetría

![](https://telegra.ph/file/f226d3bf7ac3bda5b2524.png)

![](https://telegra.ph/file/37bad1d77f3dd3c30af26.png)

## 15. Configurando nuestro gcode de inicio y fin de impresion

Una vez tenemos todo perfectamente calibrado es importante qeu nuestra impresora realice los pasos correctos a la hora de comenzar y finalizar nuestra impresión

### Gcode de inicio de impresión

Os sugerimos el siguiente gcode de inicio \(Prusa/SuperSlicer al final de la explicación encontrarás como adaptarlo a Cura\):

```groovy
; 3DWORK.IO Custom Start G-code
M117 PRE-HEATING!!!
M104 S[first_layer_temperature]   ; Set Extruder temperature
M140 S[first_layer_bed_temperature] ; Set Heat Bed temperature
M190 S{first_layer_bed_temperature[0] - 5}  ; wait for bed temperature - 5
M109 S[first_layer_temperature]             ; wait for nozzle temperature
M117 HEATING!!!
G4 S120 ;wait to preheat

; Printer Homing
M117 HOMING!!!
G28 ; Home all axes
G29 L1 ; Autolevel UBL
G29 J ; Autolevel UBL
; G29 ; Autolevel BILINEAR
; M420 S1 Z10 ; Autolevel MESH Fade 10 mm

; Purge Extruder
M117 CLEANING EXTRUDER!!!
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
```

Este gcode de inicio se divide en tres bloques imprescindibles para iniciar una impresión correctamente:

* **Calentado**, en este primer bloque usamos la configuración del slicer/fileteador para precalentar durante 120 segundos nuestro nozzle y cama \(puedes modificar esto modificando el G4 S120 cambiando el valor o comentando/borrando el comando\)
* **Homing y Nivelado**, este segundo bloque realizará un homing de nuestros ejes y en el caso de tener autonivelación realizarla \(el ejemplo esta hecho para UBL pocemos descomentar o borrar las lineas que nos sean o no útiles en nuestro caso\)
* **Purga**, aunque a mucha gente no le gusta o hacen una desde el slicer/fileteador alrededor de la pieza es preferible hacer esta linea de purga ya que permitira limpiar todo lo mas posible el nozzle para que comience el proceso de impresión en las mejores condiciones. Para ajustar el tamaño de la linea de purga, esta se realiza en la parte derecha de la cama en el eje Y, ajustaremos el valor de Y200 al tamaño de nuestra cama -10mm

En el caso de Cura modificaremos el primer bloque de la siguiente forma:

```groovy
; 3DWORK.IO Custom Start G-code
M117 PRE-HEATING!!!
M104 S{material_print_temperature_layer_0} ; Set Extruder temperature
M140 S{material_bed_temperature_layer_0} ; Set Heat Bed temperature
M190 S{material_bed_temperature_layer_0} ; Wait for Heat Bed temperature
M109 S{material_print_temperature_layer_0} ; Wait for Extruder temperature
M117 HEATING!!!
G4 S120 ;wait to preheat
```

### Gcode de final de impresión

Tan importante como el Gcode de inicio es el de final que nos asegurará apagar de una forma correcta nuestra impresora y dejará correctamente preparada para el siguiente uso.

```groovy
; 3DWORK.IO Custom End G-code

G4 ; Wait
M220 S100 ; Reset Speed factor override percentage to default (100%)
M221 S100 ; Reset Extrude factor override percentage to default (100%)
G91 ; Set coordinates to relative

M117 RETRACT TO AVOID OOZING!!!
G1 F1800 E-3 ; Retract filament 3 mm to prevent oozing
G1 F3000 Z20 ; Move Z Axis up 20 mm to allow filament ooze freely

M117 My Lord, your print ready!!!
G90 ; Set coordinates to absolute
G1 X0 Y220 F1000 ; Move Heat Bed to the front for easy print removal

M117 ENDING...
M106 S0 ; Turn off cooling fan
M104 S0 ; Turn off extruder
M140 S0 ; Turn off bed
M107 ; Turn off Fan
M84 ; Disable stepper motors

M117 That's All Folks!
```

Este gcode realiza los siguientes pasos:

* **Retracción**, para evitar que en la siguiente impresion caiga filamento durante el calentamiento, podemos variar el valor de E-3 a los mm que queramos para ajustarlo
* **Presenta la cama con la impresión finalizada**, puedes ajustar Y220 al valor máximo de tu eje Y -10 mm
* **Apagado** ventiladores, extrusor, cama y motores

## Tests en cambio filamento/color sugeridos <a id="Prueba-de-stress-finall!!!"></a>

Muchas veces al cambiar de tipo de material y/o color o marca de fabricante comenzamos a imprimir sin realizar un mínimo de tests que pueden llevarnos a resultados, a veces, desastrosos.

Para evitar este tipo de problemas os aconsejamos realizar los siguientes tests:

* [Torre de temperatura](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#6-torre-de-temperatura) que nos permitirá verificar o encontrar la temperatura óptima para nuestro nuevo filamento
* [Ajuste de flujo y comprobación de pasos](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#7-ajuste-flujo-flow), usando el test de flujo que es un test muy rápido podemos rápidamente verificar que el flujo es correcto y que nuestros pasos de ejes no variaron con el nuevo filamento. En nuestro caso el cubo de flujo usado tiene unas medidas de 35x35x15mm \(X Y Z\). **`; Filament-specific start gcode  M92 X80.2586 Y80.4597 Z401.4765 ; Ajuste de pasos específico para PETG`**
* [Retracciones](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#8-retracciones), en muchas ocasiones este parámetro es muy subsceptible de cambiar y el test es rápido de realizar desde SuperSlicer o el generador de Teaching Tech
* [Test de toleranzia horizontal](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#9-test-de-tolerancia-horizontal), aconsejable realizarlo sobretodo si cambiamos el tipo de filamento o marca

Test **imprescindibles en caso de tener/usar estas funciones avanzadas**:

* [Linear Advance](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#12-linear-advance-opcional), en el caso que usemos LA es imprescindible realizar la comprobación y ajustar el nuevo valor
* [Extrusión Volumétrica](https://3dwork.qitec.net/guias-impresion-3d/calibracion_3d#14-ajuste-de-extrusion-volumetrica-opcional), al igual que el punto anterior MVS también queda muy influenciado por el tipo de material por lo que es mas que aconsejable realizar/comprobar que no varía con el nuevo filamento

Dado que la mayoría de laminadores/slicers nuevos disponen de perfiles para tipo de filamento/material lo ideal es crear uno nuevo para cada tipo de filamento y almacenar ahi cualquier cambio con respecto a otros ya sea con cambio de valores o, de permitirlo, mediante gcode especifico de filamento \(como en SuperSlicer/PrusaSlicer que le da una gran versatilidad y personalizacion\).

## Prueba de stress finall!!! <a id="Prueba-de-stress-finall!!!"></a>

Si has llegado hasta aquí y seguiste todos los pasos sugeridos correctamente tu impresora estará probablemente ajustada.Pars hacer una verificación final podemos usar una última pieza de calibración de stress que nos ayudará a comprobar que todo este correcto.

![](../.gitbook/assets/image%20%2816%29.png)

Este test ayudará a verificar;  
- test de voladizos, donde nos indicará en wue angulos nuestra impresora piede imprimir sin soportes para configurar en tu slider y evitar/minimizar el número de soportes necesarios  
- test de puentes, para ver los límites en distancia de puentes  
- test retracción  
- test definicion esquinas  
- test de tolerancia  
- test de escala  
https://www.thingiverse.com/thing:2975429

### Puedes encontrar más tests específicos en mi colección en Thingiverse: <a id="Puedes-encontrar-m&#xE1;s-tests-espec&#xED;ficos-en-mi-colecci&#xF3;n-en-Thingiverse:"></a>

### https://www.thingiverse.com/alienboyxp/collections/calibracion <a id="https://www.thingiverse.com/alienboyxp/collections/calibracion"></a>

## Perfiles de Impresión que podemos usar como referencia <a id="Perfiles-de-Impresi&#xF3;n-que-podemos-usar-como-referencia"></a>

### Creality Ender <a id="Creality-Ender"></a>

* PrusaSlicer/SuperSlicer: [https://www.chepclub.com/prusaslicer-profiles.html](https://www.chepclub.com/prusaslicer-profiles.html)
* Cura: [https://www.chepclub.com/cura-profiles.html](https://www.chepclub.com/cura-profiles.html)
* Start/End Gcodes \(Cura\): [https://www.chepclub.com/startend-gcode.html](https://www.chepclub.com/startend-gcode.html)

### Creality CR10 <a id="Creality-CR10"></a>

* PrusaSlicer/SuperSlicer: [https://3dprintbeginner.com/cr10s-pro-prusa-slicer-profile/](https://3dprintbeginner.com/cr10s-pro-prusa-slicer-profile/)

### Artillery X1/Genius <a id="Artillery-X1/Genius"></a>

* PrusaSlicer/SuperSlicer: [https://3dprintbeginner.com/prusaslicer-profiles/](https://3dprintbeginner.com/prusaslicer-profiles/)

## Links Relacionados: <a id="Links-Relacionados:"></a>

[https://3dprintbeginner.com/flow-rate-calibration/](https://3dprintbeginner.com/flow-rate-calibration/)

