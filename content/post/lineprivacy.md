+++
title = "LINE y su privacidad - Primera parte"
description = "Una investigación sobre la aplicación de mensajeria LINE, la cual no respeta la privacidad de sus usuarios y permite alterar de forma facil los mensajes"
date = "2020-06-08"
categories = ["Investigacion"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
Hoy vamos a analizar la conocida aplicación de mensajería instantanea para android llamada LINE.

LINE, como ya sabeis es una aplicación de mensajería instantánea propia para teléfonos móviles, PC y Mac. Además de la mensajería básica, los usuarios de la aplicación disponen de envío de imágenes, vídeo, mensajes de audio y hacer llamadas VoIP. La aplicación es reconocida por sus singular sistema de pegatinas (stickers), reemplazando a los tradicionales iconos.

Bien, pues me encontraba trasteando con mi telefono android como siempre, hasta que me dio por juguetear con el SQLite Editor para android. echandole un vistazo a las apps que tenian base de datos interna, me encuentro con las de LINE, y me quedo perplejo al ir mirando la gran cantidad de tablas que poseia la base de datos principal de la aplicacion, llamada naver_line. La base de datos esta alojada en la raiz del dispositivo android, y como todos sabeis, solo root tiene acceso allí, por eso tuve que hacer uso de la app SQLite Editor para poder ver las bases de datos.

![](/blog/img/cap1.png)

De esta base de datos, me llaman la atención dos tablas de ella, una llamada chat_history y otra llamada contacts.

Me decanto por una de ellas dos, y la primera que miro es la de chat_history y... vaya sorpresa! esta todo lo que he chateado con mis amigos en LINE!

![](/blog/img/cap2.png)

Cagate lorito. Tras quedarme perplejo, me dirijo a la siguiente tabla que me daba curiosidad por mirar, la de contacts. Y vaya con la base de datos, están todos mis contactos de LINE, con su respectivo m_id (membership ID), el nombre que tienen en el servidor y en la lista de contactos de LINE.

![](/blog/img/cap3.png)

Ya que estaba puesto con la base de datos principal de LINE, me pregunte a mi mismo si en esa base de datos se guarda la información del perfil del usuario (su clave de autenticación en LINE, etc), y si, lo hace en la tabla settings.

![](/blog/img/cap4.png)

Increible, sin palabras me hallo. Publique las capturas en mi Twitter personal, detallando la magnitud del fallo de seguridad y privacidad. Un follower mio me planteo una duda: Como era posible que LINE hubiese escrito esa base de datos y otras mas que tiene la app sin ser root? Era algo logico, ya que al instalar la app, esta crea todo lo necesario para funcionar en el directorio raiz, en la carpeta “app”, que es donde estan almacenados todos los datos de todas las aplicaciones instaladas y que les permiten funcionar y/o guardar configuraciones.

Y pensareis, donde esta el peligro de todo esto? Pues, el peligro esta en un posible malware que aproveche el root de nuestro dispositivo, y poder acceder a la raiz del mismo y copiar esos archivos a un servidor remoto con la posibilidad de que vean que hemos estado hablando, quienes son nuestros contactos e incluso hacernos pasar por esa persona. Eso no es todo, por que tambien se pueden modificar los mensajes en un dispositivo infectado y hacernos creer que un contacto nuestro nos ha mandado un enlace a un apk malicioso.

Eso de los mensajes lo explicare mas adelante, pero antes no me despediré sin una imagen que os muestre lo anteriormente dicho.

![](/blog/img/cap5.png)
