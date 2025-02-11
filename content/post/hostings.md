+++
title = "Hostings: o como cagarla soberanamente - Primera Parte"
description = "Una investigación propia de hace unos años donde demuestro como una vulnerabilidad grave puede perjudicar a un hosting"
date = "2020-06-08"
categories = ["Investigacion"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
Hola a todos otra vez. Hoy os vengo a hablar de los hostings, ese caso aparte donde te pueden joder (y arruinar) vivo si tienes una vulnerabilidad grave y se hacen con el control de tu servidor al completo. Así que siguiendo la misma pauta de siempre, voy a contaros como dos hostings que son en mayor o menor medida, importantes, la han cagado pero bien, y se arriesgan a perderlo todo si se hace un Full-Disclosure de la vulnerabilidad o vulnerabilidades presentes en su servidor.

Bueno, pues comencemos con esto. Nos desplazamos hasta Granada para empezar a contar nuestra historia.

Día si y día también, me paso los días detrás del ordenador viendo otras empresas de hosting y diseño web, para ver que tipo de trabajos hacen y si me convence la maquetación de las mismas y mirar a ver si puedo copiarles algo que me pueda ayudar de cara al futuro venidero.

Viendo una de estas paginas, descubro algo que me llama muchísimo la atención, y es que tanto lo que es producto.php?id= y el id del producto van separados por barras (/). Suele ser una particularidad de un CMS propio, pero ya que me llamo la atención, por que no probar a ver si era Inyectable con parámetros SQL, para saber si el CMS estaba bien hecho o por el contrario, era una putísima mierda y había que arreglarlo cuanto antes.

Pasando directamente a la acción, compruebo que el parámetro es vulnerable pero me encuentro con una sorpresa, me devuelve un error 404.

![](/img/4041.png)

Una vez que me había devuelto el maravilloso error 404, pienso: “Y si pruebo a ponerle la extensión a producto y tirar directamente una consulta en el id?” .

Y ahí empezó la fiesta o el desastre, como lo queráis llamar.

Tras inyectar manualmente y ver que me devolvía la versión de la base de datos, procedí a inyectar automáticamente con sqlmap.

Una vez con la tabla usuarios en la mano, revise a ver que usuarios eran los que mas se repetían. Habían dos usuarios que se repetían que eran administradores en todos los sitios hosteados por esta empresa y usaban siempre la misma contraseña.

Así que, decidí echar un vistazo a su pagina principal, y de paso, saber si también era vulnerable o no.

Revisando un poco el pie de la web para ver como ponerme en contacto con ellos después, vi que estaban asociados a las comunidades de diseñadores web de Granada, además de ser una empresa joven y emprendedora.

![](/img/granada.png)

Así que, decidido, accedí al panel de administración, y subí una shell. Estuve curioseando un poco para ver los limites del usuario propietario de la web. Me impresione al saber que podía llegar a ver el contenido de /usr/home/ viendo todas las web almacenadas en el servidor.

Claramente, esto no me bastaba, y me fije en los permisos de las carpetas. Tenemos permiso de ejecución en todas, y sabiendome de antemano la ruta donde se alojaban los archivos de las diferentes web (/usr/home/usuario/www/) procedí a investigar aun mas, pero tenia que hacer uso de otra shell ya que, esta no me permitía precisamente hacer lo que yo quería.

Subí una shell cgi y la escondí en un directorio que no usaban mucho, y con esa shell pude hacer lo que me proponía desde el principio.

Una vez dentro de la shell cgi, fui carpeta por carpeta, buscando los archivos de configuración tanto del CMS propio como de WordPress (Averigue que habia varios WordPress instalados paseandome de dominio en dominio) y mostrandolos con cat, para luego posteriormente guardarlos en un archivo de texto en mi propio PC que después eliminaría.

Una vez con los usuarios y passwords de conexión a las bases de datos, el crearme una cuenta y darme permisos de administrador tanto en WordPress como en el CMS propio era coser y cantar.

Y eso es todo por ahora, seguiremos con la segunda parte, que es cuanto menos, mas de lo mismo, pero mas interesante y con demasiados clientes.
