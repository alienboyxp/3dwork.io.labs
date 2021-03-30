---
description: Añadiendo una pantalla a nuestro Octoprint
---

# OctoDash - UI

![](https://lh5.googleusercontent.com/1Vs7rR5dESwn2fc5X1KEzqb7F2EdzKUXIprXvYQS6w6mQVSwwpKD2YXDN8tlJaKphKuMSDgF3NV61dtfIzSKl2zPJHfY767CrzdkypY52aDp4M6Nfkst0nRCfhK4gpPaKQogHqvt)

OctoDash es un simple pero bonito dashboard para OctoPrint que cuenta con pantallas.

Utiliza llamadas API para lanzar los comandos a tu Octoprint desde un bonito interfaz que funciona fantástico con una pantalla touch.

## El proceso de instalación:

Preparar Octoprint instalando los siguientes plugins:  
\* **DisplayLayerProgress** \(requerido\) : muestra en la pantalla de la impresora el tiempo estimado para finalizar, el porcentaje y las capas.  
\* **PrintTimeGenius Plugin** \(sugerido\) : estimaciones exactas de tiempo de impresión.  
\* **Filament Manager** \(opcional\): podemos añadir nuestras bobinas y que nos lleve la cuenta del material que gastamos. Está relacionado con el Cost Estimation, puesto que pilla de ahí lo que cuesta cada uno de los materiales.  
\* **Preheat Button** \(opcional\), para hacer el precalentado  
\* **Ultimaker Format Package / Cura o PrusaSlicer Thumbnails** \(sugerido\), tener una imagen de previsualización de la pieza, requiere de configuración en el Slicer y es muy aconsejable  
\* **Enclosure** \(sugerido si tenemos reles\), nos permite controlar los relés configurados en este plugin

Acceder por SSH \(puedes usar [PuTTY](https://www.putty.org/) o [Terminus](https://termius.com/) para conectar ambos son multiplataforma o sea Windows MAC Linux\) el usuario por defecto es pi y el password raspberry.

Preparando el sistema Raspbian para hscer autologin:  
sudo raspi-config  
"3 Boot Options"  
"B1 Desktop / CLI"  
"B2 Console Autologin"  
Finslizar y reiniciar la Pi.

Lanza el siguiente comando:  
bash &lt;\(wget -qO- [https://github.com/UnchartedBull/OctoDash/raw/master/scripts/install.sh](https://github.com/UnchartedBull/OctoDash/raw/master/scripts/install.sh)\)  
Tardará un rato ya que actualizará bastantes partes del sistema y añadirá funciones al mismo además de instalar OctoDash.

> Aconsejable poner un teclado en el caso que la versión de Octoprint o OctoDash no permitan el auto-enlazado \(ver siguiente punto\).

Al arrancar OctoDash intentará detectará instancias de Octoprint en la red selecciona la que quieres controlar \(normalmente la misma donde instalaste OctoDash...octoprint.local\) y deberás poner la API KEY que la puedes encontrar en los ajustes de Octoprint opción API o depende de la versión te saldrá un botón verde en OctoDash que lanza una petición a Octoprint para verificar, en ese caso con ir a Octoprint le das a Allow en la ventana que sale y a disfrutar!!!

![](https://lh4.googleusercontent.com/ugNgFLwbFaXCvhC0XG6lpiJLcjXMKarRDXRQie6Wtpnh2ixjYPF81oQjy01SbZnZnzA4Y9LdJ0ruoujRTwmvdjxNOQwdQHc6jyzDm2TCwqJeXJdgidn97a9ZT2ChmFaYqjt-EmNs)

> Si no tenemos teclado o problemas con el proceso de permisos del botón verde y la parte de permitir en Octoprint podemos editar el fichero de configuración desde SSH  
> sudo nano ~/.config/octodash/config.json  
> el formato es bastante sencillo de leer aunque de la parte superior tan solo necesitamos poner la IP de Octoprint y la API KEY.

> Si deseas actualizar OctoDash desde SSH por si falla desde el interfaz podéis ejecutar el siguiente comando desde el terminal SSH:  
> sudo wget -qO- https://github.com/UnchartedBull/OctoDash/raw/master/scripts/update.sh \| bash

> Si deseas desinstalar OctoDash podéis ejecutar el siguiente comando desde el terminal SSH:  
> wget -qO- https://github.com/UnchartedBull/OctoDash/raw/master/scripts/remove.sh

## Sugerimos las siguientes pantalla para disfrutar de OctoDash:

### Waveshare 7" HDMI Touch

https://www.amazon.es/dp/B07PKLGMSY/ref=cm\_sw\_r\_cp\_apa\_fabc\_jm77FbPWNZKBZ?\_encoding=UTF8&psc=1  
Cambios a realizar para su funcionamiento \(añadir al final del fichero\):  
suso nano /boot/config.txt  
max\_usb\_current=1

hdmi\_force\_hotplug=1  
config\_hdmi\_boost=7  
hdmi\_group=2  
hdmi\_mode=87  
hdmi\_drive=1  
display\_rotate=0 \# 0 conectores a la derecha, 1 a 90 grados, 2 connectores a la izquierda, 3 270 grados  
hdmi\_cvt 1024 600 60 6 0 0 0

### **FYSECT CTP40 \(GPIO\)**

Para esta pantalla asegurarse de instslar los drivers siguiendo las instrucciones https://github.com/FYSETC/FYSETC-CTP40/tree/main/Pi3

## Artículos de Referncia

{% embed url="https://3dprintbeginner.com/install-octodash-on-raspberry-pi/" %}

{% embed url="https://github.com/UnchartedBull/OctoDash/blob/master/README.md" %}



