---
description: Unified Bed Leveling. El sistema más avanzado de nivelación de Marlin
---

# Nivelado UBL

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

1. **G28** para realizar un home
2. **M851 Z0** ajusta el offset de Z a 0
3. **M211 S0** desactiva los enstop de seguridad, ojo!!! al deshabilitar esto los motores pueden chocar con chasis o cama... ir con cuidado
4. **G1 X150 Y150 Z0 F7000** para ubicar el cabezal de impresión en el centro de la cama, este ejemplo es para una cama de 300x300 para adaptarlo al tamaño de tu cama modifica X150 e Y150 por los valores adecuados a tu impresora
5. Desde el Pronterface o LCD ir bajando Z y ajustar con un folio de la forma tradicional
6. Una vez encontremos la altura adecuada revisar el valor Z en el LCD y ajustarlo como Z-Offset mediante el comando **M851 Z "X"** \(X es el valor mostrado en la pantalla\)
7. Hacer un homing de todos los ejes desde el LCD o **G28**
8. Realizar el paso 4 de nuevo y verificar que el cabezal queda correctamente ubicado y la posición de Z es la correcta obtenida anteriormente si no volver a realizar el proceso
9. **M500** para guardar los valores en EEPROM y se aconseja guardar este valor en los Offset del Probe en Marlin para futuro. Tambien se puede hacer desde los menus del LCD en Configuracion/Salvar EEPROM

![](https://lh4.googleusercontent.com/Mce2bNax3D9-ZzPxJ1eEDM23ytPynKdZe9kqLu-QJ_yG8f9xu1l_9BsNNJe7U-M2vnE184ULljGgfe_BjbdeSOjRNLDePRR9JZaD678npKjCqQYJJpIv5hqFX74mdBpd2OR5pQOk)

## **3. Generación de malla UBL**

### Usando un sensor de nivalación

Se puede realizar desde el LCD en las herramientas UBL como desde terminal \(emplearemos el terminal para el ejemplo\)

1. **Ajuste manual de la cama 4 esquinas y folio tradicional**
2. Realizaremos un **G28**
3. Realizaremos un **pre-calentado de cama y nozzle** que normalmente usemos
4. Realizaremos una malla usando **G29 P1** desde el terminal o desde las herramientas UBL de la pantalla
5. Una vez finalizado podemos usar **G29 T** para visualizar los valores de la malla. Puedes usar estas herramientas web para verlo graficamente: [https://mkdev.co.uk/mesh-visualizer/](https://mkdev.co.uk/mesh-visualizer/) [http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/](http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/) [https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/](https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/)
6. Por la ubicación del sensor de nivelación en muchas ocasiones Marlin no va a llegar a todos los puntos de la malla con lo que en el paso anterior veremos coordenadas sin valores solamente con un ".". Para realizar el relleno de todos los puntos que no pudieron medirse lanzaremos un **G29 P3** \(**G29 P3 T** mejor ya que calcula todos los puntos que falten\) este comando calcula extrapolando valores que si que se pudieron verificar a los que no pudieron medirse.
7. Una vez aparezcan todos los puntos de la malla con un valor, fuera medido o extrapolado por el punto anterior, almacenaremos la malla en la posición 1 de la EEPROM con **G29 S1**
8. Nos aseguramos de que el autonivelado quede activo con **G29 A**
9. Ahora que ya tenemos todos los valores básicos correctos lanzaremos un **M500** para guardarlos en la EEPROM. **IMPORTANTE!!! cada vez que la EEPROM sea borrada o reseteada se tiene que volver a realizar este proceso!!**!

#### Descripcion del proceso en Marlin:

```text
;------------------------------------------
;--- Setup and initial probing commands ---
;------------------------------------------
M502            ; Reset settings to configuration defaults...
M500            ; ...and Save to EEPROM. Use this on a new install.
M501            ; Read back in the saved EEPROM.

M190 S65        ; Not required, but having the printer at temperature helps accuracy
M104 S210       ; Not required, but having the printer at temperature helps accuracy

G28             ; Home XYZ.
G29 P1          ; Do automated probing of the bed.
G29 P2 B T      ; Manual probing of locations USUALLY NOT NEEDED!!!!
G29 P3 T        ; Repeat until all mesh points are filled in.

G29 T           ; View the Z compensation values.
G29 S1          ; Save UBL mesh points to EEPROM.
G29 F 10.0      ; Set Fade Height for correction at 10.0 mm.
G29 A           ; Activate the UBL System.
M500            ; Save current setup. WARNING: UBL will be active at power up, before any [`G28`](/docs/gcode/G028.html).
;---------------------------------------------
;--- Fine Tuning of the mesh happens below ---
;---------------------------------------------
G26 C P5.0 F3.0 ; Produce mesh validation pattern with primed nozzle (5mm) and filament diameter 3mm
                ; PLA temperatures are assumed unless you specify, e.g., B 105 H 225 for ABS Plastic
G29 P4 T        ; Move nozzle to 'bad' areas and fine tune the values if needed
                ; Repeat G26 and G29 P4 T  commands as needed.

G29 S1          ; Save UBL mesh values to EEPROM.
M500            ; Resave UBL's state information.
;----------------------------------------------------
;--- Use 3-point probe to transform a stored mesh ---
;----------------------------------------------------
G29 L1          ; Load the mesh stored in slot 1 (from G29 S1)
G29 J           ; No size specified on the J option tells G29 to probe the specified 3 points
                ; and tilt the mesh according to what it finds.
```

### Manualmente sin sensor de nivelación

Como hemos comentado UBL puede ser usado con o sin sensor de nivelación \(**PROBE\_MANUALLY** tiene que estar definido en Marlin\) permitiendo tener un mejor control en las desviaciones en nuestra cama si esta no esta perfectamente recta.

Más importante que en el caso de disponer un sensor de nivelacion es muy importante que tengamos correctamente nivelada nuestra cama de impresion con el metodo de 4 esquinas y el folio para asegurar que los valores obtenidos con la malla solo requiera correcciones.

En este caso en lugar de empezar con un G29 P1 \(sondeo de la cama automático\) lo haremos con G29 P0 para realizar el sondeo de forma manual.

```text
G29 P0      ; Zero the mesh
; Optional
G29 P2...   ; Probe manually at appropriate locations
G29 P3...   ; Fill in missing points
G26         ; Jump into validation print / edit process
G29 P4 R... ; Refine mesh points
; /Optional
G29 S1      ; Save to slot 1, return to G26 for further refinement
M500            ; Save current setup. WARNING: UBL will be active at power up, before any [`G28`](/docs/gcode/G028.html).
```

## **4. Modificaciones en gcode inicio\(start\) del slicer**

Debes añadir los siguientes gcodes en le script de inicio de tu slicer justo después del G28 \(homing\)

* **G29 L1** ; Carga la malla guardada en la memoria 1 usado en los pasos anteriores para almacenar la malla
* **G29 J** ; realiza un test rápido de 3 puntos para validar cualquier desviación

{% hint style="info" %}
**IMPORTANTE!!! en el caso de usar nivelación UBL sin sensor de nivelación el G29 J no se debe de añadir a nuestro gcode de inicio.**
{% endhint %}

Realizar un test de impresión a ser posible de un modelo de calibración de cama como... 

{% embed url="https://teachingtechyt.github.io/calibration.html\#firstlayer" %}

...donde puedes crearlo personalizado para tu impresora. Idealmente durante la impresión de dicho test podemos ir ajustando con babbystepping a nuestro gusto el ZOffset y posteriormente si hicimos cambios aplicarlos al ZOffset y guardar en EEPROM.

## **Troubleshooting**

M111 S38 habilita el modo debug

\#define DEBUG\_LEVELING\_FEATURE si no es posible habilitarlo por el comando anterior

