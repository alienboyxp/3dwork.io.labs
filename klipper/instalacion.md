# Instalación

## Que necesitamos?

* Raspberry Pi
* SD, aconsejable que sea rápida y con espacio suficiente para almacenar el sistema y archivos gcode
* Cable conexión entre nuestra Pi y nuestra impresora

## **Antes de comenzar!!!**

![](../.gitbook/assets/image%20%2844%29.png)

Por favor entiende que instalar Klipper en tu impresora require de cierta experiencia con impresoras 3d, hardware y software. Puede no ser una tarea trivial para gente que se acaba de iniciar en el mundo 3D o que no dispone de unos minimos conocimientos ya que puedes romper tu impresora o pi durante el proceso

Lee antes la guía completa y entiende todos los pasos que explicamos. Si tienes cualquier duda del proceso por favor te aconsejamos unirte al grupo de Telegram [https://t.me/Klipper\_Firmware\_ES](https://t.me/Klipper_Firmware_ES) donde seguro te echaran una mano.

**No nos hacemos en ningún caso de cualquier daño o problema que se pueda ocasionar siguiendo esta guía. Estás haciendo esto bajo tu propia responsabilidad.**

## Preparación de nuestra pi

Para poder instalar Klipper debemos preparar nuestra Pi. Esta guía no va a entrar en detalle este paso tan solo haremos un listado rápido de los pasos a seguir:

1. Descargaremos una imagen de [Raspberry Pi OS Lite](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) y la "quemaremos2 en nuestra SD usando balenaEtcher o similar. Tienes información detallada de los pasos a seguir [aqui](https://3dwork.qitec.net/octoprint/instalacion/instalando-octoprint).
2. Verificaremos que nuestra pi arranca y podemos conectarnos por red wifi o cable
3. Habilitaremos SSH y verificaremos que podemos conectar por SSH
4. Ejecutaremos `sudo raspi-config` :
   1. Haremos un reset del password por defecto
   2. configuraremos el hostname si queremos
5. Ejecutaremos `sudo apt-get update` y `sudo apt-get upgrade` para actualizar nuestra pi

## Instalando Klipper usando Kiauh

Kiauh es un grandisimo avance para Klipper. Kiauh permite mediante unos sencillos menus instalar Klipper y sus principales componentes de una forma sencilla:

* Instalación de Klipper
* Instalar Moonraker \(API que permite interactuar con Klipper por terceros como Mailsail, Fluidd o KlipperScreen\)
* Instalación de diferentes interfaces web como Mailsail, Fluidd, Duet Web Control o Octoprint
* Instalar KlipperScreen \(un fork de OctoScreen para Klipper\)
* Actualizar todos los componentes \(Octoprint de ser instalado se gestionará directamente desde el mismo\)
* Eliminar cualquier componente
* Hacer un backup del sistema
* Preparar el firmware Klipper para nuestra MCU \(placa de la impresora\)
* Detectar el serial donde comunicar con nuestras MCU

Puedes encontrar un listado completo de las características [aquí](https://github.com/th33xitus/kiauh/blob/master/docs/features.md).

### Instalando Kiauh

Ahora que ya tenemos el sistema base en nuestra PI, esta actualizado y podemos conectarnos por SSH a ella comenzaremos a instalar Kiauh

```text
sudo apt-get install git -y
cd ~
git clone https://github.com/th33xitus/kiauh.git
cd kiauh
chmod +x kiauh.sh scripts/*
./kiauh.sh
```

{% hint style="info" %}
Con estos comandos hacemos...

* Instalamos git para la gestión de repositorios
* Clonamos el repositorio de Kiauh
* Permitimos que los scripts descargados puedan ser ejecutados
* Ejecutamos Kiauh

Si necesitamos volver a lanzar Kiauh en el futuro podemos ir a `home/pi/kiauh y ejecutar ./kiauh.sh`
{% endhint %}

Una vez hemos realizado el proceso completo y lanzamos Kiauh deveriamos ver un menú como este:

![](../.gitbook/assets/image%20%2845%29.png)

### Instalando Klipper, Moonraker y Fluidd

Para nuestra guía vamos a usar Klipper que es el core del sistema, Moonraker que va a crear una API para poder gestionar la comunicación entre Klipper y otros componentes y Fluidd como interfaz web para gestionar nuestro Klipper.

#### Instalación de Klipper

De las opciones del menú eligiremos la opcion 1 para acceder al menú de instalación

![](../.gitbook/assets/image%20%2846%29.png)

Volveremos a elegir la opción 1 y comenzaremos el proceso de instalación:

* Eligiremos compilar nuestro firmware
* Por ahora no elegiremos actualizar nuestra MCU

![](../.gitbook/assets/image%20%2847%29.png)

