# Telegraph

## Indice <a id="Indice"></a>

* Que es Octoprint?
* Que Necesito?
* Instalando Octoprint - Configuración de una dirección IP estática en Raspberry Pi - Raspberry Pi 4, actualización de firmware
* Configurando Octoprint
* Plugins imprescindibles - DisplayLayerProcess, que te permite enviar estado de la impresión a la pantalla de tu impresora - PrintTimeGenious, que te da estimaciones reales del tiempo de finalización de una impresión - Cancel Objects, que te permite, si tu firmware Marlin lo tiene habilitado, cancelar piezas que esten saliendo mal sin parar toda la impresión - IPConnect, que te muestra la IP asignada y por si esta cambia - Arc-Welder, mejora calidad y fiabilidad impresiones mediante ARC - Filament Manager, gestiona tus bobinas de filamentos - Bed Visualizar, nivela tu cama visualmente - Enclosure, expande oociones con gesrion de dispositivos GPIO
* Acceso remoto a Octoprint - Telegram - Octoeverywhere - VPN
* Octoprint Avanzado - OctoDash - Control de varias impresoras  - Usando varias instancias  - Usando contenedores - OctoFarm

## ----

## Que es Octoprint? <a id="Que-es-Octoprint?"></a>

Octoprint es una aplicación Open-Source completamente gratuita que nos permite monitorizar y gestionar nuestra impresora 3D de forma remota utilizando una Raspberry Pi.

De esta forma, y con nuestro navegador web preferido, podremos efectuar nuevas impresiones, monitorizar la temperatura de nuestra cama o boquilla \(nozzle\), ver en vídeo nuestra impresión en tiempo real y muchas otras cosas más.

## Que Necesito? <a id="Que-Necesito?"></a>

Antes de comenzar con la instalación debemos revisar que tengamos todo lo necesario:

* Raspberry Pi, aconsejable 3+ o superior
* Fuente de alimentación para nuestra Pi, es importante que esta tenga al menos 2A y sea constante en la entrega de voltaje para evitar problemas. Las originales suelen ser la mejor opción.
* Tarjeta Micro SD y un lector de estas si nuestro ordenador no tiene lector integrado, con un mínimo de 4Gb aunque lo aconsejable son 8Gb o más
* Un ordenador o teléfono/tablet que tenga acceso a tu red para configurar todo
* Disponer de un Marlin configurado para comunicación serial y algunas funciones que mejoran su gestión. Podéis encontrar más info al final de la guía

Tambien es importante disponer de una serie de aplicaciones para poder realizar toda la instalación:

* Un editor de textos como [Notepad++](https://notepad-plus-plus.org/download/) o [Atom](https://atom.io/). Es importante usar uno de estos ya que algunos como Word, Wordpad o editores con formato enriquecido pueden añadir caracteres de escape en los ficheros de configuración y no funcionar correctamente.
* Un cliente SSH para conectar por terminal a nuestra Pi, aconsejamos [Terminus](https://termius.com/) ya que es multiplataforma y dispone de versión para teléfono/tablet... además de que tiene un interfaz sencilla y funcional.
* Un scanner de red para nuestro teléfono/tablet para poder detectar si nuestra Pi se enlazó correctamente en nuesta red. En este caso aconsejamos [Fing](https://www.fing.com/products/fing-app).
* Necesitamos también una aplicación para crear nuestra SD, sugerimos [Etcher](https://www.balena.io/etcher/) que también es multiplataforma.
* Por último necesitaremos una imagen de Octoprint para nuestra SD que podemos descargar desde aquí [https://octoprint.org/download/](https://octoprint.org/download/).

## Instalando Octoprint <a id="Instalando-Octoprint"></a>

* Descargaremos la imagen de Octoprint a nuestro ordenador
* Conectaremos la SD a nuestro ordenador
* Ejecutaremos Etcher y haremos click en Seleccionar Imagen, seleccionamos la imagen de Octoprint que descargamos previamente. Asegúrate que seleccionaste el dispositivo SD correcto ya que el proceso borrará completamente su contenido y no se podrá recuperar. Daremos a Flash y esperaremos a que el proceso finalice, después de realizar el proceso de copiado Etcher realizará una validación... es aconsejable hacerla ya que muchas SD "baratas" pueden dar problemas o no ser 100% fiables, siempre aconsejamos usar SD de calidad. Una vez Etcher finalizó el proceso podemos cerrarlo.
* Si vamos a conectar nuestra Pi mediante WiFi si no disponemos de un cable de red a nuestro router cerca te aconsejamos realizar el siguiente procedimiento para configurarlo de forma correcta. Abre tu SD con la imagen de Octorprint ya grabada y localiza el fichero octopi-wpa-supplicant.txt y lo abrimos con el editor de textos de nuestra elección \(recuerda los sugeridos más arriba\). Tenemos 3 tipos de seguridad disponibles para configurar dicho fichero que dependerá de como tenemos configurada nuestra WiFi... estos son... WPA/WPA2 , WEP , Abierta/Insegura. Por norma general suelen estar en WPA/WPA2. Ahora iremos a la sección del fichero que coincide con nuestra configuración de WiFi y lo editaremos... para nuestro ejemplo usaremos una WiFi con SSID 3dworkio y el password WPA/WPA2 123456789 por lo que iremos a esta sección del fichero: \#\# WPA/WPA2 secured \# network={ \# ssid="put SSID here" \# psk="put password here" \#} Y lo dejaremos de la siguiente forma \(presta atención a los \# que se eliminaron y donde pusimos los datos de nuestra WiFi\): \#\# WPA/WPA2 secured network={ ssid="3dworkio" psk="123456789" }
* Guardamos los cambios y saldremos del editor.
* Extraemos de forma segura la SD de nuestro ordenador y la ponemos dentro de nuestra Pi.
* Conectaremos la alimentación de la Pi y esperaremos unos cuantos minutos para que arranque.
* Utilizaremos el scanner de red para localizar la IP asignada a nuestra pi \(la ip tiene un formato numérico de este estilo xxx.xxx.xxx.xxx\)
* Abriremos nuestro cliente SSH, Terminus si seguiste nuestro consejo y crearemos una nueva conexión a nuestra Pi usando los siguientes datos: IP/Hostname = xxx.xxx.xxx.xxx \(la IP identificada en el paso anterior\) Usuario = pi Password = raspberry Una vez creado nos conectaremos y si todo funciona correctamente veremos que conectamos y tenemos un terminal para poder lanzar comandos.
* Una de las primeras cosas a realizar una vez conectados es la de expandir el sistema de ficheror para aprovechar nuestra SD al máximo, para ello ejecutaremos el siguiente comando desde el terminal: sudo raspi-config Se nos abrirá un menú con opciones que podemos movernos con las flechas. Iremos a Advanced Options y desde ahí seleccionaremos Expand Filesystem nos pedirá una confirmación para hacer este cambio que aceptaremos.
* Por defecto el nombre de red es octoprint.local pero si queremos personalizarlo podemo abrir de nuevo el comando anterior, ir a Network Options/Hostname y desde ahí cambiaremos el de por defecto \(idealmente letras y números sin espacios\)
* Después de estos cambios reiniciaremos la Pi ya sea físicamente retirando la alimentación o desde el terminal con el comando sudo reboot.
* Ahora iremos a nuestor ordendor/teléfono/tablet, abriremos nuestro navegador preferido y pondremos la IP asignada a nuestra Pi para comenzar la configuración de Octoprint!!!

### Configuración de una dirección IP estática en Raspberry Pi <a id="Configuraci&#xF3;n-de-una-direcci&#xF3;n-IP-est&#xE1;tica-en-Raspberry-Pi"></a>

1. Para comenzar a configurar una dirección IP estática en nuestra Raspberry Pi, primero necesitaremos recuperar información sobre nuestra configuración de red actual.

Primero recuperemos el enrutador definido actualmente para tu red ejecutando el siguiente comando.

**ip r \| grep predeterminado**

Al usar este comando, deberías obtener un resultado similar al que tenemos a continuación.

**default via 192.168.0.1 dev eth0 proto dhcp src 192.168.0.159 metric 202**

Anota la primera IP mencionada en esta cadena. Por ejemplo, la IP que vamos a tomar nota de este comando es "192.168.0.1". Esta dirección IP es la dirección actual del enrutador.

2. A continuación, recuperemos también el servidor DNS actual.

Podemos hacer esto abriendo el archivo de configuración “resolv.conf” ejecutando el siguiente comando.

**sudo nano /etc/resolv.conf**

Desde este comando, debería ver las líneas de texto a continuación.

**\# Generado por resolvconf  
nameserver 192.168.0.1**

Tome nota de la IP junto a "nameserver". Esto definirá el servidor de nombres en nuestros próximos pasos.

3. Ahora que hemos recuperado nuestra IP actual del “enrutador” y la IP del servidor de nombres, podemos proceder a modificar el archivo de configuración “dhcpcd.conf” ejecutando el siguiente comando.

Este archivo de configuración nos permite modificar la forma en que Raspberry Pi maneja la red.

**sudo nano /etc/dhcpcd.conf**

4. Dentro de este archivo, pon las siguientes líneas ak final.

Primero, debe decidir si desea configurar la IP estática para su conector “eth0” \(Ethernet\) o su conexión “wlan0” \(WiFi\). Decida cuál desea y reemplace “” por él.

Asegúrese de reemplazar "" con la dirección IP que desea asignar a su Raspberry Pi. Asegúrese de que esta no sea una IP que pueda conectarse fácilmente a otro dispositivo en su red.

Reemplaza "" con la dirección IP que recuperaste en el paso 1 de este tutorial.

Finalmente, reemplace “” con la IP del servidor de nombres de dominio que desea utilizar. Esta es la IP que obtuvo en el paso 2 de este tutorial u otra como Google "8.8.8.8" o "1.1.1.1" de Cloudflare.

interfaz  
dirección\_ip estática = / 24  
enrutadores estáticos =  
static domain\_name\_servers =  
Ahora guarde el archivo presionando CTRL + X, luego Y seguido de ENTER.

5. Ahora que hemos modificado el archivo de configuración DHCP de nuestra Raspberry Pi para que utilicemos una dirección IP estática, debemos continuar y reiniciar la Raspberry Pi.

Reiniciar la Raspberry Pi permitirá que se carguen nuestros cambios de configuración y se eliminen los antiguos.

Al reiniciar, la Raspberry Pi intentará conectarse al enrutador utilizando la dirección IP estática que definimos en nuestro archivo "dhcpd.conf".

Ejecute el siguiente comando para reiniciar su Raspberry Pi.

**sudo restart**

###  Prueba de la IP estática <a id="Prueba-de-la-IP-est&#xE1;tica"></a>

Una vez que su Raspberry Pi haya terminado de reiniciarse, ahora deberías poder conectarte utilizando la dirección IP que especificó.

Si se está conectando localmente y desea verificar la dirección IP estática configurada correctamente, puede hacerlo ejecutando el siguiente comando.

ping nombre\_de\_host -I

Desde este comando, ahora debería poder ver su nueva dirección IP estática.

Si es la IP que esperaba, entonces ha configurado con éxito una dirección IP estática en su Raspberry Pi.

El uso de una IP estática será útil cuando necesite recordar la IP, como usar FTP o configurarlo para que actúe como NAS.

### Raspberry Pi 4 – Actualizar firmware a la última versión disponible usando rpi-eeprom-update <a id="Raspberry-Pi-4-&#x2013;-Actualizar-firmware-a-la-&#xFA;ltima-versi&#xF3;n-disponible-usando-rpi-eeprom-update"></a>

La Raspberry Pi 4 es sin duda el mejor modelo que han lanzado nunca, con hasta 4GB de RAM, con USB 3.0 y LAN 1GbE que no comparten bus y dos micro HDMI, es una bestia parda para todo tipo de situaciones. Sin embargo es cierto que salió al mercado con un problema de temperatura que hacía que se fuera a más de los 60 y 70 grados, eso con disipador.  
Antes de realizar nada, actualizaremos nuestros paquetes de raspbian:  
**sudo apt-get update && sudo apt-get upgrade**  
Una vez que termina de actualizar todo, instalaremos, es probable que lo tuvieramos ya instalado, rpi-eeprom-update:  
**sudo apt-get install rpi-eeprom-update**  
Haremos un reinicio de la Raspberry Pi 4:  
**sudo reboot**  
Ya con el nuevo paquete instalado, lo lanzaremos desde la consola, y veremos si mi firmware está muy obsoleto:  
**sudo rpi-eeprom-update**  
**\*\*\* UPDATE REQUIRED \*\*\***  
**BOOTLOADER: update required**  
**CURRENT: Fri 10 May 2019 06:40:36 PM UTC \(1557513636\)**  
 **LATEST: Tue 10 Sep 2019 10:41:50 AM UTC \(1568112110\)**  
**VL805: update required**  
**CURRENT: 00013701**  
 **LATEST: 000137ab**  
Con esta información, lanzaremos el mismo comando pero esta vez con el atributo -a para aplicar el update:  
**pi@raspberrypi:~$ sudo rpi-eeprom-update -a**  
**\*\*\* INSTALLING EEPROM UPDATES \*\*\***  
**BOOTLOADER: update required**  
**CURRENT: Fri 10 May 2019 06:40:36 PM UTC \(1557513636\)**  
 **LATEST: Tue 10 Sep 2019 10:41:50 AM UTC \(1568112110\)**  
**VL805: update required**  
**CURRENT: 00013701**  
 **LATEST: 000137ab**  
**EEPROM updates pending. Please reboot to apply the update.**  
Haremos de nuevo un reinicio de la RPi 4:  
**sudo reboot**  
Volveremos a realizar el proceso de verificar la versión y asegurar que ya estsmos al dia.  
Si quisieramos saber qué cambios hay en cada Firmware, [podemos irnos a GitHub](https://github.com/raspberrypi/firmware/releases) y ver los commits por cada Release.

## Configurando Octoprint <a id="Configurando-Octoprint"></a>

* Seguiremos el asistente de configuración que es muy sencillo e intuitivo, no tiene mucho misterio.
* Una vez finalizada la configuración inicial ya estaremos listos para conectar a nuestra impresora donde idealmente poniendo el puerto/velocidad en AUTO y teniendo el cable USB conectado entre nuestra Pi e impresora debería conectar y poder empezar a tener lecturas de temperaturas.
* Para comprobar que todo esta correcto iremos a la pestaña Control y desde ahi probaremos a realizar un homing o mover nuestros ejes, es un proceso muy intuitivo.
* También podemos comprobar que podemos ajustar las temperaturas de nuestro hotend y cama desde la pestaña Temperature.
* Ya está!!! tienes tu impresora con Octoprint para poder gestionarla remotamente!!! puedes arrastrar tus gcode directamente a la web de Octoprint para subirlos y dar la orden de imprimir.

## Plugins imprescindibles <a id="Plugins-imprescindibles"></a>

Ahora que ya tenemos nuestro Octoprint totalmente funcional y después de jugar y hacer nuestras primeras pruebas llega el momento de llevarlo a otro nivel y ver el verdadero potencial de Octoprint mediante sus plugins.

Estos plugins se instalan desde el área de configuración \(icono de llave inglesa en la barra superior\) desde el Administrador de Complementos \(Plugin Manager\).

Os sugerimos una lista de los que consideramos nuestros "imprescindibles".

* [DisplayLayerProcess](https://github.com/OllisGit/OctoPrint-DisplayLayerProgress): Mediante este plugin podremos añadir mucha información interesante sobre el estado de la impresión en nuestra pantalla como el progrso \(M73\), tiempos estimados de finalización, información de capa, información del estado de nuestro propio cambio de filamento \(M600\), estado de la impresora, etc... Además aplica cambios interesantes al interfaz de Octoprint para poder visualizar toda esta información remotamente en el caso que lo creamos interesante.
* [PrintTimeGenious](https://github.com/eyal0/OctoPrint-PrintTimeGenius): Con este otro plugin imprescindible podremos mejorar muchísimo las estimaciones de tiempo de impresión con respecto al sistema de cálculo de Octoprint.
* [Cancel Objects](https://github.com/paukstelis/Octoprint-Cancelobject): A quien no le ha pasado nunca que durante una impresión de varias piezas una de ellas ha quedado dañada o se está imprimiendo de forma incorrecta. Cancel Objects te permitirá seleccionar esa parte de la impresión y cancelarla permitiendo de esa manera que no tengas problemas con el resto de piezas.
* [IpOnConnect](https://github.com/jneilliii/OctoPrint-ipOnConnect): Este sencillo plugin no va a cambiar tu vida pero si que te facilitará el encontrar en que IP se encuentra tu Octoprint en el caso que no tenga una IP estática asignada.
* [Arc-Welder](https://plugins.octoprint.org/plugins/arc_welder/): Este potentísimo plugin promete mejorar la calidad de tus impresiones además de ser muy muy adecuado para el uso de tu impresora desde Octoprint. Básicamente se encarga de mejorar el gcode laminado para sustituir movimientos lineales por arcos que son más optimos con un descenso drástico de la comunicación de gcodes entre Octoprint y tu impresora \(que ayudará a evitar cuellos de botella\), mejorar la calidad \(reducción la cantidad de movimientos y reduciendo las extrusiones/retracciones\) además de reducir considerablemente el tamaño de tus archivos. No es una tecnología para nada nueva pero con el procesamiento de la electrónica actual esta comenzando a ser una referencia. Su uso es muy sencillo, puedes ponerlo en modo automático para que cuando subes un nuevo gcode automático lo transforme \(añadirá uno nuevo con .aw.gcode en su nombre\) o hacerlo manualmente desde un nuevo icono \(flechas\) en el listado de ficheros. Cuenta con un interfaz que te informa del estado de transformación además que te da datos de estadísticas de las mejoras aplicadas. Tienes una parte de configuración que puedes personalizar a tu gusto para generar el nuevo gcode optimizado para tu máquina. Es importante recalcar que para que funcione correctamente el Marlin de tu impresora debe tener habilitado ARC\_SUPPORT \(configuration\_adv.h\) que desde la versión 2.0.6 ha sido mejorado enormemente. Si no sabes si lo tienes o no habilitado puedes lanzar un comando M115 desde tu cliente terminal favorito a tu impresora para ver el estado de esta opción. Por supuesto es también compatible con otros firmwares pero deberás comprobar la compatibilidad en cada caso. 
* [Filament Manager](https://github.com/OllisGit/OctoPrint-FilamentManager): Como casi todos tenemos gran cantidad de bobinas en casa pero no tenemos gestionado el coste, cuanto nos queda o nos será suficiente para finalizar mi impresión? Filament Manager nos va a permitir poder gestionar todo esto para poder conocer en cada momento el estado de nuestro "stock" de filamentos y si tenemos que tener o no cuidado por si el filamento se nos acabase durante la impresion que queremos empezar.
* [Bed Visualizer](https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/): Uno de los aspectos más importantes a la hora de imprimir es tener correctamente calibrada nuestra máquina y en especial nuestra cama de impresión. Bed Visualizer nos va a ayudar en este aspecto mostrando un mapa en 3D con el estado de nuestro sistema de nivelación de malla. Soporta los firmwares más importantes a día de hoy usados en impresión 3D como puede ser Marlin, PrusaFirmware y Klipper.
* [Enclosure](https://github.com/vitormhenrique/OctoPrint-Enclosure): Si queremos potenciar todavía más el uso de Octoprint este plugin lo lleva a otro nivel ya que nos permite integrar y gestionar componentes externos de nuestra impresora como control de temperatura, luces, ventiladores, reles y sensores de filamento usando el puerto GPIO de nuestra raspberry. Es imposible poder listar las opciones de este plugin así que lo mejor es remirtiros a su propio repositorio en Github donde encontrareis al detalle todas sus funciones [https://github.com/vitormhenrique/OctoPrint-Enclosure](https://github.com/vitormhenrique/OctoPrint-Enclosure) .

## Acceso remoto a Octoprint <a id="Acceso-remoto-a-Octoprint"></a>

### Telegram <a id="Telegram"></a>

Una de las formas más sencillas y rápidas de controlar nuestra impresora remotamente es usando el plugin [Telegram para Octoprint](https://github.com/fabianonline/OctoPrint-Telegram). Os indicamos unos pasos rápidos para poder instalarlo y configurarlo correctamente:

**Creación del Bot**

Telegram permite la creación de nuestros propios bots, para ello lo primero es crear uno simplemente desde Telegram abriremos una conversación con [@BotFather](http://telegram.me/botfather) desde tu cliente Telegram y lanzamos el comando /start para comenzar el proceso.

1. Envía /newbot y te solicitará un nombre para tu nuevo bot, para el ejemplo usaremos 3dWork Bot
2. Una vez establecemos el nombre del bot nos solicitará un nombre de usuario para ese bot, es importante que acabe en bot, para nuestro caso usaremos @ThreeDWorkBot \(sin la arroba\)
3. Inmediatamente BotFather nos devolverá un código llamado token que será el que usaremos más tarde. Es importantísimo guardarlo en un sitio seguro.

**Configuración del plugin Telegram en Octoprint**

Cambiamos a nuestro interfaz web de Octoprint para instalar y configurar el plugin de Telegram.

1. Podemos instalar el plugin desde el Administrador de Complementos \(Plugin Manager\) o instalarlo manualmente desde [aquí](https://github.com/fabianonline/OctoPrint-Telegram/archive/stable.zip)
2. Una vez instalado nos solicitará reiniciar el servidor de Octoprint
3. Una vez reiniciado nos abrirá el asistente de configuración de Telegram, en el caso que no lo haga o no hagamos en el siguiente reinicio en las opciones de configuración de Octoprint puedes encontrar Telegram como plugin y de ahi realizar el proceso.
4. Introduciremos el token que obtuvimos desde BotFather en el punto 3 del paso anterior y probaremos que funcione correctamente.

**Configuración del usuario**

Ahora cambiamos de nuevo a nuestre cliente de Telegram, debemos abrir un chat con nuestro bot... para ello tenemos varias formas de buscarlo ya sea por el buscador de usuarios de Telegram, en la conversación con BotFather o en la captura del paso anterior nos lo indica en Token status.

Una vez abierto el chat con nuestro bot lanzaremos el comando /start y este nos indicará que tenemos que volver a Octoprint para ajustar los permisos para poder interactuar con el.

Cambiamos de nuevo a Octoprint, en su configuracion, panel izquierdo y abajo veremos Telegram. Dentro de la configuración de Telegram tendremos un apartado de Known Chats, si nuestro chat no aparece pulsaremos el botón Reload y debería aparecernos nuestro usuario.

Ahora daremos al icono del lápiz en la sección Action y permitiremos la ejecución de comandos y notificaciones.

Una vez habilitados volvemos a Known Chats y entraremos pulsando en el icono de Command y Notify para marcar las opciones que más nos interesen.

Ya podremos interactuar desde nuestre cliente Telegram con nuestra impresora!!! Facil verdad?

**Notificaciones**

En la configuración del plugin de Telegram tenemos una sección para configurar las notificaciones \(Messages at...\) estas están escritas en inglés pero podemos cambiarlas por lo que nos interese más.

En la parte de iconos azules podremos ver información sobre el lenguaje markup que se utiliza, las variables y los iconos que podemos usar.

Además podemos ajustar las opciones de enviar imágenes, combinar mensajes, etc...

**Comandos y personalización del bot**

Ya tenemos todo funcional pero como nos gusta dejarlo todo perfecto para "hablar" con nuestra impresora tenemos que recordar la lista de comandos que se pueden usar o pedirle con un /help que nos las liste.

Volveremos a hablar con BotFather y le enviamos el comando /setcommands y pegamos el listado siguiente:

```text
abort - Detiene la impresión. Requiere confirmación.
shutup - Silencia el bot hasta que la impresión termine.
dontshutup - Vuelve a permitir al bot que te mande notificaciones.
status - Información del estado de la impresora.
settings - Configura el intervalo de las notificaciones, ajustable en cada altura o en cada tiempo. Por defecto 5 milímetros o cada 15 minutos.
files - Muestra un listado de ficheros en el que tienes guardado en la microSD de la Raspberry. También te permite subir nuevos archivo e incluso descargarlos. Si presionamos sobre un archivo GCode nos mostrará información sobre el tiempo de impresión, filamento que gastará y lo que ocupa.
print - Comienza una impresión. En ella seleccionamos el archivo a imprimir.
togglepause - Pausa o reanuda la impresión.
con - Conecta / desconecta la impresora.
upload - Sube un archivo a Octoprint.
sys - Ejecuta un comando de Octoprint que pueden ser reiniciar, apagar o reiniciar el servidor.
ctrl - Controla la impresora, como haríamos con G28, G0 Z10... Es un poco inútil y necesita ser configurado.
tune - Ajusta la temperatura del cabezal, cama, feedrate y flowrate. Con ella podemos precalentar la impresora.
user - Muestra la información del usuario, permisos que tienes y las notificaciones permitidas.
help - Muestra la lista de comandos.
```

Con esto al hablar con el bot de nuestra impresora nos rellenará automaticamente al poner / el comando que deseemos o tendremos en la caja de texto de chat un icono / que nos listara los comandos y su descripción.

Por último pero no menos importante personalizaremos el icono de nuestro bot, para ello vovemos de nuevo a BotFather y le enviamos el comando /setuserpic, nos solicitará que subamos una imagen que será la que se usará en el perfil del bot de nuestra impresora.

* Discord
* Octoeverywhere
* VPN

https://www.pivpn.io/

## Octoprint Avanzado <a id="Octoprint-Avanzado"></a>

### OctoDash <a id="OctoDash"></a>

https://telegra.ph/OctoDash-añade-una-pantalla-de-control-a-tu-Octoprint-muy-funcional-y-bonita-12-21

### Control de varias impresoras <a id="Control-de-varias-impresoras"></a>

* Usando varias instancias

[https://telegra.ph/Varias-instancias-de-Octoprint-en-una-misma-Raspberry-12-21](https://telegra.ph/Varias-instancias-de-Octoprint-en-una-misma-Raspberry-12-21)

* Usando contenedores

https://github.com/AmedeeBulle/octoprint-containers

### OctoFarm <a id="OctoFarm"></a>

