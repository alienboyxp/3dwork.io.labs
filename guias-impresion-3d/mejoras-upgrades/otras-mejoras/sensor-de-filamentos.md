# Sensor de filamentos

![](https://lh3.googleusercontent.com/rsgUdGYSo92gzCE-gvaKe0uAfB7fogEPZNoI11ZOv9USIngT57U2nIQx0SpkBZOciT3ErnSHfNqbvXAMaszOHTWUCeGHTR2Rlo5nmpLz28tVL49mSqs69YYVxqj8_V5KyYM73zUO)

{% embed url="https://3dwork.io/sensor-de-filamento/" %}

El uso de un sensor de filamentos puede salvarnos más de una impresión, además si aplicaste la configuración SensorLess/stallGuard explicada en este documento probablemente dispongas de varios finales de carrera que puedes usar para este propósito sin gastarte nada, en Thingiverse dispones de varios adaptadores para ello.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

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
> Modelo - Pin por defecto  
> SKR 1.4 / 1.4 Turbo - \#define FIL\_RUNOUT\_PIN P1\_26 \(E0DET\) o P1\_25 \(E1DET\)  
> SKR 1.3 - \#define FIL\_RUNOUT\_PIN P1\_28  
> SKR MINI E3 - \#define FIL\_RUNOUT\_PIN P15

> **En caso de falsos positivos se puede jugar a habilitar o deshabilitar esta función ya que en según que escenarios puede ocasionar ciertos problemas:  
> \#define FILAMENT\_RUNOUT\_DISTANCE\_MM**  
> **También aconsejan en el caso de usar Sensorless/stallGuard doblar/desoldar/cortar el pin DIAG del driver para evitar falsos positivos.**

* Algunos finales de carrera requieren de eliminar el PULLUP de la configuración de Marlin para operar correctamente. Si es tu caso, debes editar esta linea y eliminar los comentarios: \#define FIL\_RUNOUT\_PULLUP o viceversa dependiendo del tipo de placa \#define FIL\_RUNOUT\_PULLDOWN. Recuerda que solamente una de ellas debe estar habilitada.
* Activación y el parking del cabezal para el cambio de filamentos necesario para habilitar el sensor de filamentos y las opciones en pantalla para realizar el pausado y cambio de filamento **`#define ADVANCED_PAUSE_FEATURE - en configuration_adv.h #define NOZZLE_PARK_FEATURE - en configuration.h`**

## **Cómo ajustar el comando M600 para el cambio de filamentos:**

Cuando hacemos un cambio de filamento el comando M600 \(función ADVANCED\_PAUSE\_FEATURE\) es el que se encarga del proceso. En configuration\_adv.h podemos encontrar algunas funciones interesantes para personalizarlo a nuestra máquina, aunque por norma general y salvo contadas excepciones nos funcionará sin problemas como viene de serie.

* FILAMENT CHANGE UNLOAD LENGTH con él controlaremos qué distancia de filamento haremos retroceder. Normalmente usaremos la distancia entre la salida del nozzle y los engranajes de nuestro extrusor más un poco de margen para asegurar que sale completamente del engranaje.
* Es importante que si esta distancia es superior a EXTRUDE\_MAXLENGTH \(configuration.h\) deberemos ajustar este parámetro al que necesitemos para nuestra retracción de cambio de filamento.
* FILAMENT CHANGE SLOW LOAD FEEDRATE con este parámetro indicaremos con qué velocidad realizara la carga inicial de filamentos.
* FILAMENT CHANGE SLOW LOAD LENGTH al igual que para la retracción aquí le indicaremos los mm de filamento a mover en la carga inicial de filamentos, normalmente el valor por defecto es el correcto
* FILAMENT CHANGE FAST LOAD FEEDRATE en este caso indicaremos a la velocidad que realizará la carga del filamento hasta el nozzle
* FILAMENT CHANGE FAST LOAD LENGTH usaremos una medida similar a la de UNLOAD menos SLOW LOAD LENGTH

Estos son los valores más importantes a tener en cuenta.

