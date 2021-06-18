# Configuración de una dirección IP estática

En ocasiones en lugar de que se nos asigne una dirección IP dinámica que cambie nos interesa tener una fija para siempre poder acceder sin necesidad de buscar la asignada cada vez. Con el siguiente procedimiento podrás realizar este paso.

Para comenzar a configurar una dirección IP estática en nuestra Raspberry Pi, primero necesitaremos recuperar información sobre nuestra configuración de red actual.  


* Primero recuperemos el enrutador definido actualmente para tu red ejecutando el siguiente comando. **`ip r | grep predeterminado`** Al usar este comando, deberías obtener un resultado similar al que tenemos a continuación. **`default via 192.168.0.1 dev eth0 proto dhcp src 192.168.0.159 métrica 202`** Anota la primera IP mencionada en esta cadena. Por ejemplo, la IP que vamos a tomar nota de este comando es "192.168.0.1". Esta dirección IP es la dirección actual del enrutador/gateway.
* A continuación, recuperemos también el servidor DNS actual. Podemos hacer esto abriendo el archivo de configuración “resolv.conf” ejecutando el siguiente comando. **`sudo nano /etc/resolv.conf`** Desde este comando, debería ver las líneas de texto a continuación. **`# Generado por resolvconf nameserver 192.168.0.1`** Toma nota de la IP junto a "nameserver". Esto definirá el servidor de nombres \(DNS\) en nuestros próximos pasos.
* Ahora que hemos recuperado nuestra IP actual del “enrutador” y la IP del servidor de nombres, podemos proceder a modificar el archivo de configuración “dhcpcd.conf” ejecutando el siguiente comando.

  **`sudo nano /etc/dhcpcd.conf`**  
  Este archivo de configuración nos permite modificar la forma en que Raspberry Pi maneja la red.

* Dentro de este archivo, pon las siguientes líneas al final.  
  **`interface <RED>`**

  **`static ip_address=<STATICIP>/24`**

  **`static routers=<ROUTERIP>`**

  **`static domain_name_servers=<DNSIP>`**

  * Primero, debes decidir si deseas configurar la IP estática para su conector “eth0” \(Ethernet\) o su conexión “wlan0” \(WiFi\). Decide cuál deseas y reemplaza “&lt;RED&gt;” por él.
  * Asegúrate de reemplazar "&lt;STATICIP&gt;" con la dirección IP que desea asignar a su Raspberry Pi. Asegúrese de que esta no sea una IP que pueda conectarse fácilmente a otro dispositivo en tu red o si esta dentro del rango DHCP \(asignacion automática de IP por parte del router\) asetúrate que bloqueas la IP en el.
  * Reemplaza "&lt;ROUTERIP&gt;" con la dirección IP que recuperaste en el paso 1 de este tutorial.
  * Finalmente, reemplaza “&lt;DNSIP&gt;” con la IP del servidor de nombres de dominio que desea utilizar. Esta es la IP que obtuvo en el paso 2 de este tutorial u otra como Google "8.8.8.8" o "1.1.1.1" de Cloudflare.
  * Ahora guarde el archivo presionando CTRL + X, luego Y seguido de ENTER**.** 

* Ahora que hemos modificado el archivo de configuración DHCP de nuestra Raspberry Pi para que utilicemos una dirección IP estática, debemos continuar y reiniciar la Raspberry Pi.  Reiniciar la Raspberry Pi permitirá que se carguen nuestros cambios de configuración y se eliminen los antiguos.  Al reiniciar, la Raspberry Pi intentará conectarse al enrutador utilizando la dirección IP estática que definimos en nuestro archivo "dhcpd.conf".  Ejecute el siguiente comando para reiniciar su Raspberry Pi.  sudo reiniciar Prueba de la IP estática 1. Una vez que su Raspberry Pi haya terminado de reiniciarse, ahora debería poder conectarse utilizando la dirección IP que especificó.  Si se está conectando localmente y desea verificar la dirección IP estática configurada correctamente, puede hacerlo ejecutando el siguiente comando.  nombre de host -I Desde este comando, ahora debería poder ver su nueva dirección IP estática. Si es la IP que esperaba, entonces ha configurado con éxito una dirección IP estática en su Raspberry Pi.  El uso de una IP estática será útil cuando necesite recordar la IP, como usar FTP o configurarlo para que actúe como NAS.  Espero que este tutorial de IP estática de Raspberry Pi le haya ayudado a lograr su tarea. Si tiene algún comentario sobre este tutorial, no dude en dejar un comentario.

