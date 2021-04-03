---
description: Como solucionar problemas con pin de termistor dañados
---

# Reparar pin sensor temperatura

Un fallo muy común normalmente por mala manipulación o por un corto en un termistor es que el pin de nuestra placa quede dañado y en este caso podemos intentar ajustar nuestro Marlin para usar un pin alternativo para esa función.

**Es MUY aconsejable que desechemos ese termistor para evitar que ese fallo vuelva a dañar otro pin de nuestra placa!!!**

## Cambios en Marlin

El procedimiento es muy sencillo pero antes de nada deberemos de tener claro 3 puntos importantes:

* saber identificar el fichero pins de nuestra placa y si este tiene incluidos otros relacionados. En el caso de ejemplo que desarrollaremos usaremos uno de los más complejos escenarios usando una SKR 1.4 Turbo.  En todo caso puedes revisar la table de nuestra guia de ventiladores:

{% embed url="https://3dwork.qitec.net/guias-impresion-3d/resolucion-de-problemas/problemas-hardware-impresora-3d/control-de-ventiladores-con-mosfet-externo\#que-deshabilitar-para-poder-trasladar-la-funcion-de-ventilador-a-otro-pin" %}

* Identificar un pin de nuestra placa PWM, en este caso no solemos tener muchos disponibles por lo que deberemos revisar el esquema de nuestra placa para identificar uno libre y que no sea útil/necesario.  Puedes revisar la lista de pines identificados que incluímos en nuestra guía sobre ventiladores:

{% embed url="https://3dwork.qitec.net/guias-impresion-3d/resolucion-de-problemas/problemas-hardware-impresora-3d/control-de-ventiladores-con-mosfet-externo\#que-deshabilitar-para-poder-trasladar-la-funcion-de-ventilador-a-otro-pin" %}

* Realizar los cambios de asignación de pines

### Cambios a realizar en nuestro fichero pins en Marlin

Vamos o proponer el siguiente escenario...  
Nuestra placa, SKR 1.4 Turbo, despues de un mantenimiento realizamos un corto en el thermistor y desgraciadamente rompió el pin de la placa... normalmente se dan dos casos:

* nuestra pantalla muestra -14 en la temperatura, en estos casos estamos de "suerte" ya que normalmente significa que nuestro thermisor esta fallando o no se ha conectado correctamente
* la lectura de temperatura da valores muy altos, en estos casos normalmente indica un fallo de pines de nuestra placa. Podemos cambiar thermistores con el de la cama para verificar que un thermistor que funciona correctamente en otro pin en este no lo hace

Una vez identificado que el problema debemos identificar el fichero pins de nuestra placa en Marlin, en nuestro caso SKR 1.4 Turbo el fichero pins se encuentra en:

`Marlin/src/pins/lpc1769/pins_BTT_SKR_V1_4_TURBO.h`

Los ficheros con la definicion de pines se encuentran en Marlin/src/pins y la siguiente carpeta es el tipo de CPU que usa nuestra placa.

Como podemos ver en el fichero pins de nuestra placa SKR 1.4 TURBO este incluye un include de otro:

```cpp
// Include SKR 1.4 pins
#include "../lpc1768/pins_BTT_SKR_V1_4.h"
```

Donde también vemos que tiene un include:

```cpp
// Include common SKR pins
#include "pins_BTT_SKR_common.h"
```

Así que pasaremos a editar ese fichero de pines e identificaremos la parte donde se definen los thermistores:

![](../../../.gitbook/assets/image%20%2841%29.png)

Verificaremos el ficher pins de nuestra placa, en nuestro caso al solamente usar un extrusor cambiaremos el rol de pines entre el TH0 \(falla\) con el TH1, en el caso de estar en uso y tal como comentamos al inicio de la guia buscaremos otro pin PWM que no este en uso para trasladar el rol

![Esquema de pines de nuestra SKR 1.4, el TH0 con flecha roja lo trasladaremos al TH1 con flecha verde.](../../../.gitbook/assets/image%20%2840%29.png)

Ajustaremos los valores tal como vemos a continuación:

```cpp
// Temperature Sensors
//  3.3V max when defined as an analog input
#ifndef TEMP_0_PIN
  #define TEMP_0_PIN                    P0_23_A0  // 3DWORK - P0_24_A1 Valor Original - A1 (T1) - (68) - TEMP_0_PIN
#endif
#ifndef TEMP_1_PIN
  #define TEMP_1_PIN                    P0_24_A1  // 3DWORK - P0_25_A2 Valor Original - A2 (T2) - (69) - TEMP_1_PIN
#endif
#ifndef TEMP_BED_PIN
  #define TEMP_BED_PIN                  P0_25_A2  // 3DWORK - P0_23_A0 Valor Original - A0 (T0) - (67) - TEMP_BED_PIN
#endif
```

![](../../../.gitbook/assets/image%20%2842%29.png)

Aunque este paso no es necesario pero por "consistencia" volveremos al fichero de pins ../lpc1768/pins\_BTT\_SKR_V14.h y corregiremos la asignación del pin para_ TEMP1 para que coincida con el anterior

```cpp
#define TEMP_1_PIN                      P0_24_A1  // 3DWORK - P0_23_A0 Valor Original - A0 (T0) - (67) - TEMP_1_PIN
#define TEMP_BED_PIN                    P0_25_A2  // A2 (T2) - (69) - TEMP_BED_PIN
```

Ahora tan solo nos queda compilar y seguir fundiendo!!!

