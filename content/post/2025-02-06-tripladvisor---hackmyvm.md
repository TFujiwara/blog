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

![Escaneo con nmap a la máquina virtual del reto](/blog/img/tripl1.png)

Vemos que el escaneo nos devuelve varios puertos abiertos, vamos a ver que nos espera tras el puerto 8080, que como bien nos indica *nmap*, es un servidor Apache que está ejecutandose bajo un sistema operativo Windows. 

Con tan solo acceder mediante navegador al puerto 8080, vemos que nos devuelve una web de lo que parece ser una app de reseñas de viajes. 

![Acceso a la web del reto](/blog/img/tripl2.png)

De forma muy clara, vemos que tras acceder al puerto 8080 con el navegador, nos redigire a la web y en la URL vemos **wordpress** con lo cual, concluimos que es una web realizada con Wordpress para a continuación, aplicarle un escaneo más profundo de posibles vulnerabilidades en sus temas o plugins, con la herramienta *wp-scan*.

![Escaneo de vulnerabilidades con wp-scan](/blog/img/tripl3.png)

Al terminar el escaneo, como se puede comprobar en esta captura, nos devuelve la versión 1.1 de un plugin de edición que detecta mediante el readme, siendo el plugin Site-Editor que el propio readme menciona pero que no ha podido detectar de forma correcta wp-scan.

Con la detección del mismo, en exploit-db nos encontramos que este plugin es vulnerable a LFI (Local File Inclusion): [WordPress Plugin Site Editor 1.1.1 - Local File Inclusion](https://www.exploit-db.com/exploits/44340)

Observando tambien un poco el entorno web, vemos que en nuestro navegador, al haber accedido a la web, nos aparece el favicon de XAMPP, dandonos a entender que la web está montada sobre este programa.

![Favicon de XAMPP](/blog/img/tripl4.png)

Sabiendo esto, creamos una wordlist para usar con *ffuf* y fuzzear la ruta afectada por la vulnerabilidad hasta que nos dé una peticion 200 en un fichero del sistema que podamos ver, que en este caso, es el log de apache. 

![Fuzzeando con FFuF](/blog/img/tripl5.png)

Probamos a acceder al log de apache mediante nuestro navegador para ver si contesta y coincide con lo que nos ha devuelto **ffuf**.

![Resultado del comando enviado](/blog/img/tripl6.png)

Ahora, intentaremos ver si, mediante una peticion CURL con una pequeña webshell en PHP, nos hace caso al pedirle que nos muestre el contenido del directorio, simplemente para verificar si hay opciones deshabilitadas en PHP o se han dejado tal cual por defecto. 

Al ejecutar el comando, la web nos contesta, por lo cual, hemos inyectado el comando con éxito. 

![Mandando una peticion CURL con una webshell](/blog/img/tripl7.png)

A continuación, encontraremos la salida del comando en el fichero log de apache, al final del todo, al consultarlo de nuevo con nuestro navegador. 

![Resultado del comando enviado](/blog/img/tripl8.png)

Como ya hemos visto que el sistema responde a los comandos que se le envían mediante la petición CURL, crearemos un payload en formato Powershell para crear una reverse shell con *msfvenom* que escuche en el puerto 1337 la conexión que nosotros realizaremos mediante la consola de metasploit, para que nos devuelva un meterpreter.

![Creación del payload](/blog/img/tripl9.png)

Una vez que hemos creado el payload, hacemos un *cat* al fichero donde hemos guardado el mismo, podemos observar que el payload ya está listo para ser lanzado, pero aún nos queda una cosa, que es integrarlo con CURL. 

![Creación del payload](/blog/img/tripl10.png)

A continuación, cogeremos la parte encriptada del payload y el comando que llama a Powershell, y los agregaremos a una petición CURL, y dejaremos preparada esta petición en un script de bash para poder ejecutarla de forma comoda. 

![Integración con CURL del Payload](/blog/img/tripl11.png)

Seguidamente entraremos en la consola de metasploit, y desde la misma, configuraremos el uso de un handler, y también configuraremos el uso de un payload de shell inversa mediante el puerto TCP 1337 a un sistema operativo Windows, configurandolo con la IP de nuestra máquina atacante y el puerto designado anteriormente, y ejecutaremos el exploit. 

![Ejecución de la shell inversa](/blog/img/tripl12.png)

Una vez ejecutado, metasploit quedará a la espera de recibir una conexión por el puerto 1337. Ahí es cuando ejecutaremos la peticion CURL que hemos confeccionado anteriormente y, navegando a la misma web donde podemos ver el log de Apache, estableceremos la conexión con la shell inversa, dando como resultado que se nos abra una sesión de meterpreter. 

![Conexión establecida con Meterpreter](/blog/img/tripl13.png)

Una vez tenemos la conexión establecida, veremos que estamos como el usuario **websvc**, y procederemos a buscar la flag de usuario en el directorio del usuario, que en este caso, se encuentra en la carpeta **Desktop** del mismo directorio de usuario. 

![Encontrando la flag de usuario en el sistema](/blog/img/tripl14.png)

Ahora pasaremos a la tarea de vulnerar por completo el servidor y conseguir la flag del usuario **Administrador** o **Administrator** del sistema. Para ello, pasaremos la sesión que tenemos abierta de meterpreter a segundo plano con el comando *background*, y a continuación, buscaremos un exploit compatible para la máquina Windows usando el local exploit suggester de metasploit. 

![Configurando el exploit suggester](/blog/img/tripl15.png)

Como podréis observar, hacemos tambien un listado de las sesiones activas, para asegurarnos el ID de la sesión donde vamos a inyectar el exploit suggester. Como solo tenemos esa sesión, configuramos el exploit suggester para que apunte al ID de esa sesión y luego ejecutamos la tarea.

![Terminando de configurar el exploit suggester y lanzandolo ](/blog/img/tripl16.png)

Una vez ejecutamos el exploit suggester, vemos que nos aparecerán varios exploits a los cuales la máquina es vulnerable, de todos ellos, elegiremos el exploit **ms016_075_reflection_juicy**.

![Buscando el exploit en los resultados del exploit suggester](/blog/img/tripl17.png)

A continuación, le diremos a metasploit que use ese exploit, y lo configuraremos para que se ejecute en la sesión de meterpreter que hemos puesto anteriormente en segundo plano y usando el puerto 9999 para que espere a recibir una conexión entrante por ese mismo puerto. 

![Configurando el exploit](/blog/img/tripl18.png)

Una vez lanzamos el exploit, este nos devolverá de nuevo una shell inversa en una sesion de meterpreter, pero ahora, como el usuario **NT AUTHORITY/SYSTEM**, el usuario de privilegios más altos en Windows y el equivalente a **root** en Linux. 

![Ya somos superusuario de Windows](/blog/img/tripl19.png)

Ahora, solo tendremos que invocar una shell desde el meterpreter para poder desplazarnos libremente por todos los directorios del sistema.

![Invocamos de nuevo la shell](/blog/img/tripl20.png)

Y por ultimo, navegamos hasta el directorio **Desktop** del usuario Administrator y con esto, acabamos la máquina y podemos subir las flags a la plataforma.

![Invocamos de nuevo la shell](/blog/img/tripl21.png)

Con esto, terminamos aquí el Writeup de la máquina Tripladvisor. Espero que os haya gustado y que, si lo habéis usado para resolver la máquina de primeras, lo volvaís a intentar sin mirar este Writeup para afianzar conocimientos, y os animo a seguir practicando con estos retos.
