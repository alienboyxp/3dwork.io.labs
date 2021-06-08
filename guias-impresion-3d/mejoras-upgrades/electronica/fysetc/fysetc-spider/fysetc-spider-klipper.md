---
description: Instalar Klipper en nuestra Fysetc Spider
---

# Fysetc SPIDER - Klipper

Klipper es un firmware/sistema de gestión de impresoras 3D que usa una Raspberry Pi y transforma nuestra controladora, en este caso nuestra Spider, en un mero controlador de motores, sensores y salidas de pines.

No entraremos en el detalle de como instalar Klipper ya que podéis seguir la guía que podéis encontrar [aqui](../../../../../klipper/klipper-1.md).

## Instalación del firmware Klipper en nuestra Spider

Uno de los pasos de instalación de Klipper es la creación y actualización del firmware de la placa. 

Para nuestra Spider estas son las opciones a usar cuando realizamos el paso "menuconfig"

* habilitar "extra low-level configuration setup"
* seleccionar "12Mhz crystal" como referencia de reloj

### Boot address opción 1

Actualmente no disponemos de la opción para poder definir en Klipper la "boot address" así que disponemos de los siguientes firmware preconfigurados  [klipper.bin](https://github.com/FYSETC/FYSETC-SPIDER/tree/main/firmware/Klipper) y [klipper-UART0.bin](https://github.com/FYSETC/FYSETC-SPIDER/tree/main/firmware/Klipper) \(la diferencia entre ambos la puedes encontrar  [here](https://github.com/FYSETC/FYSETC-SPIDER/tree/main/firmware/Klipper)\) estan en el  boot address 0x8000000. Por favor sigue los pasos para [subir el firmware por DFU](./#firmware) pero **indicando que el "Start address" es 0x08000000 y seleccionando klipper.bin pero no firmware.bin.**

### Boot address opción 2

Si Klipper soporta en un futuro "boot address" para los procesadores STM32F446 podremos definir como "boot address" 0x08010000 y generar nuestro propio firmware Klipper.

En ese caso necesitaremos quemar el bootloader primero. Podemos encontrar el bootloader de nuestra Fysetc en el directorio bootloader del repositorio de github siguiendo las instrucciones indicadas [aqui](https://github.com/FYSETC/FYSETC-SPIDER/tree/main/bootloader). Una vez realizados estos pasos podemos realizar el proceso normal para subir el firmware por SD.

