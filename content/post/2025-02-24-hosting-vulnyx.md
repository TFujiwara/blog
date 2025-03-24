---
title: Hosting (Vulnyx)
description: Writeup del reto Hosting de la plataforma Vulnyx
date: 2025-02-24T16:55:18.865Z
preview: ""
draft: false
tags: []
categories:
    - Writeups
author: Cristian Garcia Quevedo
lastmod: null
---
Hola a todos, hoy os traigo un Writeup del reto Hosting de la plataforma Vulnyx, en la cual nos enfrentamos a un sistema Windows. ¡Veamos el paso por paso hasta conseguir resolver el reto!

En primer lugar, vamos a realizar una enumeracion de los puertos que están abiertos en el servidor mediante la herramienta *nmap*.

![Enumeracion de puertos mediante nmap](/img/cap0.png)

Una vez realizado el escaneo de puertos, vemos que tenemos accesibles los puertos HTTP (80), SMB(445) y WinRM (47005). 

A continuación, con la herramienta *dirbuster*, haremos un ataque de fuerza bruta con una lista de directorios al puerto 80 del servidor, para encontrar el directorio donde está montada la web o si hay algún directorio oculto con información interesante para la resolución del reto. 

![Ataque de fuerza bruta con dirbuster](/img/hosting1.png)

Observamos que *dirbuster* ha encontrado el directorio **speed**, con lo cual, navegaremos hacia ese directorio en nuestro navegador web y veremos que se nos presenta la página web de un hosting de servidores web.

![Página inicial del hosting de servidores web](/img/hosting3.png)

Buscamos en la propia página web, si hay información interesante para poder ayudarnos en la resolución del reto, y vemos a continuación una sección de equipo donde podemos ver algunos de los correos electronicos corporativos de las personas que trabajan allí.

![Sección de equipo de la página web](/img/hosting4.png)

Esto nos puede servir para intentar un ataque por fuerza bruta mediante *crackmapexec* al servicio SMB para saber si la cuenta usa alguna contraseña debil o que haya estado en una brecha de datos reciente. 

A continuación colocamos los usuarios que aparecen en ese mismo lugar en un fichero, y se lo pasamos a *crackmapexec* junto con la lista **rockyou** para atacar mediante fuerza bruta el servicio de SMB. 

En poco tiempo, tendremos un resultado positivo de este ataque por fuerza bruta. 

![Ataque por fuerza bruta a SMB](/img/hosting5.png)

Una vez con la contraseña del usuario *p.smith*, haremos uso de la herramienta *smbmap* para ver en que carpetas compartidas tenemos permisos de lectura y escritura. 

![Vemos que permisos tenemos con SMBMAP](/img/hosting6.png)

Vemos que tenemos permisos de lectura en la carpeta **$IPC**, y con esto, podemos proceder a enumerar usuarios con la herramienta *emnum4linux*, la cual nos devuelve el siguiente listado de usuarios.

![Listado de usuarios de enum4linux](/img/hosting9.png)

Con la descripción que vemos en el usuario m.davis, el cual, suponemos que es la contraseña de su usuario, recopilamos todos los usuarios en una lista y con *crackmapexec*, comprobamos cual de los usuarios tiene permiso para acceder vía WinRM, haciendo uso de la contraseña ya descubierta.

![](/img/hosting7.png)

Nos conectaremos al servicio de WinRM mediante la herramienta *Evil-WinRM* con el usuario que anteriormente hemos descubierto que tiene permisos de conexión con *crackmapexec* y vemos que automaticamente nos devuelve una shell del sistema.

![](/img/hosting10.png)

Una vez aquí, comprobaremos que permisos tiene este usuario con el comando *whoami priv* y vemos que tiene todos los permisos habilitados, sobretodo, uno que nos interesa, que es el de realizar copias de seguridad de archivos y directorios del sistema. 

![](/img/hosting8.png)

Una vez comprobado, copiaremos el fichero SAM y SYSTEM del sistema en una carpeta temporal que crearemos en C:, con la finalidad de descargarlos para luego extraer los hashes.

![](/img/hosting11.png)

Para descargarlos, haremos uso del comando *download* y a continuación, el nombre del fichero que queramos descargar.

![](/img/hosting12.png)

Ahora, volcaremos los hashes con la herramienta *impacket-secretsdump*, y copiaremos el hash del usuario *administrator*, para acceder como administrador global a la máquina y así terminar el desafio.

![](/img/hosting13.png)

Accedemos con *Evil-WinRM* al servidor, haciendo uso de la opción *-H* para indicarle a continuación el hash y... Lo tenemos! Ya hemos conseguido finalizar el reto!

![](/img/hosting14.png)

Con esto, terminamos aquí el Writeup de la máquina Hosting de Vulnyx. Espero que os haya gustado y que, si lo habéis usado para resolver la máquina de primeras, lo volvaís a intentar sin mirar este Writeup para afianzar conocimientos, y os animo a seguir practicando con estos retos.