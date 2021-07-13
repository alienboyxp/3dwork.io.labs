# Instalando Octoprint

El proceso de instalacion es bastante sencillo, en cualquier caso os detallamos paso a paso el proceso y algunos pequeños trucos que os permitan instalarlo de la forma correcta.

## Instalación usando el generador de imagen OctoPi

Podemos usar la herramienta **Raspberry Pi Imager** para personalizar, crear y quemar la imagen de OctoPi.

* Descargaremos e instalaremos el [Raspberry Pi Imager](https://raspberrypi.org/software) en nuestro ordenador
* Buscaremos la imagen de OctoPi en **Choose OS** y dentro de **Other Specific Purpose OS**
* Abriremos **Open Advanced Options** para configurar las opciones de nuestra Wifi si es necesario. Seleccionaremos nuestro **SSID**, **contraseña** y **país**

![](../../../.gitbook/assets/image%20%28170%29.png)

* Elegiremos la SD y grabaremos la imagen.
* Extraeremos de forma segura la SD y la colocaremos en nuestra Raspberry, al encenderla y pasados unos segundos ya podremos conectarnos a ella tal como indicamos en los siguientes puntos de la guia.

## Instalación usando la imagen de OctoPi

* Descargaremos la imagen de Octoprint a nuestro ordenador desde aquí [https://octopi.octoprint.org/latest](https://octopi.octoprint.org/latest)
* Conectaremos la SD a nuestro ordenador
* Ejecutaremos Etcher y haremos click en Seleccionar Imagen, seleccionamos la imagen de Octoprint que descargamos previamente. Asegúrate que seleccionaste el dispositivo SD correcto ya que el proceso borrará completamente su contenido y no se podrá recuperar. Daremos a Flash y esperaremos a que el proceso finalice, después de realizar el proceso de copiado Etcher realizará una validación... es aconsejable hacerla ya que muchas SD "baratas" pueden dar problemas o no ser 100% fiables, siempre aconsejamos usar SD de calidad. Una vez Etcher finalizó el proceso podemos cerrarlo.
* Si vamos a conectar nuestra Pi mediante WiFi si no disponemos de un cable de red a nuestro router cerca te aconsejamos realizar el siguiente procedimiento para configurarlo de forma correcta. Abre tu SD con la imagen de Octorprint ya grabada y localiza el fichero octopi-wpa-supplicant.txt y lo abrimos con el editor de textos de nuestra elección \(recuerda los sugeridos más arriba\). Tenemos 3 tipos de seguridad disponibles para configurar dicho fichero que dependerá de como tenemos configurada nuestra WiFi... estos son... WPA/WPA2 , WEP , Abierta/Insegura. Por norma general suelen estar en WPA/WPA2. Ahora iremos a la sección del fichero que coincide con nuestra configuración de WiFi y lo editaremos... para nuestro ejemplo usaremos una WiFi con SSID 3dworkio y el password WPA/WPA2 123456789 por lo que iremos a esta sección del fichero: **`## WPA/WPA2 secured # network={ # ssid="put SSID here" # psk="put password here" #}`** Y lo dejaremos de la siguiente forma \(presta atención a los \# que se eliminaron y donde pusimos los datos de nuestra WiFi\): **`## WPA/WPA2 secured network={ ssid="3dworkio" psk="123456789" }`**
* Guardamos los cambios y saldremos del editor.
* Extraemos de forma segura la SD de nuestro ordenador y la ponemos dentro de nuestra Pi.

## Como acceder a nuestro OctoPrint/OctoPi

* Conectaremos la alimentación de la Pi y esperaremos unos cuantos minutos para que arranque.
* Utilizaremos el scanner de red para localizar la IP asignada a nuestra pi \(la ip tiene un formato numérico de este estilo xxx.xxx.xxx.xxx\)
* Abriremos nuestro cliente SSH, Terminus si seguiste nuestro consejo y crearemos una nueva conexión a nuestra Pi usando los siguientes datos: **`IP/Hostname = xxx.xxx.xxx.xxx (la IP identificada en el paso anterior) Usuario = pi Password = raspberry`** Una vez creado nos conectaremos y si todo funciona correctamente veremos que conectamos y tenemos un terminal para poder lanzar comandos.
* Una de las primeras cosas a realizar una vez conectados es la de expandir el sistema de ficheror para aprovechar nuestra SD al máximo, para ello ejecutaremos el siguiente comando desde el terminal: **`sudo raspi-config`** Se nos abrirá un menú con opciones que podemos movernos con las flechas. Iremos a **`Advanced Options`** y desde ahí seleccionaremos **`Expand Filesystem`** nos pedirá una confirmación para hacer este cambio que aceptaremos.
* Por defecto el nombre de red es octoprint.local pero si queremos personalizarlo podemo abrir de nuevo el comando anterior, ir a **`Network Options/Hostname`** y desde ahí cambiaremos el de por defecto \(idealmente letras y números sin espacios\)
* Después de estos cambios reiniciaremos la Pi ya sea físicamente retirando la alimentación o desde el terminal con el comando **`sudo reboot`**.
* Ahora iremos a nuestor ordendor/teléfono/tablet, abriremos nuestro navegador preferido y pondremos la IP asignada a nuestra Pi para comenzar la configuración de Octoprint!!!

