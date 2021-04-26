---
description: >-
  Una gran mejora para una impresora 3D es la posibilidad de disponer de doble
  motor en el eje Z dando más consistencia a este eje ya que normalmente tiene
  que soportar el peso del cabezal de impresión
---

# Doble Z y autonivelado

Una gran mejora para una impresora 3D es la posibilidad de disponer de doble motor en el eje Z dando más consistencia a este eje ya que normalmente tiene que soportar el peso del cabezal de impresión o la cama en formato CoreXY.

![](https://telegra.ph/file/0415a3d5dd0ee59c944a5.jpg)

Nos vamos a centrar solamente en la parte de configuración ya que la parte mecánica dependerá mucho del tipo y modelo de impresora.

Tenemos las siguientes opciones...

## **Usar los dos motores Z en un mismo driver**.

Esta opción es la normal y más adecuada en el 90% de los casos, para averiguar si estamos en ese 10% tendremos que ver el consumo A \(amperios\) de nuestros motores y la capacidad de nuestros drivers, en este aspecto no hace falta que el driver soporte la suma A de ambos motores pero si que al menos soporte ⅔ de esa suma.  
Para la conexión de ambos motores dependiendo de tu placa disponemos de dos opciones:

### **La placa dispone de doble conector para motores en Z**

 Placas SKR 1.4, SKR MINI v2, MINI E3 TURBO... como ejemplo.

![](https://lh5.googleusercontent.com/UH6oNygA0geJYXWpSi7UaNx1cYwVZ85hiGwUYR1rIKQE6aglcV_YsWsxEtgawpCmKBaXRs-3HMGImHYxpddUhOWEfgYFCtQXCNvAS5MB31pkI4dAUxuNVUCUDlIBHGt3qgp-j6FH)

### Si no disponemos de doble conector en la placa

 Podemos optar por un módulo splitter existen serie y paralelo

{% embed url="https://s.click.aliexpress.com/e/\_APU7K5" %}

![](https://lh4.googleusercontent.com/98781UiF01nPbb1sSFZOz0sb6LJArEMKpCJhL9YU2-KgsLt1foVyTEewDgfo05gBBF0AvZ9l1njIB5T3c6AfNBJEFiIynhRfT14bAnS4ACY4nkRZrSyl278kBhYqzBslH3QQGHbn)![](https://lh6.googleusercontent.com/5nE_kZQsV62u-OXpuhA3mieGboW4OQV0pfNJRxiSQ6H9MuqRSDax_53U9n1J0n6Jzc_Htf9eTE4COYrEKBd6Q6lBFxzGx6uUkmt-UL6yPycMKeEbnG8V3zA3dIyr7fGc0rEIIjC8)

## Usar drivers separados en Z

En el caso que nuestros motores no sean soportados por un solo driver o que nos interese separarlos por disponer de otras funcionalidades para sincronización automática de nivelado en Z deberemos de usar un puerto de driver libre de nuestra placa para instalar un nuevo driver y configurarlo en Marlin

Instalación del nuevo driver, para este ejemplo utilizaremos una SKR 1.4 y el módulo del driver E1. 

{% hint style="info" %}
**Recordar que los jumpers y/o cualquier otra configuración de la placa para habilitar el driver correctamente se tiene que hacer de igual forma que el resto de drivers.**
{% endhint %}

![](https://lh6.googleusercontent.com/jKoHPy8sStUAsDB2zh_jmYZVLz8vFZK8eXgxvo3Xy0rmY5hs8k8dqNISS8XaP5Qd4_x28DeFelMXyCbXVB7ze3R71N3C4WN7wNWhuspZ5AeI2-mCVLrOcY1fvNkOMDUBA3OHPSEq)

### **Cambios en Marlin en el caso de uso de finales de carrera/endstop** para nivelar doble Z

Habilitaremos en Marlin \(configuration.h\) un nuevo driver en Z2, en este caso como ejemplo un TMC2209 en modo UART.  
Recuerda que en configuration\_adv deberías de configurar del mismo modo que el eje Z original los valores de microsteps y vref \(revisa el apartado de UART del documento para más detalle\)  
**`#define Z2_DRIVER_TYPE TMC2209`**

Habilitaremos también \(configuration.h\) el doble Z  
**`#define Z_DUAL_STEPPER_DRIVERS`**  
en el caso de versiones de Marlin 2.0.6.x o superiror en configuration\_adv.h  
**`#define NUM_Z_STEPPER_DRIVERS 2`**

{% hint style="info" %}
Si los **motores giran en direcciones contrarias girar el cableado para corregirlo.**
{% endhint %}

Podemos realizar el autoalineado de dos formas:

#### Usando finales de carrera fisicos

En el caso que sea necesario habilitar doble final de carrera y en que endstop está ubicado  
**`#define Z_DUAL_ENDSTOPS  
#if ENABLED(Z_DUAL_ENDSTOPS)  
#define Z2_USE_ENDSTOP _XMAX_`**

Para veriones 2.0.6.x o superiores:

**`#define Z_MULTI_ENDSTOPS`**

En este caso recordar que el final de carrera a utilizar se tiene que habilitar también.  
**`#define USE_XMAX_PLUG`**

#### Usando un sensor de nivelación automático \(bltouch, inductivo, etc...\)

Si realizamos el ajuste de doble Z con un ABL, algo más que aconsejable, habilitaremos el comando G34 de la siguiente forma \(recordar que previamente debemos tener configurado un probe y correctamente funcionando \(revisar la parte de nivelación en este documento para más información y habilitado el driver\):  
**`#define Z_STEPPER_AUTO_ALIGN`**

Así como los puntos donde se desea posicionar el cabezal para las medidas del offset.  
**`#define Z_STEPPER_ALIGN_XY { { 50, 150 }, { 210, 150 } }`**

Idealmente para una impresora con formato cartesiano \(motores en ambas columnas Z\) la posicion idónea seria X 1/4 y 3/4 del tamaño y en Y 1/2.

{% hint style="info" %}
**Por defecto Marlin incluye tres puntos de medida, depende del numero de motores estos puntos deben coincidir en número, 2 motores Z 2 puntos de control, 3 motores Z 3 puntos de control...**
{% endhint %}

También es ideal el número de repeticiones de test para un ajuste fino del offset.  
**`#define Z_STEPPER_ALIGN_ITERATIONS 10`**

Y habilitar el leveling después de un G34 tal como hacemos con un G28 con un nivelado de cama G29  
**`#define RESTORE_LEVELING_AFTER_G34`**

## Comprobaciones previas:

* verificar la correcta dirección de los motores en Z
* verificar que los finales de carrera o probe funcionen correctamente M119
* realizar un G34 desde un terminal para verificar que las posiciones de check y el proceso se hace correctamente

