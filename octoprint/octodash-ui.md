---
description: Añadiendo una pantalla y un interfaz bonito para manejar nuestro Octoprint
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

Preparando el sistema Raspbian para hacer autologin \(dependiendo de la versión pueden variar los menus\):  
`sudo raspi-config  
"3 Boot Options"  
"B1 Desktop / CLI"  
"B2 Console Autologin"`  
Finalizar y reiniciar la Pi.

Lanza el siguiente comando:

```bash
bash <(wget -qO- https://github.com/UnchartedBull/OctoDash/raw/main/scripts/install.sh)
```

> Aconsejable poner un teclado en el caso que la versión de Octoprint o OctoDash no permitan el auto-enlazado \(ver siguiente punto\).

Al arrancar OctoDash intentará detectará instancias de Octoprint en la red selecciona la que quieres controlar \(normalmente la misma donde instalaste OctoDash...octoprint.local\) y deberás poner la API KEY que la puedes encontrar en los ajustes de Octoprint opción API o depende de la versión te saldrá un botón verde en OctoDash que lanza una petición a Octoprint para verificar, en ese caso con ir a Octoprint le das a Allow en la ventana que sale y a disfrutar!!!

![](https://lh4.googleusercontent.com/ugNgFLwbFaXCvhC0XG6lpiJLcjXMKarRDXRQie6Wtpnh2ixjYPF81oQjy01SbZnZnzA4Y9LdJ0ruoujRTwmvdjxNOQwdQHc6jyzDm2TCwqJeXJdgidn97a9ZT2ChmFaYqjt-EmNs)

> Si no tenemos teclado o problemas con el proceso de permisos del botón verde y la parte de permitir en Octoprint podemos editar el fichero de configuración desde SSH  
> **`sudo nano ~/.config/octodash/config.json`**  
> el formato es bastante sencillo de leer aunque de la parte superior tan solo necesitamos poner la IP de Octoprint y la API KEY.

> Si deseas actualizar OctoDash desde SSH por si falla desde el interfaz podéis ejecutar el siguiente comando desde el terminal SSH:  
> **`sudo wget -qO- https://github.com/UnchartedBull/OctoDash/raw/main/scripts/update.sh | bash`**

> Si deseas desinstalar OctoDash podéis ejecutar el siguiente comando desde el terminal SSH:  
> **`wget -qO- https://github.com/UnchartedBull/OctoDash/raw/main/scripts/remove.sh | bash`**

## Resolución de errores

### Fallo al iniciar entorno gráfico por fallo en fbturbo

En el caso que tengamos un error al iniciar OctoDash que indique un fallo en la libreria fbturbo 

![](../.gitbook/assets/image%20%28169%29.png)

Podemos seguir los siguientes pasos para solucionarlo:

```text
sudo apt-get install libgtk-3-0 xserver-xorg xinit x11-xserver-utils
sudo apt-get install git build-essential xorg-dev xutils-dev x11proto-dri2-dev
sudo apt-get install libltdl-dev libtool automake libdrm-dev
git clone https://github.com/ssvb/xf86-video-fbturbo.git

cd xf86-video-fbturbo
autoreconf -vi
./configure --prefix=/usr
make
sudo make install
sudo cp xorg.conf /etc/X11/xorg.conf
```

## Sugerimos las siguientes pantalla para disfrutar de OctoDash:

### Waveshare 7" HDMI Touch \(HDMI

{% embed url="https://www.amazon.es/dp/B07PKLGMSY/ref=cm\_sw\_r\_cp\_apa\_fabc\_jm77FbPWNZKBZ?\_encoding=UTF8&psc=1" %}

Cambios a realizar para su funcionamiento \(añadir al final del fichero\):

```text
sudo nano /boot/config.txt
```

Y modificaremos/añadiremos lo siguiente

```bash
max_usb_current=1

hdmi_force_hotplug=1
config_hdmi_boost=7
hdmi_group=2
hdmi_mode=87
hdmi_drive=1
display_rotate=0 # 0 conectores a la derecha, 1 a 90 grados, 2 connectores a la izquierda, 3 270 grados
hdmi_cvt 1024 600 60 6 0 0 0
```

### LongRunner 5" XPT2046 \(HDMI GPIO Táctil\)

{% embed url="https://www.amazon.es/gp/product/B07QC6G6S9/ref=ppx\_yo\_dt\_b\_asin\_image\_o00\_s00?ie=UTF8&psc=1" %}

Cambios a realizar para su funcionamiento \(añadir al final del fichero\):

```text
sudo nano /boot/config.txt
```

Y modificaremos/añadiremos lo siguiente:

```bash
# uncomment if you get no picture on HDMI for a default "safe" mode
#hdmi_safe=1

# uncomment this if your display has a black border of unused pixels visible
# and your display can output without overscan
disable_overscan=0

# uncomment if hdmi display is not detected and composite is being output
#hdmi_force_hotplug=1

# uncomment to force a specific HDMI mode (this will force VGA)
hdmi_group=2
hdmi_mode=1
hdmi_mode=87
hdmi_cvt=800 480 60 6 0 0 0

# Enable touchscreen on Elecrow HDMI interface.
dtparam=spi=on
dtparam=i2c_arm=on
dtoverlay=ads7846,cs=1,penirq=25,penirq_pull=2,speed=50000,keep_vref_on=0,swapxy=0,pmax=255,xohms=150,xmin=200,xmax=3900,ymin=200,ymax=3900
dtoverlay=w1-gpio-pullup,gpiopin=4,extpullup=1
```

### **FYSECT CTP40 \(GPIO\)**

Para esta pantalla asegurarse de instslar los drivers siguiendo las instrucciones 

{% embed url="https://github.com/FYSETC/FYSETC-CTP40/tree/main/Pi3" %}



## Soporte/Ayuda

{% embed url="https://discord.com/invite/PeFgKUuZ?utm\_source=Discord%20Widget&utm\_medium=Connect" caption="Discord OctoDash" %}

{% embed url="https://github.com/UnchartedBull/OctoDash/blob/master/README.md" caption="OctoDash Github" %}

{% embed url="https://unchartedbull.github.io/OctoDash/index.html" caption="OctoDash Website" %}





