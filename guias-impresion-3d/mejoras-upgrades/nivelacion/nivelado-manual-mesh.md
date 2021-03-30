---
description: Mejora el nivelado de tu cama sin necesidad de sensor de nivelación
---

# Nivelado manual MESH

![](https://telegra.ph/file/9faf4b29f0850641ff9ea.jpg)

El nivelado MESH permite una control más avanzado y sencillo de nuestra cama, permite crear una malla de puntos de control de la cama de forma manual y cómodamente desde la pantalla.

Puedes ver en el siguiente video cómo funciona:

{% embed url="https://youtu.be/YRE6DkD1c\_U" %}

## Como activar nivelado MESH

### **1. Cambios en Marlin \(configuration.h\)**

* \#define MESH\_BED\_LEVELING // Habilita el nivelado manual MESH
* \#define RESTORE\_LEVELING\_AFTER\_G28 // Habilita el nivelado después de un homing
* \#define LCD\_BED\_LEVELING // Habilita los menús LCD para crear la malla manual
* \#define INDIVIDUAL\_AXIS\_HOMING\_MENU // Habilita el homing por eje

### **2. Ajuste de la malla manual MESH**

####  **Ajuste de la malla utilizando un terminal \(Pronterface/Octoprint/Repetier\)**

![](https://lh6.googleusercontent.com/Rr8RqrnxGIQ29EIUatM6Phb7M307Ns3kzZhEeA_FLzh9crPlDfJJUSw938IK-nH45PrkYWzashHZbqh8VKeRgCsiKvUl6SZf-b-npBlTOH4ld4P5Fk9TqpVqeG5J7tR8P-2zWE9-)

1. Idealmente realizar primero un nivelado de 4 esquinas utilizando los tornillos de nivelación de la cama y un folio. Esto se hace para minimizar, en la medida de lo posible, que se tengan que compensar grandes desviaciones por una cama mal nivelada.
2. Desde el terminal lanzaremos G29 S1 y pulsamos enter. Esto comenzará el proceso de nivelación.
3. Poner un folio debajo del nozzle.
4. Utilizar los controles de movimientos de la aplicación de terminal usada \(idealmente en pasos de 0.1 o menos\) para mover el nozzle arriba o abajo hasta que el nozzle comience a rozar el folio. Debes poder mover el folio debajo del nozzle pero notando un poco de fricción entre el nozzle y el folio.
5. Una vez tengamos el punto correcto lanzaremos un G29 S2 en la línea de comandos y pulsamos enter. Esto almacenará la altura de Z para ese punto y el nozzle se moverá al siguiente punto \(por defecto son 9\).
6. Repetir los pasos 4 y 5 hasta que completamos el proceso asegurándose que lanzamos un G29 S2 al finalizar.
7. Lanzaremos el comando M500 para guardar los valores en la EEPROM. IMPORTANTE!!! SI VOLVEMOS A RESTAURAR LA EEPROM DEBEREMOS REALIZAR ESTE PROCESO DE NUEVO.

####  **Ajuste de la malla utilizando la pantalla**

1. Ajuste manual de la cama 4 esquinas y folio tradicional, desde el LCD tendremos un asistente para realizarlo en las 4 esquinas
2. Desde el LCD iremos a Nivelado Cama/Bed Leveling y Nivelar Cama/Level Bed, el cabezal se colocara en el primer punto a ajustar mediante el folio y desde el control de la pantalla podemos ir ajustando la altura de Z \(izquierda baja y derecha sube el cabezal\)

### **3. Activación nivelado MESH en tus impresiones y primeras pruebas**

Una vez realizado el proceso de creación de malla MESH, y guardado en la EEPROM, tenemos que indicar a nuestro slicer/fileteador que use esta funcionalidad. Para realizar esto es necesario añadir M420 S1 en tu slicer/fileteador en la configuración de gcode de inicio. Tiene que estar justamente seguido del comando G28 que hace el homing ya que por defecto el G28 deshabilita el nivelado de cama.

Puedes desde el terminal realizar un G29 S0 para visualizar los valores numéricos de desfase de tu malla y utilizar este visualizador online [https://mkdev.co.uk/mesh-visualizer/](https://mkdev.co.uk/mesh-visualizer/) para ver estos desfases de una forma más visual.

Es aconsejable realizar un test de nivelación de cama, este por ejemplo [https://www.thingiverse.com/thing:34558](https://www.thingiverse.com/thing:34558), para comprobar que todo está correcto, podemos utilizar BABYSTEPPING para realizar correcciones en caso de ser necesario y anotar esas correcciones editando desde el LCD los puntos de nivelación.

