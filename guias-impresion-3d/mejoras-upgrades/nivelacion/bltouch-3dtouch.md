---
description: >-
  Tener instalado un sensor de Nivelación nos permite asegurar unas primeras
  capas perfectas sin tener que estar ajustando manualmente nuestra mecánica.
---

# Bltouch/3DTouch

![](https://lh5.googleusercontent.com/UB7xkAUJMNHPbd81QghwDEZIkbHwfGCuRWWrTixvJnYR5bG-s52njZGCR0H1t28X_AP7fldDC6G8D20znRLpzwJaHSZxQZF4hghsL3hr9d0Fd28kalS5gIsNIsXo6jo00urQja4p)

En este caso usaremos un BLtouch que es un sensor con mucha precisión y polivalente ya que puede trabajar wn cualquier superficie.

Os recordamos que tienes mas guias de ayuda en nuestro bot de Telegram @ThreeDWorkHelpBot

Para más información visita [https://3dwork.io/configurar-bltouch-en-marlin/](https://3dwork.io/configurar-bltouch-en-marlin/)

Podéis encontrar como ejemplo el sensor original de AntcLabs y un clónico 3DTouch:

{% tabs %}
{% tab title="AliExpress" %}
{% embed url="https://s.click.aliexpress.com/e/\_ASO4vz" %}

{% embed url="https://s.click.aliexpress.com/e/\_ATWOTt" %}
{% endtab %}

{% tab title="Amazon" %}
{% embed url="https://amzn.to/3e56DhE" %}

{% embed url="https://amzn.to/2Rg8cAn" %}
{% endtab %}
{% endtabs %}

## Esquemas de conexión

{% tabs %}
{% tab title="SKR 1.3" %}
![](https://lh6.googleusercontent.com/gq2Q81SUbJyT3lt4qXUBBEhxZjzNPtsXGwE0qJj_9yoIqHkgHH0ImEA_o6wWXGKAcUQ6xew4qvwOW7f23GZF6sJK2LaogIHi6p2ucBBBu_81MvbzGCAifsZelTiXuQNnTMjQw2AY)
{% endtab %}

{% tab title="SKR 1.4" %}
Este esquema depende de la versión de Marlin cargada en la impresora, normalmente a partir de la 2.0.7.x se puede usar el puerto dedicado PROBE en la placa si no se deberá utilizar el puerto Z-STOP.

![](https://telegra.ph/file/6c54297e6e5edb52cb861.png)



![](https://lh5.googleusercontent.com/ekyD_A5GbpEMf9xCeeoebOtgNanm5zvrwrMZyJYSCHq4RxghXi49hJC7_tOMDOGmTqSbALRevT1swCRy3Xp5pc9Y79_-CSnkT9Rxzyg1fTn0YnmYa2iMZpLDmrfXxGXwOkQxRsTr)
{% endtab %}

{% tab title="SKR MINI E3 1.2" %}
Usando el conector dedicado al PROBE, ojo depende de version de Marlin o configuraciones especiales.

![Usando el conector dedicado al PROBE, ojo depende de version de Marlin o configuraciones especiales.](https://lh6.googleusercontent.com/cSevTzP07fQJSNtLIncS2m5-sTJSQoV_-I4M6lehwNUhw0QbX7ebOTCbGZZ248XVKEWJhlvEb9Oq0RBoKobiES8UmkZYpWX5mSISs97zjz_50prJ20aG9iOU7m9kZv2tjtlewviO)

![Usando el conector Z- , ojo depende de version de Marlin o configuraciones especiales.](../../../.gitbook/assets/image%20%2837%29.png)
{% endtab %}

{% tab title="SKR MINI E3 V2" %}
![](https://lh5.googleusercontent.com/S7MWMIPbTX8GcGJmKNy4ZU1vzPm_ToYfc99FVobYYgBq_6hgrw07CXVuYJ2UL4jKCezvOZrHvjTc_3aV3YH0xvM6l5UK5Y5a7z6cvfIba8WZIu5LxpQlGVJMp8Lc4P7H3m21jIGo)
{% endtab %}

{% tab title="FYSETC S6" %}
![](../../../.gitbook/assets/image%20%28119%29.png)
{% endtab %}

{% tab title="SUNLU S8" %}
![](https://lh4.googleusercontent.com/W5qBvoYdZE9ByeuaLWWr5RCTDIdOCKqhUZO3P9gNYjWLMt2-WxpyN-9D7wET6hSgshalMO8MOHaj9dXHV9cffFkLqZgOj0cVL9A1-L8nVcxmlU0YOYlvnEmM1IYMGAbx6_SqQWZP)
{% endtab %}

{% tab title="Artillery X1/Genius" %}
Importante actualizar el firmware a la última versión o uno custom y actualizar el firmware del TFT que tenga soporte, este es muy aconsejable https://github.com/wgcv/RAWR-TFT-Firmware-Artillery3D. Tambien puedes ver el siguiente video del proceso https://youtu.be/QY0BPCdgkr4

![](https://telegra.ph/file/4e16399723c1b956acd3e.png)
{% endtab %}
{% endtabs %}

## **Cambios en Marlin**

* En el caso que el sensor de nivelación de cama se encuentre en el conector del final de carreras Z-MIN/Z-STOP dependiendo de la placa: **`#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN`** _****_En el caso que nuestra placa disponga de un conector especial para el endstop del sensor y dependiendo de la versión de Marlin usada \(normalmente 2.0.7.x ya soporta la mayoria de placas actuales\) **`#define USE_PROBE_FOR_Z_HOMING`** Si nuestro fichero pin de nuestra placa no contempla la definicion del PROBE\_PIN deberemos definirla aqui \(XX sera el pin usado\): **`#define Z_MIN_PROBE_PIN XXX`**

![Ejemplo para una SKR 1.4 usando el pin PROBE para Marlin 2.0.5.3 o superior](https://telegra.ph/file/d2448300145e90cb4943c.png)

* Indicamos el tipo de sensor de nivelación **`#define BLTOUCH`**
* Ajustamos los offsets \(localización del sensor de nivelación con respecto a la punta del nozzle\) del sensor de nivelación **`#define NOZZLE_TO_PROBE_OFFSET…`**

> Es muy importante una vez subidos los cambios veríficar que los offsets esten correctamente cargados en la EEPROM. Esto lo podemos verificar usando el comando **M503** desde Pronterface/Octoprint o desde la propia pantalla \(segun version y opciones habilitadas en Marlin\) en Configuración/Avanzado/Probe Offsets.   
>
>
> En el caso que no estén cargados correctamente esros valores podemos refrescar los datos desde Pronterface/Octoprint usando **M502** **M500** o desde la pantalla de la impresora **Configuración Reset Load Save Eeprom**.
>
> Si deseamos **cambiarlos sin tener que compilar Marlin podemos desde Pronterface/Octoprint** usar el comando **M851** y despues un **M500** \(guardarlos en EEPROM\) y un **M503** para verificar que se guardaron correctamente. Por ejemplo para un desfase X de -1.7 e Y -1.3 de nuestro PROBE usariamos el siguiente comando
>
> ```text
> M851 X-1.70 Y-1.30
> ```

### Valores offsets PROBE dependiendo de ubicación del mismo

![Que son los offsets en X e Y](https://lh6.googleusercontent.com/2pICfeL85Bf_94PrItyrp_2dNxat8Q72dytc1EBN1C1E28yD6L-7Zm5Gf3Rqnf1Cw9tTPwhQwIwRNwRdfEREayv3JCDIX7rRt-iBy1TjLkoYApA9GRefn9rr7_pnfQ0za2IheZdA)

![Que es el offset en Z](https://lh5.googleusercontent.com/yEia6h8u7jPBSNZcz3pTZ-gnSzN0y2ZXjYT6giDQI2btWuixphnl2bFLBOWnfJ_zPvZxlsMJeAc0FIw5ks9IZkDfsdnrbiWVVNE_0oB36WjFwTVSY0WWj_F6fkg7ZYMECAR1j7rP)

![](https://lh3.googleusercontent.com/LJVKD2x3FXgO03Pb9lZPsqnFR0J55nJIRInl-DhLDFLwkQ6nGK82BTbbNAY3KkeKR1EBlVMzHT2lOkQ1ZxFXA0yS3KEvrPe9_HEjCA4Vf86E_dU6zxTLSzX91XIYGu1vR2NI_gkv)

![](https://lh6.googleusercontent.com/MIzOshOTC8vslx7nFkpDWoTm5BKDquaQ_hJwKyiYPGruNr0namQnSjOnOyM8Je0da4-NwVCrvlhtlb4eoG0aHlpLdhnQwWLP0tRkS8zFHY32IVg6mupLJPvBN9IkwKY_fgUOdDQO)

* Ajustamos el tipo de nivelación automática, inicialmente durante la instalación del sensor de nivelación se aconseja usar BILINEAR pero el aconsejable para dejar en nuestra impresora es UBL que se puede encontrar en esta misma guía el proceso de configuración **`#define AUTO_BED_LEVELING_BILINEAR`**
* En el caso de tener cama de cristal con grapas/clips ajustamos el margen de seguridad para que el sensor no toque con las grapas/clips **`#define MIN_PROBE_EDGE 20 // En este caso decimos 20mm de seguridad`** Para versiones 2.0.6 de Marlin o superiores: **`#define PROBING_MARGIN 20 // En este caso decimos 20mm de seguridad`**
* Para una mayor resolución en las medidas del sensor podemos habilitar que realice diferentes medidas en un mismo punto **`#define MULTIPLE_PROBING 3 // Realizar tres medidas en cada punto`**
* Con la siguiente linea permitiremos poder bajar el eje Z por debajo de 0 algo muy util para poder encontrar el ZOffset pero tambien peligroso ya que no tendremos limites de final de carrera en este eje en movimientos manuales **`#define MIN_SOFTWARE_ENDSTOP_Z // Permite mover el nozzle por debajo de 0 en Z`**
* Para sensores tipo BlTouch es necesario invertir el funcionamiento de final de carrera **`#define Z_MIN_PROBE_ENDSTOP_INVERTING true`**
* Por seguridad y teniendo en cuenta la localización del sensor es aconsejable que el homing en Z se realice en el medio de la cama **`#define Z_SAFE_HOMING`**
* Es importante habilitar esta linea dado que un G28 desactiva el autonivelado, de esta forma no lo hará **`#define RESTORE_LEVELING_AFTER_G28`**
* Podemos habilitar el comando G26 que nos permite lanzar un test de nivelación directamente sin necesidad de generar ninguno desde nuestro slicer, algo muy útil para un test rápido. Dentro de las opciones del G26 podrás ajustar los parámetros básicos de impresión **`#define G26_MESH_VALIDATION`**
* Es muy útil y necesario habilitar las opciones de nivelación en el LCD **`#define LCD_BED_LEVELING`**
* Seleccionar el método de nivelado a usar, descomentar una de las líneas solamente, en este mismo documento está descrito como realizar el tipo UBL aconsejable para camas de gran tamaño o con problemas: **`//#define AUTO_BED_LEVELING_3POINT //#define AUTO_BED_LEVELING_LINEAR //#define AUTO_BED_LEVELING_BILINEAR // Aconsejable camas menores 235x235 //#define AUTO_BED_LEVELING_UBL // Aconsejable resto de tamaños o con problemas nivelacion //#define MESH_BED_LEVELING`**
* Otro valor interesante es el de ajustar hasta que punto de la impresión se realizarán las correcciones, por defecto Marlin aplica estas correcciones en los primeros 10mm pero dependiendo de las necesidades se puede ajustar asegurando que habilitamos **`#define ENABLE_LEVELING_FADE_HEIGHT`**

![](https://lh3.googleusercontent.com/4gh4NwC0kjGEfpy7iiS9xGEaw8IMMe2kt8OvGRVn5lwH0JM-rWEmFnHI6kY7jUSbpfo4S7trK9iK14NM6DRTs1ysMwH1e5r3G--kzj4V2KMiCUnUKCzArgT5J0-uQpLoOcNQBdP2)

![](https://lh6.googleusercontent.com/k9AzQIyVbu4dEixRRkGHPK8oXlUlGAeaGC9VFd4Fv1tUQCS2Zb2g_Pv0n59xujlY1gp6OVjyjJM7DX9rtSJhmdP7dsvES1l9cZtH9WCgEFo9iiWWEdlNmWrm-VwPTAx9OQy0XUdB)

* Habilitar el homing por eje, muy útil para ver que cada eje funciona correctamente en el proceso de homing **`#define INDIVIDUAL_AXIS_HOMING_MENU`**
* Otro cambio muy interesante e útil es la activación del nivelado mediante sensor/probe en el asistente de nivelación de cama de 4 esquinas que nos ayudará a dejar nuestra cama de una forma sencilla y fiable lo más recta posible **`#define LEVEL_BED_CORNERS ... #define LEVEL_CORNERS_USE_PROBE`**

### **Otros cambios en configuration\_adv.h**

* Es aconsejable aumentar el tiempo de respuesta para evitar problemas de comunicaciones **`#define BLTOUCH_DELAY 500`**
* Otra funcion interesante a activar para prevenir problemas de comunicación en el caso de cables no apantallados o muy largos es forzar el modo SW que permite el enviar pulsos más largos mejorando ruido en las señales **`#define BLTOUCH_FORCE_SW_MODE`**
* Es aconsejable activar reintentos en el proceso de generacion de malla G29 para prevenir problemas durante el proceso \(**esta función puede no ser compatible con todos los sistemas de nivelación como por ejemplo UBL**\) **`# define G29_RETRY_AND_RECOVER`**

### **Activar Babystepping**

Esta funcionalidad es muy útil para un ajuste fino durante la impresión de la altura del eje Z  
En configuration\_adv:  
**`#define BABYSTEPPING  
#define DOUBLECLICK_FOR_Z_BABYSTEPPING  
#define BABYSTEP_DISPLAY_TOTAL`**

## **Comprobaciones previas**

* **Comprobación del sensor**:
  * **Revisar la parte servo**, la parte servo funciona con el grupo de tres cables \(rojo/amarillo/negro\). Para comprobar si funciona correctamente lo aconsejable es desde la pantalla ir a Configuración donde encontraremos un menu para gestionar el bltouch, en este caso tenemos que comprobar que podamos desplegar y recoger el pin. Si esto no funciona deberemos revisar el cableado o en el caso de placas antiguas la definición del SERVO en Marlin. Tambien se puede hacer esta parte con gcode desde un terminal: **`M280 P0 S10 ; despliega el pin M280 P0 S90 ; recoge el pin M280 P0 S120 ; Autotest – despliega y recoge el pin hasta que ponemos el dedo o lanzamos el comando posterior M280 P0 S160 ; Elimina estado ALARMA`**
  * **Comprobación de la parte endstop**, la parte endstop funciona como cualquier endstop y esta gestionado por el par de cables negro y blanco que normalmente colocamos en el puerto PROBE o Z-STOP/Z-MIN de la placa. Dependiendo de las versiones de Marlin debera colocarse en uno o en otro si esta soportada la placa o no completamente. Para comprobar que funcione correctamente y tal como hicimos en el paso anterior mediante el menu o comandos desplegamos y recogemos el pin y comprobamos con M119 que el enstop cambia de TRIGERED \(pin recogido\) a OPEN \(pin desplegado\) correctamente.
* **Activar Modo Debug nivelado** _Cambios en Marlin \(configuration.h\):_ `#define DEBUG_LEVELING_FEATURE // Permite obtener información detallada en caso de problemas` _Comandos gcode para habilitar/deshabilitar el modo Debug:_ M111 S38 ; LEVELING, ERRORS, INFO M111 S0 ; Disable debug
* **Activar Test de precisión** Permite habilitar un menú dentro del LCD para realizar tests de repetibilidad y mostrar el rango de precisión del sensor: \#define Z\_MIN\_PROBE\_REPEATABILITY\_TEST
* **Verificar ajuste del pin para errores aleatorios en lecturas** Se pueden dar dos casos que el pin este demasiado junto al servo que imanta el mismo y evite que caiga o que el iman del pin no tenga la suficiente tracción por el estado de su imán para que el servo lo pueda imantar y retraer. En estos casos mediante el tornillo central podremos ajustar ambos problemas en la medida de lo posible. Puedes ver el siguiente video donde puedes ver como es el proceso

{% embed url="https://youtu.be/wq5QmN-MIJA" %}

Por otro lado en el caso que el pin este totalmente sin iman puedes extraerlo y dejarlo 24h junto con otro imán para que recupere sus propiedades... en caso que ninguna de estas soluciones funcione puedes solicitar un repuesto del pin o del propio sensor.

## **Ajuste del Z-Offset - WIZARD**

Desde la version 2.0.7.2 Marlin incluye un nuevo asistente para encontrar el valor Z Offset de una forma sencilla. Para activarlo iremos a configuration\_adv y habilitaremos:

**`#define PROBE_OFFSET_WIZARD`**

Con esto dispondremos de un nuevo menu en Configuración/Avanzado/Probe Offsets

{% embed url="https://www.youtube.com/watch?v=2gRfU26aTDs" %}

## **Ajuste del Z-Offset - LCD**

* Realizaremos un Auto Home

![](https://lh4.googleusercontent.com/O3_2XDPXKbzpRQQqKDeGKUMzsofDmuDehM0nLqk8-ZGGHqT60aZUAjyVo_kxb89AnmyN5iTYRvd1otIS10OsUpSRmJm-2ChpUUZhRhMTh0XJb-bLyM4E6r1giNzPXfmRlm8IANzV)

* Para verificar que todos los valores anteriores de offset son correctos nos fijaremos que el home se realiza en el centro de la cama con el sensor de nivelado en el centro de la misma.

{% hint style="info" %}
En el caso que no quede perfectamente centrado:

* verifica que los offsets de tu sensor configurados en**`NOZZLE_TO_PROBE_OFFSET`**

   estan correctamente cargados en la **EEPROM** tal como indicamos en la modificación de Marlin para esa variable

* confirma que tus coordenadas **X0Y0** coincidan exactamente con el **inicio de la cama** y **Xmax Ymax** coinciden exactamente con el **final de la cama**
* asegurate que **no has modificado** las formulas de la función **SAFE\_HOMING** que por defecto usa el tamaño de tu cama en cada eje /2
{% endhint %}

![](https://lh6.googleusercontent.com/2JGxz5P78FqGo0iFFUs09BtgyXgWUhlHh-yVS5Werzzsga1U8mGHuNii8xA3K6SI6f66JJXhpaxM1HuhHRSs-kgqC8WYD3qCa8MX-sX9lLJvx59Z0W1I4iYUo09AHEE0ePTk1iPi)

* Ajustaremos la temperatura del nozzle y cama a las temperaturas normales de impresion y volvemos a realizar un autohome

![](https://lh6.googleusercontent.com/cpq_A4DJ58YLK0bjNoCQqLzuZFwdDUv0wIaM8DkcPUvpFtXNsc42bEYZeU42D_eW1w7W7j0dbPP2q6nShjMeUafBB7WTN4eOQX87hlciqNpect2GVIzTuGS3Nm6Kojn9ZvtQvPVe)

* Vamos al menu de movimiento para el eje Z y seleccionamos movimientos de 0.1mm y bajamos hasta el punto donde mediante un folio notemos que roza suavemente con la punta del nozzle

![](https://lh5.googleusercontent.com/_6u57CffxMb1k9DU-TdoSJsafRxGGQCt9CQB0je6ymHWTz2__NeT0Cjm2XGXbcdM0Cu2KUIcNnIhhy6hmd3rLtof05dOPdsZWgLWztT21Ru0PjL_GzO3dCB74duEBwoy5nV4FrRd)

![](https://lh5.googleusercontent.com/d_xsDJvaS3W9VKBll-iRDd2QYy-4YeEAFcxmM5Vu2FqxgdiVK1WwzOGzg0fzDVWIK5l9tXD9GKAeqqE6t_sgclat5YY7HBfha0AAZjhRoSfKBXl0BBWtCRUZ5gMkpXHTu9wu2CW9)

* En el punto donde encontremos que el folio y la punta del nozzle estan correctos anotaremos el valor que aparece en pantalla para el eje Z el cual debería de ser negativo para sensores bltouch, este valor lo ajustaremos en el menú Z Offset del LCD dentro de Configuracion y dentro de este mismo menu guardaremos en EEPROM y es ideal apuntar este valor en nuestro fichero configuration.h en el \#define NOZZLE\_TO\_PROBE\_OFFSET

![](https://lh5.googleusercontent.com/Cf6SH2dZ4-8IncQ9q6kk_Pl7leRcDP_nXEzFcmGCUvr6BSM4YCJyRcoVJBwqAUJG8g3JwhBenfxuUnSzLBR0VdBfGnAfoLRidNboPx6zeBaWMMxEZGomhwUWx7cWk33qXsaOwgWn)

* Realizaremos un test de nivelado \(G29\) desde la pantalla en Movimiento/Nivelado de Cama/Nivelar Cama

![](https://lh4.googleusercontent.com/4FMD5yyOPAUhdvUyJAho_0LO0UZvDz-8jda1ESop5iwyoMV8PUf0N7G_3gNqxok_dPE16mh12mKZ1ZoP7In7CsHzeNvL51v1FPZHj2QleACDE56gwFMjvT8wH4kp_dPYrBGjNfYS)

![](https://lh4.googleusercontent.com/AU5QmIzuQlype4oIRqcelIsCY9FevslqDZJaPbPmJLU_hZoJeude1epUiMTpx2-L2NNUrsyz7sjPehvXMOT90POW4lVAnJFXebxBQFxsXQoGq0nym-6jISIOYcSohFSEfSplSAFJ)

## **Ajuste del Z-Offset - Pronterface**

Desde un cliente terminal como Pronterface:

1. **Ajuste manual de la cama 4 esquinas y folio tradicional**
2. Realizaremos un **G28**
3. Realizaremos un **pre-calentado de cama y nozzle** que normalmente usemos
4. Realizaremos una malla usando **G29 P1** desde el terminal o desde las herramientas UBL de la pantalla
5. Una vez finalizado podemos usar **G29 T** para visualizar los valores de la malla. Puedes usar estas herramientas web para verlo graficamente: [https://mkdev.co.uk/mesh-visualizer/](https://mkdev.co.uk/mesh-visualizer/) [http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/](http://lokspace.eu/3d-printer-auto-bed-leveling-mesh-visualizer/) [https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/](https://i.chillrain.com/index.php/3d-printer-auto-bed-leveling-mesh-visualizer/)
6. Por la ubicación del sensor de nivelación en muchas ocasiones Marlin no va a llegar a todos los puntos de la malla con lo que en el paso anterior veremos coordenadas sin valores solamente con un ".". Para realizar el relleno de todos los puntos que no pudieron medirse lanzaremos un **G29 P3** \(**G29 P3 T** mejor ya que calcula todos los puntos que falten\) este comando calcula extrapolando valores que si que se pudieron verificar a los que no pudieron medirse.
7. Una vez aparezcan todos los puntos de la malla con un valor, fuera medido o extrapolado por el punto anterior, almacenaremos la malla en la posición 1 de la EEPROM con **G29 S1**
8. Nos aseguramos de que el autonivelado quede activo con **G29 A**
9. Ahora que ya tenemos todos los valores básicos correctos lanzaremos un **M500** para guardarlos en la EEPROM. **IMPORTANTE!!! cada vez que la EEPROM sea borrada o reseteada se tiene que volver a realizar este proceso!!**!

