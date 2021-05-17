---
description: >-
  Entender los comandos G-code te permitirá tener un control avanzado de tu
  impresora!!!
---

# Marlin - Gcodes Básicos

## Que son los códigos G-code

G-code es un lenguaje de programación para CNC para la comunicación y control de movimientos de máquinas.

En esta guía vamos a intentar mostraros los más básicos/comunes.

### Sintaxis

![](../../../.gitbook/assets/image%20%2891%29.png)

Cada código G-code tiene una sintaxis y cada línea de código es un comando.

La primera parte suelen ser G o M que indica el tipo de comando,  seguido de un número que identifica el comando seguido de los parámetros propios que tenga cada comando.

Otro apunte interesante es que podemos añadir un comentario para dar contexto al comando o al código usando ; seguido de la descripción:

```text
G1 X25 Y5 ; Esto es un comentario!
```

## Comandos G-code básicos

### G0 y G1 : Movimientos lineales

![](../../../.gitbook/assets/image%20%2886%29.png)

Estos comandos se usan para indicar movimientos lineales siendo G0 para movimientos sin extrusion y G1 para movimientos de extrusion normalmente.

Por ejemplo:  
G1 X90 Y50 Z0.5 F3000 E1 le dice a la impresora que se mueva \(G1\) 90 mm en el eje X \(X90\) 50mm en el eje Y \(Y50\) a una altura de 0.5 de Z \(Z0.5\) usando un feedrate de 3000mm/min \(F3000\) extruyendo 1mm de material \(E1\).

### G28  y G29 : Auto Home y Nivelación de cama

![](../../../.gitbook/assets/image%20%2889%29.png)

