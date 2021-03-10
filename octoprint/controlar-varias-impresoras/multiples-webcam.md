---
description: OctoPrint 0.18.0 Raspberry Pi
---

# Multiples Webcam

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam00.png)

Verificar la versión, al final de la página de octoprint verás que versión tienes, esta guía es válida para la versión 0.18.0.

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam01.png)

Primero entramos por SSH a octoprint, por ejemplo desde Windows con la aplicación PuTTy, introducimos la IP de la raspberry, marcamos SSH y finalmente a Open.![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam02.png)

Duplicamos este fichero:

```text
sudo cp /root/bin/webcamd  /root/bin/webcamd2
```

Editamos el primer fichero:

```text
sudo nano /root/bin/webcamd
```

Solo editaremos estos dos parámetros para la pi cam:

```text
camera="raspi"
camera_raspi_options="-x 1280 -y 720 -fps 15 -q 75"
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam03.png)

Podéis poner la configuración que prefiráis.

Guardamos con CRTL+X, luego Y \(yes\), finalmente ENTER.

Vamos a configurar la webcam por usb, en este caso será un Logitech C270.

Antes que nada, tenemos que detectar el puerto del video de la webcam. Con este comando nos saldrá lo que vemos en la imagen, en este caso donde vemos UVC Camera es la camera sub, donde encontramos el video en /dev/video0, apuntarlo porque se usará más adelante.

```text
v4l2-ctl --list-devices
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam04.png)

Aparte también se puede renombrar el puerto usb, al final del tutorial estará explicado.

Entramos el fichero duplicado de la segunda webcam:

```text
sudo nano /root/bin/webcamd2
```

En este caso editaremos estas líneas, y las dejaremos como se indica aquí, añadiendo el puerto 8081, para la segunda camera.

```text
camera="usb"
camera_usb_options="-d /dev/video0 -r 1280x720 -f 15"
camera_http_options="-p 8081 -n --listen 127.0.0.1"
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam05.png)

Sin salir del fichero, vamos bajando y buscamos el apartado \# add video device into options. Donde eliminaremos la parte `-d /dev/$device` quedando así:

```text
    # add video device into options
    options="$options"
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam06.png)

Guardamos y salimos de este fichero.

Duplicamos este segundo fichero:

```text
sudo cp /etc/systemd/system/webcamd.service /etc/systemd/system/webcamd2.service
```

Abrimos el fichero duplicado:

```text
sudo nano /etc/systemd/system/webcamd2.service
```

Editamos estos parámetros:

```text
Description=the OctoPi webcam2 daemon with the user specified config

StandardOutput=append:/var/log/webcamd2.log
StandardError=append:/var/log/webcamd2.log
ExecStart=/root/bin/webcamd2
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam07.png)

Activamos porque se inicie el servicio de la segunda webcam al arrancar la raspberry.

```text
sudo systemctl enable webcamd2
```

Ahora editaremos el archivo haproxy para añadir la webcam 2.

```text
sudo nano /etc/haproxy/haproxy.cfg
```

Hay dos tipos de configuración:

En caso de tener 2 instancias de octoprint, dejar el apartado de `frontend public2`, así:

```text
frontend public2
        bind :::81 v4v6
        bind :::444 v4v6 ssl crt /etc/ssl/snakeoil.pem
        option forwardfor except 127.0.0.1
        use_backend webcam2 if { path_beg /webcam/ }
        default_backend octoprint2

backend octoprint2
        acl needs_scheme req.hdr_cnt(X-Scheme) eq 0

        reqrep ^([^\ :]*)\ /(.*) \1\ /\2
        reqadd X-Scheme:\ https if needs_scheme { ssl_fc }
        reqadd X-Scheme:\ http if needs_scheme !{ ssl_fc }
        option forwardfor
        server octoprint2 127.0.0.1:5001
        errorfile 503 /etc/haproxy/errors/503-no-octoprint.http

backend webcam2
        reqrep ^([^\ :]*)\ /webcam/(.*)     \1\ /\2
        server webcam2  127.0.0.1:8081
        errorfile 503 /etc/haproxy/errors/503-no-webcam.http
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam08.png)

En el caso que solo tengáis una instancia de octoprint y 2 cameras, hay que añadir en `frontend public` la nueva línea de la webcam.

```text
frontend public
        bind :::80 v4v6
        bind :::443 v4v6 ssl crt /etc/ssl/snakeoil.pem
        option forwardfor except 127.0.0.1
        use_backend webcam if { path_beg /webcam/ }
        use_backend webcam2 if { path_beg /webcam2/ }
        use_backend webcam_hls if { path_beg /hls/ }
        use_backend webcam_hls if { path_beg /jpeg/ }
        default_backend octoprint

.... añadir al final ....

backend webcam2
        reqrep ^([^\ :]*)\ /webcam2/(.*)     \1\ /\2
        server webcam2  127.0.0.1:8081
        errorfile 503 /etc/haproxy/errors/503-no-webcam.http
```

Guardamos el fichero y comprobamos que todo esté correcto con:

```text
sudo service haproxy restart
```

Si no da ningún error es que todo es correcto.

En el caso que os salgo un error como este, volver a editar haproxy, revisad bien el fichero y que no este algún valor mal introducido.

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam09.png)

Reiniciamos la raspberry con:

```text
sudo reboot now
```

Una vez reiniciado octoprint, podremos verificar fácilmente si las cámaras funcionan, entrando en estos enlaces:

Camera pi cam:

```text
http://octopi.local/webcam/
http://octopi.local:8080
http://192.168.X.XXX/webcam/
http://192.168.X.XXX:8080
```

Segunda webcam con multi-instancia:

```text
http://octopi.local:81/webcam/
http://192.168.X.XXX:81/webcam/
```

Segunda webcam sin multi-instancia:

```text
http://octopi.local/webcam2/
http://octopi.local:8081
http://192.168.X.XXX/webcam2/
http://192.168.X.XXX:8081
```

Finalmente solo queda configurar la segunda webcam en la segunda instancia de octoprint.

Vamos a `Settings` –&gt; `Webcam & Timelapse`

Activamos `Enable webcam support`

En la casilla Stream URL: `/webcam/?action=stream`

Probamos con el botón `test`.

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam10.png)

Activamos la opción de `Timelapse Recordings`, luego añadimos estos valores en:

Snapshot URL: `http://127.0.0.1:8081/?action=snapshot`

Path to FFMPEG: `/usr/bin/ffmpeg`

Probamos con el botón `test`.

Esto es todo lo necesario para poder tener  múltiples cameras en octoprint 0.18.0  
  


#### Renombrar puerto usb camera USB

Mandamos este comando, con el nombre del puerto asignado a nuestra webcam, recomendable solo conectar a los usb la webcam, para evitar encontrar los valores de otros dispositivos.

```text
udevadm info -q all -n /dev/ttyUSB0 --attribute-walk
```

Saldrán muchos números. Nos desplazamos aproximadamente al tercer parágrafo, nos paramos en el primero mas largo, como en la foto.

Apuntamos estos dos valores:

```text
ATTRS{idVendor}=="" 
ATTRS{idProduct}==""
```

En el caso que las webcams tengan los mismos valores, hay que apuntar un tercer valor y recordar a que puerto se ha conectado esa webcam.

```text
ATTRS{devpath}==""
```

![](https://drababuilding.com/wp-content/uploads/octomulticam/octomulticam11.png)

Editamos este fichero, si es la primera vez que accedes saldrá en blanco.

```text
sudo nano /etc/udev/rules.d/99-usb.rules
```

Dentro añadimos una línea para cada webcam, rellenándolo con los valores conseguidos anteriormente. En `SYMLINK+`, añadimos el nombre que le daremos al nuevo puerto. Quedando algo así:

```text
SUBSYSTEM=="video4linux", ATTRS{idVendor}=="046d", ATTRS{idProduct}=="0825", ATTRS{devpath}=="1.3", SYMLINK+="WEBCAM"
```

Guardamos, y posteriormente reiniciamos la raspberry.

Vamos a configurar la webcam con su nuevo nombre.

Primera webcam:

```text
sudo nano /root/bin/webcamd
```

Segunda webcam:

```text
sudo nano /root/bin/webcamd2
```

Buscamos esta linea, y las dejaremos como se indica aquí:

```text
camera_usb_options="-d /dev/WEBCAM -r 1280x720 -f 15"
```

Reiniciamos la raspberry, y si todo es correcto ya deberíamos ver correctamente las webcam.  
  
Extraído del manual de nuestro compañero [https://t.me/rogodra](https://t.me/rogodra) que podéis encontrar aquí

{% embed url="https://drababuilding.com/octoprint/octoprint-0-18-raspberry-pi-multiple-printers/" %}

Si tenéis alguna duda poder entrar en la magnífica comunidad de octoprint para telegram:

[https://t.me/Octoprint](https://t.me/Octoprint)

Y quiero dar mención a Chris Riley y Thomas Messmer por sus manuales para la versión de octoprint 0.17.0

[Channel Chis Riley](https://www.youtube.com/watch?v=F0wC4jjF9EQ)

[Web Thomas Messmer](http://thomas-messmer.com/index.php/14-free-knowledge/howtos/79-setting-up-octoprint-for-multiple-printers)

