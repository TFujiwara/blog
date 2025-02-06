---
title: Tripladvisor - HackMyVM
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

En este caso, para este reconocimiento siempre usaremos el mismo comando, el cual, en los siguientes Writeup obviaré y no pondre, puesto que ya lo conocemos; El comando a útilizar en este caso es ```arp-scan --local-net | grep 08:00:27```. 

Hay que recordar que, al estar en entorno local, podemos usar arp-scan sin problema, pero si estamos en entorno corporativo, es mejor utilizar _netdiscover_, con la siguiente sintaxis: ```netdiscover -i (nombre de la interfaz) -r```.

