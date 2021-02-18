# Que es y que necesito para Octoprint?

## Que es Octoprint?

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

