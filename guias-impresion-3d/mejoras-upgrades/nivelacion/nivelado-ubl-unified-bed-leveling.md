---
description: El sistema más avanzado de nivelación de Marlin
---

# Nivelado UBL - Unified Bed Leveling

![](https://telegra.ph/file/ea2e1181d01693225b0cd.jpg)

UBL es el sistema más avanzado de nivelación existente para Marlin. Incorpora mejores herramientas \(gcode/terminal y menús LCD\) para la generación, edición y gestión de las mallas generadas. Además permite un muestreo de puntos en la cama de hasta 100 puntos y un check rápido de 3 puntos de referencia en el momento de iniciar la impresión generada con el slicer.

## **1. Cambios en Marlin \(configuration.h\)**

`#define AUTO_BED_LEVELING_UBL // Habilita UBL como sistema nivelación, en siguientes lineas indica el número de puntos 7 aconsejable`

`#define DEBUG_LEVELING_FEATURE // Permite obtener información detallada en caso de problemas`

`#define ENABLE_LEVELING_FADE_HEIGHT // Indica hasta qué altura aplica las correcciones de cama 10 mm por defecto`

`#define G26_MESH_VALIDATION // Habilita el comando G26 para realizar un test de impresión de la malla`

`#define MESH_INSET 10 // Permite ajustar el margen a partir del cual puede realizar las mediciones en mm`

`#define GRID_MAX_POINTS_X 3 // permite especificar los puntos a usar por eje, si ponemos 3 puntos creará una malla de 3 x 3 puntos (9) de nuestra cama, siempre y cuando no modifiquemos la fórmula en #define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X. El límite de puntos son 15.`

## **2.a Ajuste del Z-Offset a partir de 2.0.7 \(método rápido\)**

Con esto dispondremos de un nuevo menu en Configuración/Avanzado/Probe OffsetsDesde la version 2.0.7.2 Marlin incluye un nuevo asistente para encontrar el valor Z Offset de una forma sencilla. Para activarlo iremos a configuration\_adv y habilitaremos:  
  
`#define PROBE_OFFSET_WIZARD`

## **2.b Ajuste del Z-Offset**

Desde el terminal pero se puede realizar de otras formas

 1\) G28 para realizar un home

 2\) M851 Z0 ajusta el offset de Z a 0

 3\) M211 S0 desactiva los enstop de seguridad, ojo!!! al deshabilitar esto los motores pueden chocar con chasis o cama... ir con cuidado

 4\) G1 X150 Y150 Z0 F7000 para ubicar el cabezal de impresión en el centro de la cama \(para una cama de 300x300\)

 5\) Desde el Pronterface o LCD ir bajando Z y ajustar con un folio de la forma tradicional

 6\) Una vez encontremos la altura adecuada revisar el valor Z en el LCD y ajustarlo como Z-Offset mediante el comando M851 Z "X" \(X es el valor mostrado en la pantalla\)

 7\) Hacer un homing de todos los ejes desde el LCD o G28

 8\) Realizar el paso 4 de nuevo y verificar que el cabezal queda correctamente ubicado y la posición de Z es la correcta obtenida anteriormente si no volver a realizar el proceso

 9\) M500 para guardar los valores en EEPROM y se aconseja guardar este valor en los Offset del Probe en Marlin para futuro![](https://lh4.googleusercontent.com/Mce2bNax3D9-ZzPxJ1eEDM23ytPynKdZe9kqLu-QJ_yG8f9xu1l_9BsNNJe7U-M2vnE184ULljGgfe_BjbdeSOjRNLDePRR9JZaD678npKjCqQYJJpIv5hqFX74mdBpd2OR5pQOk)

## **3. Generación de malla UBL**

se puede realizar desde el LCD en las herramientas UBL como desde terminal \(emplearemos el terminal para el ejemplo\)

 1\) Ajuste manual de la cama 4 esquinas y folio tradicional

 2\) Realizar G28

 4\) Cama/Nozzle precalentado

 5\) Realizar un Cold Mesh desde las herramientas UBL del LCD o G29 P1 desde terminal

 6\) Puedes realizar un G29 T para ver los valores de la malla, desde estas web puedes ver el desfase graficamente

* [https://mkdev.co.uk/mesh-visualizer/](https://mkdev.co.uk/mesh-visualizer/)
* [http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/](http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/)
* [https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/](https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/)

 7\) Seguramente por la ubicación del propio PROBE le será por seguridad imposible llegar a todos los puntos de la malla con un G29 P3 calcula esos puntos por extrapolación

 8\) G29 S1 almacena la malla en la memoria 1 de mallas

 9\) G29 A activa el autonivelado

 10\) M500 para guardar los ajustes en EEPROM

Descripcion del proceso en Marlin:

M502 ; Reset settings to configuration defaults...

M500 ; ...and Save to EEPROM. Use this on a new install.

M501 ; Read back in the saved EEPROM.

M190 S65 ; Not required, but having the printer at temperature helps accuracy

M104 S210 ; Not required, but having the printer at temperature helps accuracy

G28 ; Home XYZ.

G29 P1 ; Do automated probing of the bed.

G29 P2 B T ; Manual probing of locations. USUALLY NOT NEEDED!

G29 P3 T ; Repeat until all mesh points are filled in.

G29 T ; View the Z compensation values.

G29 S1 ; Save UBL mesh points to EEPROM.

G29 F 10.0 ; Set Fade Height for correction at 10.0 mm.

G29 A ; Activate the UBL System.

M500 ; Save current setup. WARNING - UBL will be active at power up, before any G28.

## **4. Modificaciones en gcode inicio\(start\) del slicer**

Debes añadir los siguientes gcodes en le script de inicio de tu slicer justo después del G28 \(homing\)

 1\) G29 L1 ; Carga la malla guardada en la memoria 1

 2\) G29 J ; realiza un test rápido de 3 puntos para validar cualquier desviación

5\) Realizar un test de impresión a ser posible de un modelo de calibración de cama como [https://teachingtechyt.github.io/calibration.html\#firstlayer](https://teachingtechyt.github.io/calibration.html#firstlayer) donde puedes crearlo personalizado para tu impresora

## **Troubleshooting**

M111 S38 habilita el modo debug

\#define DEBUG\_LEVELING\_FEATURE si no es posible habilitarlo por el comando anterior

