# Marlin - Guía Compilación

![](https://telegra.ph/file/0c5672a9f68d4955fbb13.jpg)

Marlin es el software que gobierna tu Impresora, a continuación te enseñamos como prepararlo para poder instalar o actualizar en tu Impresora.  
Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

## Consigue una copia de Marlin \(última versión estable\)

* Desde tu navegador favorito ves al GitHub oficial de Marlin: [https://github.com/MarlinFirmware/Marlin](https://github.com/MarlinFirmware/Marlin)
* Una vez en la página descarga el código de Marlin de la siguiente forma:

![](https://telegra.ph/file/c39cb7e17a03226855085.png)

> IMPORTANTE!!! Recuerda bajar la version estable del firmware y solo la bugfix en caso que contenga alguna mejora wue solvente un problema que te suceda en tu máquina

![](https://telegra.ph/file/295556b86680b0b6a3eec.jpg)

* El siguiente paso, si tenemos una impresora comercial, descargaremos los pre-configurados de configuración desde [https://github.com/MarlinFirmware/Configurations](https://github.com/MarlinFirmware/Configurations) y siguiendo el mismo proceso que el explicado en el punto anterior

> IMPORTANTE!!! Recuerda bajar la version de preconfiguraciones wue concuerde con la versión de Marlin descargada con release en su nombre

![](https://telegra.ph/file/c7802d17b18ab8c8255bb.jpg)

Ahora y dispone de los archivos de Marlin en tu ordenador deberemos de descomprimirlos.

> **Importante!!!**  
> **Idealmente no dejar la carpeta con tu Marlin dentro de muchas carpetas anidadas ni con símbolos o acentos en los nombres de las mismas ya que pueden provocar errores en la compilación.**

## Instalación de software necesario para la edición y compilación de Marlin

El siguiente paso es instalar el software necesario, para ello descargue e instale los siguientes programas:

* Python: [https://www.python.org/downloads/](https://www.python.org/downloads/) -&gt; _**tambien puedes descargarlo desde la Store de Apps de Windows ya que puede dar problemas en ocasiones si no se hace de una de estas dos formas**_
* Git: [https://git-scm.com/](https://git-scm.com/)
* Visual Studio Code: [https://code.visualstudio.com/](https://code.visualstudio.com/)A pesar de que parezca que Python y Git no se usan en ningún paso, son requerimientos de Visual Studio Code para este tutorial
* Una vez instalados ambos programas, vedemos instalar el plugin PlatformIO en Visual Studio Code, para ello seguiremos los siguientes pasos:
* Presionamos los cuadrados del menú izquierdo
* en el buscador escribimos «PlatformIO»
* presionamos sobre le rectángulo verde de instalar

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-18-1024x769.png)

## Importación y compilación de Marlin 2.0.x

* Abrimos el explorador de archivos de Windows y accedemos a la carpeta donde hemos descargado y descomprimido el repositorio de Marlin .
* Una vez ahí, abriremos otro explorador de ficheros con el contenido del repositorio de pre-configuraciones
* Encontraremos dos ficheros de pre-configuración de Marlin para nuestra impresora, en la imagen de ejemplo usaremos una Ender 3 con las pre-configuraciones para una placa SKR MINI v2 \(la carpeta a buscar sería lo indicado en el cuadro azul de la siguiente captura\):

![](https://telegra.ph/file/cd0495eb5684364f721f5.png)

* Copiamos los archivos del interior de la carpeta de nuestro pre-configurado \(cuadro rojo de la captura anterior y los pegaremos \(substituyendo los existentes\) en la carpeta «...\Marlin» donde dejamos descomprimido el repositorio de Marlin
* Una vez realizado este paso, abrimos Visual Studio Code, presionamos sobre el icono con dos documentos del menú izquierdo, y a continuación sobre le botón azul de «Agregar carpeta»

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-20.png)

* Buscamos la ubicación donde tenemos nuestro repositorio Marlin y presionamos Agregar:

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-22.png)

* En Visual Studio Code, Seleccionamos el archivo plaformio.ini del área de trabajo de Marlin que acabamos de añadir.
* Editamos la fila de «default\_envs» y sustituimos » megaatmega2560 » por el adecuado para nuestra placa/impresora, haciendo scroll en este documento encontraras diversas placas, encuentra la tuya y substituye «megaatmega2560» por tu placa en caso de que sea necesario

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-23-1024x770.png)

* A continuación si queremos hacer cambios en la configuración básica de nuestra impresora, ya sea por que le hemos hecho una modificación o para mejorar algún ajuste, desplegamos la carpeta Marlin y nos aparecerán los dos archivos de configuración. Podeis encontrar información de opciones básicas de Marlin en el siguiente link de su documentación https://marlinfw.org/docs/configuration/configuration.html

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-24.png)

* Una vez realizados los ajustes deseados, presionamos sobre el botón de compilar, este se encuentra en la parte inferior izquierda y tiene forma de «check»

![](https://wiki.eurek.org/wp-content/uploads/2019/12/image-25.png)

* Cuando ya esté compilado el código, vamos a la carpeta: «...\Marlin\.pio\build\xxxx» donde encostraremos un fichero llamado «firmware.bin».
* Copiamos este fichero en una tarjeta SD vacía en el directorio raíz
* Extraemos la SD del ordenador
* Apagamos la impresora \(Desconectamos el USB y la alimentación si están ambos conectados\)
* Esperamos unos 30 segundos
* Colocamos la tarjeta SD con el nuevo Firmware
* Encendemos la Impresora, tras unos segundos, veremos como esta instalando el nuevo firmware, poco después, y se iniciara con nuestro nuevo Marlin instalado.

## Consejos:

* Si vas a actualizar una impresora y no tienes los ficheros de Marlin originales y has hecho cambios en la configuración de la pantalla te aconsejamos hacer un M503 desde un cliente de terminal como Pronterface y guardarte la información que devuelve ya que son los valores de la EEPROM muy útiles a ajustar en tu nuevo firmware
* Recuerda que es aconsejable después de subir el firmware cargar los valores de fábrica de la EEPROM, esto lo puedes hacer por terminal con un M502 y seguido un M500 o desde la pantalla en modo Marlin en Configuración/Restaurar y seguido Configuración/Salvar EEPROM
* Las placas actuales se genera un firmware.bin que se pone en la SD, en estos casos si el proceso de actualización se hizo de forma correcta este fichero queda renombrado a .CUR. Así que para comprobar si el proceso fué de forma correcta extrae la SD después de realizar el proceso de actualización y comprueba desde tu ordenador que el .BIN cambió a .CUR
* Otras placas, normalmente antiguas de 8b, se "quema" directamente el nuevo firmware en la placa con lo que es necesario tenerlas conectadas por USB. En algunos casos puede dar problemas al programarla por USB por lo que puedes ir a la ubicación donde dejó el binario compilado... en estos casos es un fichero .hex normalmente... y subirlo desde otras herramientas especificas para tu impresora, los slicers/fileteadores como PrusaSlicer o Cura pueden realizar este paso de forma bastante sencilla también

{% embed url="https://3dwork.io/configurar-marlin-2-0-x-desde-cero/" %}



