---
description: Controla y supervisa tu impresora remotamente sin complicaciones
---

# Telegram

Una de las formas más sencillas y rápidas de controlar nuestra impresora remotamente es usando el plugin [Telegram para Octoprint](https://github.com/fabianonline/OctoPrint-Telegram). Os indicamos unos pasos rápidos para poder instalarlo y configurarlo correctamente:

**Creación del Bot**

Telegram permite la creación de nuestros propios bots, para ello lo primero es crear uno simplemente desde Telegram abriremos una conversación con [@BotFather](http://telegram.me/botfather) desde tu cliente Telegram y lanzamos el comando /start para comenzar el proceso.

1. Envía /newbot y te solicitará un nombre para tu nuevo bot, para el ejemplo usaremos 3dWork Bot
2. Una vez establecemos el nombre del bot nos solicitará un nombre de usuario para ese bot, es importante que acabe en bot, para nuestro caso usaremos @ThreeDWorkBot \(sin la arroba\)
3. Inmediatamente BotFather nos devolverá un código llamado token que será el que usaremos más tarde. Es importantísimo guardarlo en un sitio seguro.

**Configuración del plugin Telegram en Octoprint**

![](../../.gitbook/assets/image%20%2830%29.png)

Cambiamos a nuestro interfaz web de Octoprint para instalar y configurar el plugin de Telegram.

1. Podemos instalar el plugin desde el Administrador de Complementos \(Plugin Manager\) o instalarlo manualmente desde [aquí](https://github.com/fabianonline/OctoPrint-Telegram/archive/stable.zip)
2. Una vez instalado nos solicitará reiniciar el servidor de Octoprint
3. Una vez reiniciado nos abrirá el asistente de configuración de Telegram, en el caso que no lo haga o no hagamos en el siguiente reinicio en las opciones de configuración de Octoprint puedes encontrar Telegram como plugin y de ahi realizar el proceso.
4. Introduciremos el token que obtuvimos desde BotFather en el punto 3 del paso anterior y probaremos que funcione correctamente.

**Configuración del usuario**

Ahora cambiamos de nuevo a nuestre cliente de Telegram, debemos abrir un chat con nuestro bot... para ello tenemos varias formas de buscarlo ya sea por el buscador de usuarios de Telegram, en la conversación con BotFather o en la captura del paso anterior nos lo indica en Token status.

![](../../.gitbook/assets/image%20%2831%29.png)

Una vez abierto el chat con nuestro bot lanzaremos el comando /start y este nos indicará que tenemos que volver a Octoprint para ajustar los permisos para poder interactuar con el.

Cambiamos de nuevo a Octoprint, en su configuracion, panel izquierdo y abajo veremos Telegram. Dentro de la configuración de Telegram tendremos un apartado de Known Chats, si nuestro chat no aparece pulsaremos el botón Reload y debería aparecernos nuestro usuario.

Ahora daremos al icono del lápiz en la sección Action y permitiremos la ejecución de comandos y notificaciones.

Una vez habilitados volvemos a Known Chats y entraremos pulsando en el icono de Command y Notify para marcar las opciones que más nos interesen.

Ya podremos interactuar desde nuestre cliente Telegram con nuestra impresora!!! Facil verdad?

**Notificaciones**

En la configuración del plugin de Telegram tenemos una sección para configurar las notificaciones \(Messages at...\) estas están escritas en inglés pero podemos cambiarlas por lo que nos interese más.

![](../../.gitbook/assets/image%20%2835%29.png)

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

