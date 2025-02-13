+++
title = "Monta tu servidor con hardware de Aliexpress"
description = "Breve guia de como montar un servidor propio con hardware de Aliexpress y algunos consejos útiles."
date = "2021-10-29"
categories = ["Tutoriales"]
author = "Cristian Garcia Quevedo"
lastmod = "2021-10-29"
+++
### Introducción

A todos los que somos tanto administradores de sistemas como los que nos dedicamos a ciberseguridad, nos gusta tener nuestro propio laboratorio para poder montar nuestros experimentos, investigaciones o cosas a las que le podemos dar un uso cotidiano como un NAS.

Pues bien, en esta guía, os explicaré de forma concisa como montar vuestro propio servidor de torre (Sobremesa) o rack, y ver mas o menos un presupuesto tipico, sin tener que dejarnos medio presupuesto en comprar un servidor HP o de cualquier otra marca, y dejar esos servidores tan caros para el entorno profesional.

Primeramente, pongamonos en situación. Aliexpress, como muchos sabreís, es el gran marketplace chino por excelencia, y aquí se puede comprar de todo lo que puedas imaginar, como decia la canción, que si no la habéis escuchado, os la dejo por aquí: [https://www.youtube.com/watch?v=ME2NrVRXUi8](https://www.youtube.com/watch?v=ME2NrVRXUi8). Increible temazo, por cierto.

Al final de esta guía tendreís las busquedas que he realizado, además de webs de consulta, como una especializada en placas bases chinas, y algún grupo de Telegram.

Más adelante tocaré el tema de confeccionar un PC Gaming Ryzen, comprando uno de los combos Ryzen que venden en Aliexpress y sacarle el mayor partido a la inversion, intentando reducir el coste de un PC Gaming.

### Primeros pasos. Decision y compra de la placa base.

Bueno, siguiendo con el tema, primeramente empezaremos con la base, aquí lo que deberemos buscar principalmente son los combos de placa base, procesador y RAM. Con una simple busqueda en el buscador de la web, ya nos aparecen miles de ellos.

![](/img/alie1.png)

Depende de nuestras necesidades, podemos optar por una placa de un único procesador Xeon, o de una dual. En este caso ficticio de diseño de un presupuesto para un servidor de estar por casa, pero que aguante lo suyo, usaremos una placa base con doble procesador Xeon. El combo que adquiriremos ficticiamente es este, que nos lleva, además de 128GB de RAM y los dos procesadores, un NVME de 512GB listo para montar en la placa y ponerle el sistema operativo que más rabia nos de. **Especial hincapie en que la placa que compremos debe llevar gráfica integrada.**

![](/img/alie2.png)

**NOTA DEL AUTOR: muchas de estas placas chinas tienen chips reinjertados, así que en el caso de necesitar un RAID gestionado por hardware, en el caso de que nuestro servidor sea un NAS o un servidor de virtualizacion o almacenamiento/proceso de datos, nos será necesario una controladora RAID.**

Como he indicado en la nota, nosotros al querer montar el servidor para virtualización, nos tocará buscar una controladora RAID PCI para ponersela a la placa y que esta gestione el enorme RAID de discos SATA, y como podreís observar, hay miles de ellas.

![](/img/alie3.png)

Una vez tengamos el hardware principal decidido, pasaremos a la siguente fase, que es la compra de los discos de almacenamiento.

## Compra de almacenamiento fisico.

Una vez ya hemos tanteado el terreno del hardware principal, ahora nos toca la parte que nos va a costar mas dinero, el soporte de almacenamiento de los datos del servidor.

Aquí, podemos optar por las multiples vías que hay: Amazon, PCComponentes… pero también teneis cosas en Aliexpress, no os vayais a pensar que es lo único que no tienen. Aquí se tiene que valorar el precio y ver si sale rentable o no comprarlo en un sitio u otro. Os recomiendo ir con la decision del tipo de RAID que vais a usar hecha y de si os harán falta discos de repuesto por si las moscas, habiendonos asegurado que la controladora nos permita el cambio en caliente de los discos (Hot Swap).

Yo prefiero coger siempre los Western Digital Red, me gustan por el rendimiento que tienen a la hora de usarse en un servidor. Aqui los tenemos desde 2TB hasta 14TB, a cada cual más caro.

![](/img/alie4.png)

## La caja y su refrigeración

Ahora nos toca elegir una caja acorde para nuestro servidor. En este caso, al ser la placa de factor de forma EATX (Extended ATX), buscaremos una de estas caracteristicas.

En este caso, nuestro cometido es usar una caja de sobremesa, para que el servidor no nos ocupe un sitio exagerado en nuestro despacho, y hemos elegido esta.

![](/img/alie5.png)

Como veréis, en la propia tienda incluso nos recomiendan usar tambien sus fuentes de alimentación modulares, que son altamente recomendables por que podemos sustituir los cables si estos estan dañados. En este caso es ideal, por que a parte de la tipica alimentación de 24 pines, la placa que vamos a usar tambien nos exige una alimentación adicional por CPU, con un conector de 8 pines.

![](/img/alie6.png)

En temas de refrigeración, ya no toco Aliexpress, ya vamos a Amazon, PCComponentes, etc. Yo montaria unos Noctua en esta caja y con eso lo dejaria listo. Pero si os gusta más la refrigeración liquida, esta caja lo soporta.

Y si queremos mirar otro tipo de cajas, algo más serias y listas para enrackar, en Aliexpress hay, no es ningun problema pillarlas aqui. Hay hasta de servidor NAS incluso.

![](/img/alie7.png)

![](/img/alie8.png)

## Conclusiones

Con esto, acabariamos la guia, solo nos quedaria que llegara todo el material a nuestra casa y realizar el montaje del mismo y la instalación del sistema operativo, además de realizar unas pruebas de estres al propio servidor para ver como se porta.

Las pruebas que realicé con el modelo de placa base que compré para renovar el parque informático de la empresa de mi padre, salieron bien. El PC rinde estupendamente y no ha dado ningún tipo de problema, con lo cual recomiendo la compra de este tipo de placas si se quiere modernizar un parque informático antiguo o queremos montar nuestros propios servidores para nuestro homelab.

Como dije al principio, me aventuraré a montar un PC gaming con uno de los combos de Ryzen que venden en Aliexpress y intentaremos reducir costes o ver si la inversión merece la pena y nos ahorra algo de tiempo en el montaje de un equipo solamente para jugar y os haré tambien una guia similar al respecto.

Documentación y biografía
[https://placaschinas.com/](https://placaschinas.com/) - Una web especializada en este tipo de Hardware chinoso y plasticoso.

[https://t.me/Xeonx99](https://t.me/Xeonx99) - Grupo de Telegram donde tendremos más información sobre este tipo de Hardware

[https://bit.ly/3GwZOBL](https://bit.ly/3GwZOBL) - Enlace de busqueda rápida en Aliexpress para encontrar este tipo de Hardware (Solamente las placas base)

[https://bit.ly/2Zzbsei](https://bit.ly/2Zzbsei) - Enlace a la busqueda de controladoras RAID PCI en Aliexpress.

[https://bit.ly/3BpX2KT](https://bit.ly/3BpX2KT) - Enlace a el combo que he usado en la guia

[https://bit.ly/3jQxGzP](https://bit.ly/3jQxGzP) - Enlace a la fuente de alimentacion modular que uso en la guia.

[https://bit.ly/3pQuTuw](https://bit.ly/3pQuTuw) - Enlace al chasis EATX de la guia.

[https://bit.ly/3GyHjge](https://bit.ly/3GyHjge) - Enlace de busqueda rápida de chasis de servidor de rack.