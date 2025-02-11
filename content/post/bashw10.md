+++
title = "Bash en Windows 10: ¿Es seguro? ¿O monto mejor un arranque dual?"
description = "Una investigacion breve sobre el subsistema de Linux en Windows 10 en sus inicios"
date = "2020-06-08"
categories = ["Investigacion"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
A todos nos pareció genial que Microsoft introdujera Linux en su sistema privativo mediante una alianza con Canonical para apaciguar los ánimos de la comunidad GNU y así posicionarse con algo que esta en aumento, el numero de equipos domésticos, de empresa, y demás, que usan Linux.

Además, para poder usar esta característica, hace falta activar el modo desarrrollador de Windows 10.

Pero... ¿Cuán seguro es el subsistema Linux implementado por Microsoft en Windows 10?

Veamos... según Alex Ionescu, de Crowdstrike, este subsistema es totalmente inseguro, ya que deja abiertas muchas puertas para atacar tanto a Windows como a su subsistema Linux.

Y cuales son las razones por las que el subsistema es inseguro?

Lo primero de todo, es que el subsistema no corre de ninguna manera dentro del hipervisor Hyper-V que posee el propio Windows, teniendo acceso al hardware del equipo.

Lo segundo, las unidades del sistema de Windows, están mapeadas en /mnt, siendo totalmente accesibles y podiendo leer y escribir en las mismas.

![](/img/bash1.png)

Tercero, el subsistema usa un kernel customizado por parte de Microsoft (y que se actualiza mediante Windows Update), y este no es Open Source como la mayoría esperaba. Si quieres actualizar el kernel por el actual (4.8.12) tienes que hacer virguerias para actualizarlo, tanto el propio kernel, como el sistema operativo del mismo (Ubuntu).

![](/img/bash2.png)

Cuarto, hay muchas maneras detalladas en las que Windows podría inyectar codigo en el subsistema Linux, y a la inversa. Por ejemplo, si un ransom infecta la parte Linux de Windows 10, y sabiendo que monta las unidades del sistema Windows en /mnt, menuda fiesta tendremos montada...

Pero, hay un peligro para los usuarios de a pie? De momento no, por que esta caracteristica no esta muy extendida, y por lo tanto, no llama la atención de los ciberdelincuentes, aunque no se puede asegurar que ya este circulando algun exploit salvaje que afecte a esto.

Por lo tanto, cual es mi consejo? Montaros un arranque dual, y no activeis eso de Bash en Windows, contra mas lejos de eso, mejor.
