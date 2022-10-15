+++
title = "Hostings: o como cagarla soberanamente - Segunda Parte"
description = "Una investigación propia de hace unos años donde demuestro como una vulnerabilidad grave puede perjudicar a un hosting"
date = "2020-06-08"
categories = ["Investigacion"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
Buenas a todos otra vez, como ya sabreis, os prometí la segunda parte de esto y aquí esta. Asi que sin mas dilacion, empezemos.

Esta vez nos desplazamos a Gijón, para poder, con este articulo, finalizar la historia que empezé antes.

Esta empresa es un show puro y duro, ademas del exponente maximo de caradurismo. Solo hay que hacer una busqueda en Google de dicha empresa, y aparece directamente en trabajobasura.info, y cuando tu empresa aparece alli, algo malo has hecho o estas haciendo. Y los comentarios (del año pasado, por cierto) de esa pagina hablan por si solos, hay gente descontenta por lo que parece.

Nunca esta demás corroborar y comprobar que lo que dicen en esos comentarios es cierto. Y como decía en el primer comentario, le he pasado el W3c validator, y aunque pongan que usan XHTML y el logo de W3c en la pagina, no esta validado ni lo estará nunca si no corrigen los 69 errores que devuelve.

Y ahora pasamos al test de campo, en el que siempre suelo comprobar primero si existe una vulnerabilidad de Inyeccion SQL y si no existe busco otras vulnerabilidades o configuraciones mal hechas.

En este caso cogí una web cualquiera y me puse a mirar por encima. Parece ser que no hay por donde mirar, pero como siempre, la barra de direcciones me delata las cosas. Al acceder a un articulo de la web, me encuentro con el mismo panorama que en la empresa de Granada, solo que en esta pega el cante que no veas (/index.php/id/11/objeto/).

Enseguida que le hago la técnica del codo-comílla delante de /objeto/, la web se queja, mira tu por donde.

Extraigo los usuarios de la base de datos con sqlmap, y me voy a otra web, hago lo mismo y encuentro a un usuario que se repite en la anterior web, con la misma contraseña. Madre mía….

Me voy directamente al panel, entro y veo una advertencia recordandole al administrador la importancia de la implantacion de la LOPD por parte de esa empresa, que en este caso, es nula.

Y ahora viene lo mejor, me voy a revisar la parte del panel que pone Datos y allí estan los datos de la empresa, metadatos, etc.

Veo un apartado que pone “documentación”, y le doy clic, pensándome que seria algún manual sobre el CMS o algo así. Que va, para nada. Ahí estaba, guardado en el mismo dominio, en un puto PDF... LAS CONTRASEÑAS DE TODO EL SITIO. Contraseñas de emails, panel de administrador, estadísticas... y el ISPConfig (es un cpanel Open-Source con licencia BSD) del dominio, pudiendo acceder y subir archivos vía Web-FTP.

Ya que tenia la ruta de la documentación, me dije a mi mismo: “esto seguramente este así en todas las paginas web que hayan diseñado, probaré en otra a ver”. Y exacto, acerté con mi premonición. Todos y cada uno de los sitios web de esa empresa, tenían la documentación en esa misma ruta.

De hecho tambien me pasee por su pagina principal, la unica que no tenia documentacion, pero he logrado acceder tambien a su panel.

Y con esto, pongo fin a esta investigación, donde, como podreís comprobar, es imprescindible que si contratamos un hosting, sepamos con quien trabajamos, como trabajan y que cumplan toda la normativa referente a ciberseguridad para evitar sustos o males mayores.
