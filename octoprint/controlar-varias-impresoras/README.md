---
description: OctoPrint 0.18.0 Raspberry Pi
---

# Controlar varias impresoras

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint00.png)

De entrada puede parecer una guía muy larga y difícil, si es larga es porque está todo lo necesario para no tener ningún problema con múltiples instancias en octoprint. Tampoco es difícil, solo hay que ir haciendo los pasos como se muestran.

Temas claros antes de duplicar la instancia:

* Plugins:
  * Quedan compartidos entre todas las instancias instaladas.
  * Los datos cada instancia guarda sus datos, eso no queda compartido.
  * Para actualizar un plugin, solo basta en actualizar en una instancia, para que se muestre actualizado en las otras, o se reinicia la raspberry o las distintas instancias.
  * Recomendado actualizar plugins cuando no se este imprimiendo en ninguna de las impresoras.
  * Si en una instancia no se quiere uno de los plugins, con desactivar para esa instancia es suficiente.
  * Si se borra un plugin, al reiniciar se borrara de las otras instancias.
* Muy importante renombrar los puertos usb, para evitar problemas de conexión.
* Los gcode subidos a cada instancia, solo se mostrarán en esa instancia, eso es bueno, ya que solo veremos los ficheros para esa impresora.

Verificar la versión, al final de la página de octoprint verás que versión tienes, esta guía es válida para la versión 0.18.0.  
  


![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint01.png)

#### **Duplicado de instancias de octorpint**

Primero entramos por SSH a octoprint, por ejemplo desde Windows con la aplicación PuTTy, introducimos la IP de la raspberry, marcamos SSH y finalmente a Open.![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint02.png)

Nos logeamos, si no habéis cambiado el usuario y password, por defecto el usurario es pi y el password es raspberry.

Por Seguridad es recomendable cambiar el password.

Duplicamos el fichero octorpint.service, es posible que os vuelva a pedir de nuevo el password, es decir raspberry o el que tengáis.

Recuerdo que para pegar los comando en PuTTy, se puede hacer pulsando el botón derecho del ratón.

```text
sudo cp /etc/systemd/system/octoprint.service /etc/systemd/system/octoprint2.service
```

Si queréis más impresoras, el proceso es igual, pero cambiado el 2 por 3, 4, etc…

```text
sudo cp /etc/systemd/system/octoprint.service /etc/systemd/system/octoprint3.service

sudo cp /etc/systemd/system/octoprint.service /etc/systemd/system/octoprint4.service
```

Entramos en el nuevo fichero duplicado:

```text
sudo nano /etc/systemd/system/octoprint2.service
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint03.png)

Os saldrá una ventana como esta:

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint04.png)

Editamos estos parámetros, 5001 para la segunda impresora.

```text
Description=The snappy web interface for your 3D printer 2
Environment="PORT=5001"
```

Añadimos al final de ExecStart esto:

```text
--config /home/pi/.octoprint2/config.yaml --basedir /home/pi/.octoprint2
```

Quedando así:

```text
ExecStart=/home/pi/oprint/bin/octoprint serve --host=${HOST} --port=${PORT} --config /home/pi/.octoprint2/config.yaml --basedir /home/pi/.octoprint2
```

Quedando así:

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint05.png)

Guardamos con CRTL+X, luego a Y \(yes\), finalmente a ENTER

De nuevo, para más de dos impresora, el proceso es el mismo, hay que editar los otros ficheros:

Tercera impresora, puerto 5002:

```text
sudo nano /etc/systemd/system/octoprint3.service

Description=The snappy web interface for your 3D printer 3
Environment="PORT=5002"
ExecStart=/home/pi/oprint/bin/octoprint serve --host=${HOST} --port=${PORT} --config /home/pi/.octoprint3/config.yaml --basedir /home/pi/.octoprint3
```

Cuarta Impresora, puerto 5003:

```text
sudo nano /etc/systemd/system/octoprint4.service
Description=The snappy web interface for your 3D printer 4
Environment="PORT=5003"
ExecStart=/home/pi/oprint/bin/octoprint serve --host=${HOST} --port=${PORT} --config /home/pi/.octoprint4/config.yaml --basedir /home/pi/.octoprint4
```

Activamos el inicio automático al iniciar la raspberry para estas segundas instancias:

```text
sudo systemctl enable octoprint2
sudo systemctl enable octoprint3
sudo systemctl enable octoprint4
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint06.png)

Si sale como la imagen es que ya han quedado activadas correctamente para el inicio.

Pasamos a editar el haproxy.cfg, abriéndolo con:

```text
sudo nano /etc/haproxy/haproxy.cfg
```

Crearemos un nuevo frontend para cada impresora, esto tiene como ventaja poder tener una cámara para cada una de las impresoras, y la gestión de multi-instancia, desde mi punto de vista, es mejor.

Con esto accederemos a la segunda impresora desde el puerto 81, tercera 82 etc. Mencionar que la primera se accede desde el puerto 80, que no es necesario introducir en el navegador.

Vamos al final fichero y pegamos esto para la segunda impresora:

```text
frontend public2
        bind :::81 v4v6
        bind :::444 v4v6 ssl crt /etc/ssl/snakeoil.pem
        option forwardfor except 127.0.0.1
        default_backend octoprint2

backend octoprint2
        acl needs_scheme req.hdr_cnt(X-Scheme) eq 0

        reqrep ^([^\ :]*)\ /(.*) \1\ /\2
        reqadd X-Scheme:\ https if needs_scheme { ssl_fc }
        reqadd X-Scheme:\ http if needs_scheme !{ ssl_fc }
        option forwardfor
        server octoprint2 127.0.0.1:5001
        errorfile 503 /etc/haproxy/errors/503-no-octoprint.http
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint07.png)

Repetimos lo mismo para las otras instancias:

```text
frontend public3
        bind :::82 v4v6
        bind :::445 v4v6 ssl crt /etc/ssl/snakeoil.pem
        option forwardfor except 127.0.0.1
        default_backend octoprint3

backend octoprint3
        acl needs_scheme req.hdr_cnt(X-Scheme) eq 0

        reqrep ^([^\ :]*)\ /(.*) \1\ /\2
        reqadd X-Scheme:\ https if needs_scheme { ssl_fc }
        reqadd X-Scheme:\ http if needs_scheme !{ ssl_fc }
        option forwardfor
        server octoprint3 127.0.0.1:5002
        errorfile 503 /etc/haproxy/errors/503-no-octoprint.http


frontend public4
        bind :::83 v4v6
        bind :::446 v4v6 ssl crt /etc/ssl/snakeoil.pem
        option forwardfor except 127.0.0.1
        default_backend octoprint4

backend octoprint4
        acl needs_scheme req.hdr_cnt(X-Scheme) eq 0

        reqrep ^([^\ :]*)\ /(.*) \1\ /\2
        reqadd X-Scheme:\ https if needs_scheme { ssl_fc }
        reqadd X-Scheme:\ http if needs_scheme !{ ssl_fc }
        option forwardfor
        server octoprint4 127.0.0.1:5003
        errorfile 503 /etc/haproxy/errors/503-no-octoprint.http
```

Guardamos el fichero y comprobamos que todo este correcto con:

```text
sudo service haproxy restart
```

Si no da ningún error es que todo es correcto.

En el caso que os salga un error como este, debes volver a editar haproxy, revisando bien el fichero y que no haya algún valor mal introducido.

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint08.png)

Reiniciamos la raspberry con:

```text
sudo reboot now
```

Una vez reiniciada ya tendremos acceso a las nuevas instacias, las direcciones serian estas:

```text
http://192.168.X.XX (pimera)
http://192.168.X.XX:81 (segunda)
http://192.168.X.XX:82 (tercera)
http://192.168.X.XX:83 (cuarta)
```

o

```text
http://octopi.local (pimera)
http://octopi.local:81 (segunda)
http://octopi.local:82 (tercera)
http://octopi.local:83 (cuarta)
```

Vamos a configurar las nuevas instancias:

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint09.png)

Importante, en la sección de Server Commands, hay que poner:

```text
sudo service octoprint2 restart
sudo shutdown -r now
sudo shutdown -h now
```

Para la tercera y cuarta

```text
sudo service octoprint3 restart
sudo service octoprint4 restart
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint10.png)

Ahora, para distinguir cada impresora y evitar confusiones, es recomendable cambiar el nombre de cada una de ellas y su color.  
  


![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint11.png)

#### **Renombrado de los puertos USB**

Finalmente, y muy pero que muy importante, es renombrar los puertos. Esto es muy importante, ya que al abrir la raspberry puede que cambie el nombre del puerto, y no conectes a la impresora correcta. Pero aun puede ser peor, como que en otra instancia conectes a otra impresora que esta imprimiendo, provocando el reinicio de esa placa y adiós a esa impresión.

1. Conectamos solo una de las impresoras, y miramos, desde octorpint, cual es el nombre de Serial Port que le asigna la raspberry:

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint12.png)

En este caso está asignada a `/dev/ttyUSB0`, el nombre puede ser diferente, por ejemplo, `ttyAMC1`.

Si las impresoras no tienen la misma placa, da igual a que puerto usb de la raspberry conectes la impresora, en el caso de tener la misma placa hay que acordarse a que puerto as conectado cada impresora.

Mandamos este comando, con el nombre del puerto asignado a nuestra impresora.

```text
udevadm info -q all -n /dev/ttyUSB0 --attribute-walk
```

Saldrán muchos números. Nos desplazamos aproximadamente al tercer parágrafo, nos paramos en el primero mas largo, como en la foto.

Apuntamos estos dos valores:

```text
ATTRS{idVendor}=="" 
ATTRS{idProduct}==""
```

En el caso que las dos impresores sean iguales, o tengan la misma placa, estos valores saldrán iguales, hay que apuntar un tercer valor.

```text
ATTRS{devpath}==""
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint13.png)

Una vez apuntados estos valores, desconectamos del usb de la impresora y conectamos la otra, volvemos a octoprint para saber que nombre le da octoprint al puerto usb a la nueva impresora, puede que sea igual que la otra impresora.

¡OJO! Si tus impresoras son iguales o tienen la misma placa, tienes que conectar a otro puerto usb de la raspberry, y acordarte a que puerto usb as conectado cada impresora, sino conectara a la que no toca.

Repetimos el proceso para todas la impresoras. Al final conseguiremos los valores de idVendor, idProduct, y en caso necesario, el devpath, para cada una de las impresoras.

Con esto terminamos con PuTTY, editamos este fichero, si es la primera vez que accedes saldrá en blanco.

```text
sudo nano /etc/udev/rules.d/99-usb.rules
```

Dentro añadimos una línea para cada impresora, rellenándolo con los valores conseguidos anteriormente. En `SYMLINK+`, añadimos el nombre que le daremos al nuevo puerto. Quedando algo así:

```text
SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="ARTIX1"
SUBSYSTEM=="tty", ATTRS{idVendor}=="2c99", ATTRS{idProduct}=="0002", SYMLINK+="PMINI"
```

En el caso de impresoras iguales o con la misma placa, hay que añadir el tercer parámetro, quedando así:

```text
SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ATTRS{devpath}=="1.1", SYMLINK+="ARTIX1"
SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", ATTRS{devpath}=="1.3", SYMLINK+="PMINI"
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint14.png)

Guardamos, y posteriormente reiniciamos la raspberry.

Con todas las impresoras conectadas, podemos comprobar si los nombres asignados son correctos, podemos mandar este comando por SSH.

```text
ls -l /dev
```

Ahora vamos a una de las instancias de octoprint. Por ejemplo, en este caso, la de la Prusa.

Vamos a Settings – Serial Connection – General.

En Additional serial ports, añadiremos el nuevo nombre del puerto para la Prusa, en este caso /dev/PMINI

Luego en Blacklisted serial ports, pondremos todos los nombres de los puertos que nos aparezcan de mas. Es decir, algo así:

```text
/dev/ttyUSB0
/dev/ttyUSB1
/dev/ttyACM0
```

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint15.png)

Finalmente guardamos, si está todo correcto veremos que ahora solo nos sale el puerto de la Prusa, en esa instancia de octorpint.

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint16.png)

Repetimos el mismo proceso para la Artillery X1, vamos al octoprint para esta impresora y la configuramos igual que la otra, solo que en Additional serial ports, esta vez añadimos el puerto de la Arillery.

![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint17.png)

Si todo es correcto, volveremos a ver que ahora solo nos muestra el puerto de la Artillery.![](https://drababuilding.com/wp-content/uploads/octomultiprint/octomultiprint18.png)

Repetimos este proceso para cada una de las máquinas. De esta forma, no correremos el riesgo de conectar a la impresora que no toca.

Esto es todo lo necesario para poder crear múltiples instancias de octoprint 0.18.0  
  
Extraído del manual de nuestro compañero [https://t.me/rogodra](https://t.me/rogodra) que podéis encontrar aquí

{% embed url="https://drababuilding.com/octoprint/octoprint-0-18-raspberry-pi-multiple-printers/" %}

Si tenéis alguna duda poder entrar en la magnífica comunidad de octoprint para telegram:

[https://t.me/Octoprint](https://t.me/Octoprint)

Y quiero dar mención a Chris Riley y Thomas Messmer por sus manuales para la versión de octoprint 0.17.0

[Channel Chis Riley](https://www.youtube.com/watch?v=-_sM2367Gq8)

[Web Thomas Messmer](http://thomas-messmer.com/index.php/14-free-knowledge/howtos/79-setting-up-octoprint-for-multiple-printers)

