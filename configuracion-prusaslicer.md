# Configuración PrusaSlicer

ya teniendo la unidad MMU y el extrusor listos, se puede ya imprimir las piezas en multicolor, pero como ya se sabe, la probabilidad de fallo es muy alta, y con la modificación de colocar un _splitter_ ayuda a que se omita el sensor del selector y el propio selector, con lo cual los fallos -en su mayoría- se reducen a la hora de cargar hacia el extrusor, pero para que una introducción sea óptima, las puntas también deben de serlo.

### La elección del conjunto fusor

Ese es un paso fundamental a la hora de utilizar el sistema multimaterial, ya avisado en el anterior artículo y volveremos a recodar.

#### Trianglelab Dragon

El fusor Dragon combina lo más popular del V6 que son sus dimensiones, el acople y las dimensiones del bloque con la destreza tecnológica del SE Mosquito, su barril térmico con un disipador en miniatura de cobre. Es bi-metal, pues su interior es de acero inoxidable, mientras que el exterior es de cobre, [aquí tienes mas información](https://zepyrshut.com/elegir-la-boquilla-y-el-barril-adecuado/).Dragon, de Trianglelab

![](https://zepyrshut.com/wp-content/uploads/2020/12/dragon.jpg)

No he podido localizar los dibujos técnicos del barril térmico de ese fusor. Pero sí que se ha podido realizar las medidas gracias a Fusion 360, que se utilizarán más adelante.

[Dragon](https://es.aliexpress.com/item/4000404170721.html), de Trianglelab en Aliepress, opción _Standard-flow_.

Hay una alternativa llamada [Phaetus](https://www.3djake.es/phaetus/hotend-dragon-standard-flow), que es ligeramente más caro, pero más accesible, desde tiendas europeas. El fabricante es el mismo \(y calidad también\), solo que cada uno pone su sello.

#### E3D V6 de Prusa

Es el conjunto fusor más famoso, extendido y replicado, no sólo su diseño, sino también sus medidas, ya que podemos encontrar con impresoras con otro conjunto fusor pero siempre respetando las medidas de rosca y paso de rosca de todo el conjunto.

Sin embargo, no vale el fusor V6 original a secas ni los clones por muy buena calidad que tengan, ni si quiera los bi-metálicos, pues carecen del diseño especial que tienen los barriles térmicos:Barril térmico de E3DBarril térmico E3D de Prusa

![](https://zepyrshut.com/wp-content/uploads/2020/12/prusahb.jpg)

![](https://zepyrshut.com/wp-content/uploads/2020/12/e3dhb.jpg)

Fijaos las diferencias, aunque las medidas externas sean idénticas, no lo son las internas.

En Prusa, hay un pequeño chaflán que pasa de 2 mm a 2.2 mm, esa parte es crucial para hacer la punta, es donde se hacen los movimientos para redondear el filamento, y una vez terminado, sale.

[E3D V6 Prusa entero](https://www.3djake.es/e3d/prusa-v6-hot-end)  
[Barril solamente](https://www.3djake.es/e3d/prusa-heatbreak)  
[PTFE 1.85 mm](https://imprime3dbcn.com/mecanica/116-ptfe-especifico-para-mmu2s.html)

#### ¿Entonces?

Después de haber experimentado con el V6 de Prusa, probando incluso [SuperSlicer](https://github.com/supermerill/SuperSlicer) y [PrusaSlicer Dribbling](https://github.com/antimix/PrusaSlicer) no se ha obtenido los resultados que esperaba.

El primero tiene opciones de _skinny-dip_, que según Prusa, [no da buenos resultados](https://github.com/prusa3d/PrusaSlicer/issues/2385#issuecomment-499236267), y por ello no lo implementan en la versión final, lo que hace es quemar los pelillos sueltos que van dejando al salir el filamento, mientras que la edición _Dribbling_ hace pausas intencionadas para cambiar de temperaturas y así mejorar las puntas.

Incluso el PTFE 1.85 es necesario que lo tenga y se desgasta muchísimo con el uso, con lo que al final, lo tendrás que cambiar, así empeorando las puntas.

Así que… **Dragon**, esa sería la elección ideal, por los siguientes motivos:

* Estándar de V6 \(medidas, tornillos y acoples\)
* Tecnología de Mosquito \(barril térmico bi-metal, con apenas transferencia de calor\)
* La no necesidad de un PTFE de 1.85 mm
* Funciona con menor esfuerzo y otorga mayor fiabilidad

Es ligeramente más caro que el V6, pero merece la pena.

### Lo importante, las puntas

Para que el sistema MMU funcione correctamente y sin fallos, es crucial que salgan bien las puntas para su salida del fusor y su reentrada. Las puntas deben medir menos de 1.9 mm de diámetro \(el tamaño interno que tiene el barril térmico de Dragon\).

#### Configuración de la impresora

Para ello hay que modificar varios parámetros en el laminador PrusaSlicer, primero cambiamos las propiedades del fusor, ya que los números de referencia son para el V6 y el MMU2S. Y en este caso se está usando Dragon y MMU2+1S.![](https://zepyrshut.com/wp-content/uploads/2020/12/dragonconf.png)Configuración Dragon con _splitter_

* 27 mm es lo que mide el PTFE del extrusor.
* 33 mm es lo que mide el barril térmico.
* 90 mm es lo que mide desde la punta de la boquilla hasta las poleas Bondtech.
* -29 mm significa que el filamento, a la hora de ser cargado, se quedará a 29 mm de llegar a la boquilla, sirve para que no deje bolas de filamentos en la purga a la hora de cargar.

Medidas obtenidas gracias a Fusion 360.

En caso del V6 Prusa, la configuración debe quedar así:![](https://zepyrshut.com/wp-content/uploads/2020/12/v6config.png)

#### Configuración del filamento

La parte crucial e importante. Cabe destacar que es muy importante usar un filamento de una calidad razonable y que todos los colores que se vayan a usar sean capaz de imprimirse a una misma temperatura. 205 grados es mi punto de referencia.

Los valores a modificar está en Avanzado, Parámetros del cambio de herramienta para la impresora de un único extrusor MM.

* **Velocidad de carga al inicio:** el filamento irá a la velocidad determinada cuando está entrando al tubo PTFE, si ves que el tubo hace resistencia o le cuesta entrar, subir la velocidad.
* **Velocidad de carga:** velocidad a la que va cuando está echando el filamento mientras se purga en la torre de limpieza, el valor por defecto está bien. Sin embargo, si se usa un filamento especial como perlado o de madera, sería conveniente bajar la velocidad para reducir la presión. Ejemplos:
  * PLA normal: 10 – 15 mm/s
  * PLA mármol: 5 – 8 mm/s
  * Flexible: 3 mm/s
  * PETG: 10 mm/s
* **Velocidad de descarga al inicio:** es el primer movimiento que después del empuje. Poner un valor reducido hace que la punta salga mediocre o alargada. Mientras que un valor muy alto hace que la punta salga muy redondeada. Buscar un valor intermedio entre 80 a 160 mm/s, según la densidad y dureza del filamento.
* **Velocidad de descarga:** velocidad en la que se descarga el filamento cuando ya va por el PTFE en adelante. El valor por defecto está bien, no influye en nada cambiarlo.
* **Tiempo de carga del filamento:** no tocar.
* **Tiempo de descarga del filamento:** no tocar.
* **Retardo tras la descarga:** ese valor es útil si hay un cambio de temperatura entre un filamento y otro. Para permitir que el fusor tenga la temperatura correcta antes de una nueva carga.
* **Número de movimientos de enfriamiento:** veces que hará el proceso de enfriamiento, son los movimiento que se hacen dentro de los 33 mm que miden el barril térmico. Es una zona fría, en la que se va enfriando, y al mismo tiempo, haciéndose la punta. Crucial.

Si la punta del filamento sale con hilitos, aumentar de 1 a 2, hace que los pequeños hilos se vayan quemándose en los movimientos de enfriamiento. Si el problema persiste, subir a 3, pero implica un aumento considerable de tiempo de impresión.

* **Velocidad del primer movimiento de enfriamiento:** relacionado con los números de movimiento, los movimiento empezará a una velocidad determinada, recomiendo valores reducidos, entre 10 y 15 mm/s.
* **Velocidad del último movimiento de enfriamiento:** también relacionado con lo anterior, los movimiento de enfriamiento terminará a la velocidad determinada, deben ser menores que el primer movimiento, entre 5 y 10 mm/s.
* **Parámetro de empuje:** importantísimo, que nos olvidamos porque es una configuración “complicada”, que en realidad no lo es para nada. Es la principal responsable de los hilillos superlargos que dejan a la hora de extraer el filamento. En inglés es _ramming_, significa que el extrusor embestirá el filamento sobre la boquilla, ¡el famoso _skinny-dip_ hace exactamente lo mismo!, bueno, no exactamente, pero por ahí van los tiros.

### A considerar:

Para analizar y obtener las mejores puntas, lo primero es hacer una serie de comprobaciones:

* Extrusor calibrado, sensor correctamente funcionando. Cuando no hay filamento, dar 0, cuando hay filamento, dar 1, es una obviedad muy grande, pero hay quienes lo pasan por alto.
* _Splitter_ bien impreso, el filamento desliza con suavidad.
* El PTFE del _splitter_ debe deslizar con suavidad, el filamento tiene que estar al ras del final del tubo, insertado en los racores.
* Poleas Bondtech limpias.
* _Idler_ libre y firme, no muy apretado ni muy flojo, tanto del extrusor como la de la unidad MMU.
* PTFE que va en el fusor Dragon debe ser de 1.9 mm, avellanado en la entrada, plano en el extremo que está dentro \(el azul que trae en el kit de la impresora o MMU\), el motivo es porque el barril térmico de Dragon es de 1.9 mm.
* Parámetro del extrusor con los valores correctos en PrusaSlicer \(ver bloque anterior\).

Bloques para probar las puntas y no “tirar” tanto plástico:

[Descargar desde aquí.](https://www.prusaprinters.org/prints/4133-mmu2-test-block/files)

Lo primero es empezar con una base, con la configuración de filamento ya predefinido en PrusaSlicer, que, ojo, específico para MMU \(las configuraciones difieren entre el modo normal y el modo multi\). Se elige Fillamentum PLA @MMU2 y ajustamos a la temperatura, observamos el resultado:

A partir de ahí, se puede ir variando los valores a más o a menos. Pongo una serie de ejemplos útiles:

![](https://zepyrshut.com/wp-content/uploads/2020/12/photo_2020-12-22_03-51-17.jpg)

Ahí se varía la **Velocidad de descarga al inicio**, de izquierda a derecha: 80, 100, 120, 140 y 160. Se observa que a menor velocidad, más alargada, mientras que a mayor velocidad, se hace más redonda, las ideales son la negra y la blanca, entre 100 mm/s y 120 mm/s.

![](https://zepyrshut.com/wp-content/uploads/2020/12/photo_2020-12-22_05-49-54.jpg)

Ahí se varía la **Velocidad del primer movimiento de enfriamiento y del último**, de izquierda a derecha: 3/2, 8/4, 12/8, 16/12, 20/15. Se puede observar que todas las puntas salen bien, si nos ponemos muy exigentes, la mejor es la negra, con velocidades 12/8.

Al tener los demás valores afinados, éstos no tienen mucho impacto, lo que hace es acelerar el proceso de enfriamiento, sobre todo si lo tenemos en 2 o más.

![](https://zepyrshut.com/wp-content/uploads/2020/12/photo_2020-12-22_05-49-55.jpg)

Ahí se varía los **Párametros de empuje**, de izquierda a derecha, en tiempo de empuje total: 1s, 2s, 3s, 4s y 5s. Los demás valores está establecidos en 130% y 120%.

Como se puede apreciar, la calidad varía muchísimo entre uno y otro, aunque la punta inicial sea perfecta, deja una cola bastante larga. En caso de que ocurra, subir o bajar el tiempo de empuje.

![](https://zepyrshut.com/wp-content/uploads/2020/12/photo_2020-12-22_05-49-56.jpg)

De izquierda a derecha, parámetros por defecto de Prusament PLA, Fillamentum PLA y el útlimo, los valores son copiados de esta [guía](https://www.hcd.net/prusa-mmu2s/Prusa-MMU2S-Reliability-Modifications.htm).

Desaconsejo totalmente la confianza ciega que hay en copiar los valores de configuración. Desconfiad incluso de este documento. Espero que sólo sirva para orientar y poder sacar tus propias y únicas conclusiones. Ánimo y suerte.

### Mi configuración:

He dejado lo mejor para el final, os dejo mi configuración de puntas. Solamente es válido para aquellas impresoras con fusor Dragon, PTFE de 1.9 y MMU2+1S \(con _splitter_\). Extrusor de Prusa R6, la que tiene chimenea.

![](https://zepyrshut.com/wp-content/uploads/2020/12/b.png)

![](https://zepyrshut.com/wp-content/uploads/2020/12/a.png)

Filamentos probados y funcionando, todos a 205 grados:

* Fillamentum PLA
  * Metallic Grey
  * Rapunzel Silver
  * Signal Red
  * Sky Blue
  * Traffic Black
  * Turquoise Green
  * Wizard’s Voodoo – Con ese cuidado, ensucia muchísimo las poleas Bondtech, revisar de vez en cuando y limpiar.
* Fervi PLA
  * Blanco luminoso
  * Piel
* Eryone PLA
  * Silk Gold

Filamentos probados, sin éxito:

* Eryone PLA
  * Glow Green
  * Marble
* Extrudr BioFusion
  * Inca Gold
  * Steampunk Copper
  * Quicksilber
* PLA Genérico de mala calidad, cualquiera.

### Fuentes

[https://www.hcd.net/prusa-mmu2s/Prusa-MMU2S-Reliability-Modifications.htm](https://www.hcd.net/prusa-mmu2s/Prusa-MMU2S-Reliability-Modifications.htm)

[https://www.reddit.com/r/prusa3d/comments/c9e501/mk3s\_mmu2s\_problems\_with\_pla\_filament/](https://www.reddit.com/r/prusa3d/comments/c9e501/mk3s_mmu2s_problems_with_pla_filament/)

[Grupo Telegram Vertex 3D](https://t.me/vertex3despanol)

[Grupo Telegram MMU2+1S](https://t.me/joinchat/EQR2RkVbozDUYIRuDF9OsA)

