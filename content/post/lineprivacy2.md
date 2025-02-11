+++
title = "LINE y su privacidad - Segunda parte"
description = "Una investigación sobre la aplicación de mensajeria LINE, la cual no respeta la privacidad de sus usuarios y permite alterar de forma facil los mensajes"
date = "2020-06-08"
categories = ["Investigacion"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
Segundas partes nunca fueron buenas, o eso dicen. Pero aqui estoy yo, escribiendo este post el dia de San Valentin, fiesta empalagosa donde las haya. (digo esto por que no tengo pareja, pero acabare igual que los demás cuando tenga novia).

Haciendo un resumen rapido de la primera parte, vimos que LINE guarda sus datos (historial de chat, contactos, etc) en la raíz del dispositivo android y que utilizaba unos permisos especiales para poder acceder alli y tener permisos de escritura y lectura.

Ahora en lo que voy a centrarme es en la parte de los mensajes, y para ello hare uso de mi terminal android (un S3 mini) y SQLite Editor como la primera vez, para veais con que facilidad un malware puede cambiar o crear una entrada del historial de chats e incluso crear un chat, para poder, por ejemplo, enviarnos un enlace a un apk con contenido malicioso (que puede estar alojado en Google Play o no).

Asi que, como siempre, vuelvo a entrar en la base de datos de la aplicacion, y esta vez nos dirijimos unicamente a la tabla chat_history.

![](/img/cap6.png)

Vale, a partir de aquí os empezaré a explicar un poco como almacena LINE nuestro historial de chat. Tenemos una serie de campos en la tabla que pasare a mencionar y a explicar detalladamente para que sirven.

ID : Es el identificador del chat, no tengo más explicación para esto.

server_ID : Es el identificador del servidor desde donde se ha enviado o recibido el mensaje.

type : El tipo de chat que es, el campo es un Integer con valores del 1 al 5.

chat_id : el Identificador del chat, en este caso el identificador del chat es igual al siguiente campo que voy a explicar.

m_id (member ID) : Este es el identificador que es igual al del chat_id, este identificador es el identificador de miembro de LINE, el mismo que podemos consultar en la tabla contacts.

content : Este es el contenido del chat, aqui podeis poner lo que os de la real gana.

created_time : Tiempo (en segundos) en el que se creo el mensaje. Normalmente son Horas y Minutos.

delivered_time : Tiempo (en segundos) en el que se recibio el mensaje. Normalmente son Horas y Minutos.

status : Estado del mensaje. El campo es un Integer con valores del 1 al 5.

sent_count : Esto es para saber cuantos mensajes se han enviado. Normalmente es 1. Esto cuenta los mensajes que hemos enviado al servidor para que la app los cuente.

read_count : Esto nos sirve para saber si se ha leido el mensaje o no. Normalmente es 1. Esto cuenta los mensajes que hemos leido para que la app los cuente.

Los campos que vienen mas adelante nos indican el nombre, direccion, coordenadas GPS que muestra la localizacion del telefono en ese instante, si hemos adjuntado alguna imagen o no, su tamaño, el tipo de adjunto que es, la URL local del adjunto (si es que la tiene) y un bonito campo llamado parameter para poder insertar ciertos parametros.

Vale, ahora como podemos modificar un mensaje?

Pues vamos por partes, como dijo alguna vez Jack el Destripador.

Lo primero de todo es seleccionar el m_id de uno de tus contactos cualquiera de LINE, el cual lo sacaremos de la tabla contacts.

Lo segundo que vamos a hacer, es crear un chat. Para ello usamos la tabla chat de la base de datos.

Metemos en el chat_id el m_id de nuestro contacto. El chat_name lo dejaremos vacio, ya que por lo visto no le hace falta a LINE para abrir un chat.

Ahora el owner_id sera nuestro m_id, el de nuestro usuario de LINE. En last_from_mid meteremos el m_id de nuestro contacto si quieres que muestre el mensaje de ese contacto o que parezca que tu has enviado uno.

En el campo last_message, introducid el ultimo mensaje que se supone que habeis recibido de ese chat.

last_created_time funciona igual que el created_time de la tabla chat_history asi que lo obviaremos.

A partir de aqui estan los campos message_count y read_message_count, que recojen el count que se realiza a partir de la tabla chat_history.

is_notification es un valor true o false que le indica a LINE si ese chat va a generar notificaciones push.

Los ultimos tres campos son campos que a mi parecer no son importantes para lo que estoy exponiendo en este post, asi que los obviare. Si tenéis alguna duda de lo que hacen esos tres ultimos campos no dudéis en preguntarmelo.

Ahora con el chat creado en la tabla chat, nos vamos a chat_history, rellenamos los campos e voila.

Parece que tenemos un nuevo mensaje de uno de nuestros contactos :)
