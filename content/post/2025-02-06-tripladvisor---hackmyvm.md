---
title: Tripladvisor (HackMyVM)
description: Writeup del reto Tripladvisor de HackMyVM
date: 2025-02-06T01:19:39.279Z
preview: ""
draft: false
tags: []
categories:
    - Writeups
---
Hola a todos, he decidido empezar la sección de Writeups de retos de ciberseguridad con una máquina virtual procedente de HackMyVM, Tripladvisor. 

Después de descargarnos la máquina virtual de la web, añadirla a VirtualBox y posteriormente arrancar la máquina, procedemos a hacer un reconocimiento inicial donde localizaremos la máquina en nuestra red local, ya que en este tipo de retos, no se nos da la dirección IP de la misma, ya que se deja configurada la tarjeta de red para que un DHCP ya existente le asigne una dirección IP y así poder empezar a trabajar contra esa máquina de forma inmediata. 

En este caso, para este reconocimiento siempre usaremos el mismo comando, el cual, en los siguientes Writeup obviaré y no pondre, puesto que ya lo conocemos;

 El comando a utilizar en este caso es ```arp-scan --local-net | grep 08:00:27```. 

Hay que recordar que, al estar en entorno local, podemos usar arp-scan sin problema, pero si estamos en entorno corporativo, es mejor utilizar _netdiscover_, con la siguiente sintaxis: ```netdiscover -i (nombre de la interfaz) -r```.

Una vez ya descubierta la dirección IP de la máquina, seguidamente ejecutaremos _nmap_ para realizar un escaneo de puertos y ver que nos encontramos en esta máquina.

![Escaneo con nmap a la máquina virtual del reto](/img/tripl1.png)

Vemos que el escaneo nos devuelve varios puertos abiertos, vamos a ver que nos espera tras el puerto 8080, que como bien nos indica *nmap*, es un servidor Apache que está ejecutandose bajo un sistema operativo Windows. 

Con tan solo acceder mediante navegador al puerto 8080, vemos que nos devuelve una web de lo que parece ser una app de reseñas de viajes. 

![Acceso a la web del reto](/img/tripl2.png)

De forma muy clara, vemos que tras acceder al puerto 8080 con el navegador, nos redigire a la web y en la URL vemos **wordpress** con lo cual, concluimos que es una web realizada con Wordpress para a continuación, aplicarle un escaneo más profundo de posibles vulnerabilidades en sus temas o plugins, con la herramienta *wp-scan*.

![Escaneo de vulnerabilidades con wp-scan](/img/tripl3.png)

Al terminar el escaneo, como se puede comprobar en esta captura, nos devuelve la versión 1.1 de un plugin de edición que detecta mediante el readme, siendo el plugin Site-Editor que el propio readme menciona pero que no ha podido detectar de forma correcta wp-scan.

Con la detección del mismo, en exploit-db nos encontramos que este plugin es vulnerable a LFI (Local File Inclusion): [WordPress Plugin Site Editor 1.1.1 - Local File Inclusion](https://www.exploit-db.com/exploits/44340)

Observando tambien un poco el entorno web, vemos que en nuestro navegador, al haber accedido a la web, nos aparece el favicon de XAMPP, dandonos a entender que la web está montada sobre este programa.

![Favicon de XAMPP](/img/tripl4.png)

Sabiendo esto, creamos una wordlist para usar con *ffuf* y fuzzear la ruta afectada por la vulnerabilidad hasta que nos dé una peticion 200 en un fichero del sistema que podamos ver, que en este caso, es el log de apache. 

![Fuzzeando con FFuF](/img/tripl5.png)

Probamos a acceder al log de apache mediante nuestro navegador para ver si contesta y coincide con lo que nos ha devuelto **ffuf**.

![Resultado del comando enviado](/img/tripl6.png)

Ahora, intentaremos ver si, mediante una peticion CURL con una pequeña webshell en PHP, nos hace caso al pedirle que nos muestre el contenido del directorio, simplemente para verificar si hay opciones deshabilitadas en PHP o se han dejado tal cual por defecto. 

Al ejecutar el comando, la web nos contesta, por lo cual, hemos inyectado el comando con éxito. 

![Mandando una peticion CURL con una webshell](/img/tripl7.png)

A continuación, encontraremos la salida del comando en el fichero log de apache, al final del todo, al consultarlo de nuevo con nuestro navegador. 

![Resultado del comando enviado](/img/tripl8.png)