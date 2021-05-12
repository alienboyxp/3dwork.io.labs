---
description: Configuración y características
---

# Pantallas BigtreeTech/SKR

![](https://lh4.googleusercontent.com/-nMT-H5BeHKlVRYXRBJ8tGpynX-Wzs4_7cGvl3gqxw-YPxPMiqebHZB3ldcO627G6pyrQr54zlklMzyZBf-yEH4LVd1nzoB7_O6eDAaXMcEk-LiY0Gbsc8SPxWKJeTHPFYZJTq64)

## **Conexión de la pantalla**

Normalmente las pantallas SKR cuentan con dos modos de funcionamiento, el Touch o táctil y el modo emulación Marlin

Podéis ver en el siguiente video como cambiar de un modo a otro... 

{% embed url="https://youtu.be/Yp4\_PyfMntE" %}

Si necesitas más información o ayuda no dudes a unirte al grupo de Telegram de SKR [@SKR\_board\_32bits](https://t.me/SKR_board_32bits).

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram [@ThreeDWorkHelpBot](https://t.me/ThreeDWorkHelpBot)

{% embed url="https://www.buymeacoffee.com/jjr3d" %}

## **Modo Marlin**

* Para pantallas SKR “normales”,normalmente estas pantallas se conectan usando los conectores EXP1 y EXP2 entre la placa y la pantalla
* Para pantallas SKR E3, normalmente estas pantallas solamente se conectan usando el conector único EXP \(Normalmente en placas E3 o stock\) al EXP3 de la pantalla

## **Modo Touch o táctil**

Estas funcionan por serial y son “universales” ya que se pueden conectar a cualquier placa que cuente con un puerto serial… dependiendo de la marca puede tener que adaptarse los cables para que funcione correctamente si no es SKR.

Este cable es un cable de 5 pines Dupont que se conecta al conector TFT de la placa, revisa el pinout para identificar el pin RST y así averiguar rápidamente la orientación del cable.

![Ejemplo de conexi&#xF3;n SKR 1.4](https://telegra.ph/file/bce3c24f2743531f19802.jpg)

![Esquema conexi&#xF3;n SKR TFT35 V3](https://telegra.ph/file/aa9510b4e90f7d7856099.png)

![Esquema conexi&#xF3;n SKR TFT35 E3 V3](https://telegra.ph/file/aa7a856f23c17cb924f54.png)

![Esquema conexi&#xF3;n SKR TFT24](https://telegra.ph/file/da039bfe8737627a23503.png)

![Resumen de conexiones para todas las pantallas SKR y sus definiciones en Marlin](https://telegra.ph/file/477a3f6afee20f7260a6c.jpg)

Como hemos comentado es muy importante la configuración de SERIAL y BAUDIOS en Marlin \(configuration.h\), os ponemos como ejemplo algunas configuraciones para placas SKR:

**SKR 1.3**  
\#define SERIAL\_PORT 0  
\#define SERIAL\_PORT\_2 -1

**SKR 1.4**  
\#define SERIAL\_PORT -1  
\#define SERIAL\_PORT\_2 0

SKR PRO 1.1  
\#define SERIAL\_PORT -1  
\#define SERIAL\_PORT\_2 1

**SKR MINI E3 V1.2**  
**\#define SERIAL\_PORT 2**  
**\#define SERIAL\_PORT\_2 -1**

SKR E3 DIP  
\#define SERIAL\_PORT 2  
\#define SERIAL\_PORT\_2 -1

En cuanto BAUDRATE podeis elegir entre estos wue son los wue suelen dar mejor resultado:  
\#define BAUDRATE 115200  
O  
\#define BAUDRATE 250000

## **Actualización de Firmware y Configuración**

* Descargar la última versión del firmware desde el Github de Bigtreetech [https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware](https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware)
* Descomprimir el contenido de Github e ir a la carpeta **Copy to SD Card root directory to update**
* Copiar a la raíz de la SD a colocar en la pantalla:
* El .bin del firmware BIGTREE\_TFT\*\_V\*.\*.\*.bin. Ejemplo: BIGTREE\_TFT35\_V3.0.26.1.bin
* Actualmente se disponen de 4 temas diferentes cada una con su estilo de iconos:

![Unified Menu Theme - Carpeta Copy to SD Card root directory to update - Unified Menu Material theme](https://telegra.ph/file/b4c088eea77f051f80eb7.png)

![The Round Miracle Theme by Acenotass - Carpeta Copy to SD Card root directory to update - The Round Miracle theme](https://telegra.ph/file/9c91d1c65d3f5c15de937.png)

![Hybrid Red Material Theme by AntoszHUN - Carpeta Copy to SD Card root directory to update - Hybrid Red Menu Material theme](https://telegra.ph/file/909271e671522343ad145.png)

![Hybrid Mono Material Theme by bepstein111 - Carpeta Copy to SD Card root directory to update - Hybrid Mono Menu Material theme](https://telegra.ph/file/debcfc3d5b2faabee46c6.png)

* Elegiremos de la que mas nos guste la carpeta TFT\* que coincida con el tamaño de la pantalla, dentro deberá tener la carpeta font \(fuentes\) y bmp \(iconos\)
* El archivo de configuración avanzada config.ini. Para más detalle de las opciones o como modificarlo revisa este link https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware/blob/master/config\_instructions.md

![](https://lh3.googleusercontent.com/w7Y_lamMVyNThBButhFZ8aKhdgMkllKInmGM_u-QLD65KIxwsyAqWOUXA6WPgZJPOXuhXEihGyJjDZt_RDMCqizzUwXictRsZL51SttaW7BJDikvPxfzzzgoz9OL8c_Lckjnx8Uq)

* Insertar la SD y encender la impresora, deberá comenzar el proceso de actualización

![](https://lh5.googleusercontent.com/Lihd76o4XEJN68RgHVaDdf7tU3PhMo40HTMfNelO2NHgX-q2bgSQmfiCis-EBBSydZT2AExfblRCm3Gq8VwMqBvFP_jFIifx0_sCqByTt4oKIsWR1JozoKFmzgUz0-iKU3t2hC4M)

* Una vez finalizado la SD debería de quedar con un backup de seguridad

![](https://lh5.googleusercontent.com/SG5Z9GiRQzmNN83bQTAxYutYB9ATvMFWU8IHfkX6IJ-0qvM7Wpr6Qc9PpdAY8xys_W1AmRgsk67dI0eO2IcDjlmsm38OlDfxDJ8028HI8sIyT8caj-NvvzfQ5_KeeapXV5PwwRol)

Si queremos cambiar del idioma ñor defecto deberemos añadirlo a la SD y acruslizar la pantalla tal como vemos en la siguiente captura:

![](https://telegra.ph/file/0b00b8c15602ff47f4fe6.jpg)

> **Errores mas comunes al actualizar el firmware de la pantalla:**  
> - la pantalla deja de responder el modo táctil, una posible solución es dejar un fichero reset.txt \(sin contenido\) en la raíz de la SD y encender/reset de la pantalla para que restaure su EEPROM  
> - muestra errores de "Invalid App" o proglema en config.ini, puede solucionarse dejando el config.ini de la nueva version y un fichero reset.txt \(sin contenido\) en la raíz de la SD, endender/reset de la pantalla para que aplique la nueva estructura de EEPROM  
> - iconos o textos aparezen corruptos, volver a copiar la carpeta TFTXX que coincida con nuestra pantalla debería de solucionar el problema

## **Cambios en Marlin**

Para habilitar el modo Marlin asegúrate que tienes:

* Para pantallas SKR “normales”, habilitar solamente **`REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER`**
* Normalmente estas pantallas se conectan usando los conectores EXP1 y EXP2 entre la placa y la pantalla
* Para pantallas SKR E3, habilitar únicamente **`CR10_STOCKDISPLAY`**.
* Normalmente estas pantallas solamente se conectan usando el conector único EXP \(Normalmente en placas E3 o stock\) al EXP3 de la pantalla

Para un correcto funcionamiento de las funciones en el modo Touch es aconsejable activar las siguientes funcionalidades en Marlin:

Opciones Generales:  
`# EEPROM_SETTINGS (Configuration.h)  
# BABYSTEPPING (Configuration_adv.h)  
# AUTO_REPORT_TEMPERATURES (Configuration_adv.h)  
# M115_GEOMETRY_REPORT (Configuration_adv.h)  
# M114_DETAIL (Configuration_adv.h)  
# REPORT_FAN_CHANGE (Configuration_adv.h)  
# LCD_SET_PROGRESS_MANUALLY (Configuration_adv.h)`

Opciones para soportar impresiones desde la SD de la placa:  
`# SDSUPPORT (Configuration.h)  
# LONG_FILENAME_HOST_SUPPORT (Configuration_adv.h)  
# AUTO_REPORT_SD_STATUS (Configuration_adv.h)  
# SDCARD_CONNECTION ONBOARD (Configuration_adv.h)`

Opciones para soportar HOST ACTIONS:  
`# EMERGENCY_PARSER (Configuration_adv.h)  
# SERIAL_FLOAT_PRECISION 4 (Configuration_adv.h)  
# HOST_ACTION_COMMANDS (Configuration_adv.h)  
# HOST_PROMPT_SUPPORT (Configuration_adv.h)`

Opciones para soportar menú Carga/Descarga filamentos:  
`# NOZZLE_PARK_FEATURE (Configuration.h)  
# ADVANCED_PAUSE_FEATURE (Configuration_adv.h)  
# PARK_HEAD_ON_PAUSE (Configuration_adv.h)  
# FILAMENT_LOAD_UNLOAD_GCODES (Configuration_adv.h)`

Opciones para soportar Babbystepping:  
`# BABYSTEPPING (Configuration_adv.h)`

Opciones para soportar el test de precisión de sensor de nivelación \(M48\):  
`# Z_MIN_PROBE_REPEATABILITY_TEST (Configuration.h)`

Opciones para soportar la alineación de Z en configuración de multiples drivers por eje Z \(G34\):  
`# Z_STEPPER_AUTO_ALIGN (Configuration_adv.h)`  


También revisa el config.ini para que coincida con las características de tu impresora. Además podrás ajustar los valores por defecto, normalmente y dependiendo del firmware de la pantalla, de la pantalla así como opciones avanzadas como comandos personalizados que puedes encontrar en el siguiente link:

https://raw.githubusercontent.com/bigtreetech/BIGTREETECH-TouchScreenFirmware/master/Copy%20to%20SD%20Card%20root%20directory%20to%20update/config.ini

## **Como poder ver una previsualización de los archivos desde la pantalla en modo Touch**

Os explicamos en unos sencillos pasos como poder previsualizar los gcodes que tenéis en vuestras SD desde las pantallas SKR.

Esto es muy útil cuando acumulas muchos y no recuerdas ni si quiera que tenías en cada :P.

![](../../../.gitbook/assets/image%20%2848%29.png)

### **Cura**

1. Actualiza el firmware de tu pantalla con la última versión disponible, la compatibilidad con este se añadió a partir de Vx.x.27.
2. Asegúrate de habilitar la previsualización **desactivando el modo List Mode**, puedes hacerlo desde **Menu -&gt; Settings -&gt; Feature -&gt; Files viewer - List Mode**
3. Descarga el siguiente [zip](https://github.com/bigtreetech/Bigtree3DPluginSuit/archive/master.zip) y lo descomprimes
4. Encuentra el directorio de plugins de tu Ultimaker Cura:
   1. **Windows**, el directorio por defecto es C:\Program Files\Ultimaker Cura \[version number\]\plugins
   2. **macOS**, botón derecho sobre Ultimaker Cura.app y Show Package Contents. El directorio por defecto es Ultimaker Cura.app -&gt; Contents -&gt; Resources -&gt; Plugins -&gt; Plugins
   3. **Linux**, el directorio por defecto es ~/.local/share/cura/\[version number\]/plugins
5. Copia del zip descomprimido las carpetas **Bigtree3DPlugin**, **ResolutionExtension**, **BigtreeRemovableDriveOutputDevice** en el directorio plugins que encontraste en el punto anterior
6. Abrimos Cura y hacemos un fileteado de una pieza
7. Veremos que el botón de guardar tiene un desplegable que nos permitirá guardar en el nuevo formato Bigtree3D

![](../../../.gitbook/assets/image%20%2843%29%20%281%29.png)

### **PrusaSlicer/SuperSlicer**:

- **Windows**, Primero descargaremos la última versión del conversor desde [https://github.com/effgarces/Biqu-Thumbnail-Generator/releases/](https://github.com/effgarces/Biqu-Thumbnail-Generator/releases/) \(recuerda al final de cada release hay un apartado Assets y desde ahí puedes descargar el .zip necesario biqu\_convert\_new.zip descomprimimos y el ejecutable que contiene lo colocaremos en la carpeta donde se encuentra el laminador PrusaSlicer

![Captura de nuestro amigo https://t.me/OFF\_TOPIC\_Nules](https://telegra.ph/file/3fe1ea1c978a7c6d15ca3.png)

Abriremos PrusaSlicer/SuperSlicer y nos vamos al **perfil de nuestra impresora en General** configurando el **valor 200x200 en Miniaturas de código G** tal como puedes ver en la siguiente captura.![](https://telegra.ph/file/0b4d79f71153b5ec8125d.png)

En la pestaña **Opciones de Salida de nuestro perfil de impresión** añadiremos "**biqu\_convert\_new.exe;**" \(sin las comillas\) en **Scripts de postprocesamiento**. Puedes verlo en la siguiente captura.![](https://telegra.ph/file/3c670080fe84c27701134.png)

Otros Sistemas Operativos como Linux/MAC, nos aseguraremos de tener instalado Python 3 además de PyQt5 \(pip install PyQt5\).

Descargamos el script en Python [https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware/files/5758960/biqu\_convert\_mod.zip](https://github.com/bigtreetech/BIGTREETECH-TouchScreenFirmware/files/5758960/biqu_convert_mod.zip)

Abriremos PrusaSlicer/SuperSlicer y nos vamos al **perfil de nuestra impresora en General** configurando el **valor 200x200 en Miniaturas de código G** tal como puedes ver en la captura para la versión Windows.

En la pestaña **Opciones de Salida de nuestro perfil de impresión** añadiremos la linea de codigo para lanzar Python y el biqu\_convert\_new.py \(sin las comillas\) en **Scripts de postprocesamiento**.

> Para asegurarnos que se usan las previsualizaciones desde nuestra pantalla es aconsejable entrar en el **menú Configuración/Añadidos** de la TFT y **desactivar la opción Files viewer List Mode**

## Como adaptar una pantalla CR10/LCD12864/TFT24 a placas SKR MINIE3 que solamente cuentan con EXP3

Tal como indicamos al inicio de la guía las pantallas E3 cuentan con el conector EXP3 para poder conectar sin problemas en placas Creality o SKR MINI o similares.

En cualquier caso podemos, con un splitter, transformar esa conexión EXP3 de la placa en EXP1 y 2.

Podemos ir por la via sencilla que es solicitar en Aliexpress ese cable [https://es.aliexpress.com/i/1005001325478608.html](https://es.aliexpress.com/i/1005001325478608.html).

Por otro lado podemos crearnos, como buenos makers, nosotros mismos ese cable siguiendo el siguiente esquema:

![](https://telegra.ph/file/109a451f6106c47d480e7.png)

## Links de Compra

{% tabs %}
{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_9fMWsT" %}

{% embed url="https://s.click.aliexpress.com/e/\_AVRWDl" %}

{% embed url="https://s.click.aliexpress.com/e/\_APXHkJ" %}
{% endtab %}

{% tab title="Amazon" %}
{% embed url="https://amzn.to/3eGJ5iu" %}

{% embed url="https://amzn.to/3aRq0Jc" %}

{% embed url="https://amzn.to/3u77T9O" %}
{% endtab %}
{% endtabs %}



