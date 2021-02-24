# Cambios y comprobaciones en nuestro Marlin

Como hemos comentado es muy importante la configuración de SERIAL y BAUDIOS en Marlin \(configuration.h\), os ponemos como ejemplo algunas configuraciones para placas SKR.

## Configuración SERIAL placas SKR

**SKR 1.3**  
\#define SERIAL\_PORT 0  
\#define SERIAL\_PORT\_2 -1  
  
**SKR 1.4**  
\#define SERIAL\_PORT -1  
\#define SERIAL\_PORT\_2 0  
  
**SKR PRO 1.1**  
\#define SERIAL\_PORT -1  
\#define SERIAL\_PORT\_2 1  
  
**SKR MINI E3 V1.2**  
\#define SERIAL\_PORT 2  
\#define SERIAL\_PORT\_2 -1  
  
**SKR E3 DIP**  
\#define SERIAL\_PORT 2  
\#define SERIAL\_PORT\_2 -1

## Configuración BAUDRATE

En cuanto BAUDRATE podeis elegir entre estos wue son los wue suelen dar mejor resultado:  
\#define BAUDRATE 115200  
O  
\#define BAUDRATE 115200

## **También es aconsejable activar las siguientes funciones en Marlin:**

* \#define EMERGENCY\_PARSER
* \#define HOST\_ACTION\_COMMANDS

