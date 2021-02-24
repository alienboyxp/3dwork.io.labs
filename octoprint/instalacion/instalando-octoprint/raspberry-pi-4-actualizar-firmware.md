---
description: Actualizar firmware a la última versión disponible usando rpi-eeprom-update
---

# Raspberry Pi 4 – Actualizar firmware

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

