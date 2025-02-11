+++
title = "HUGO: Qué es y como realizar nuestro propio blog - Segunda parte"
description = "Un tutorial sobre el framework de static pages HUGO, como configurarlo y desplegarlo en firebase"
date = "2020-06-09"
categories = ["Tutoriales"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-09"
+++
Bienvenidos a esta segunda parte del tutorial del framework HUGO! Como dije al final del post anterior, este cubrirá la creación de la estructura de directorios del blog, además de como crear y editar contenido.

Primeramente crearemos la estructura nuestro blog, para ello, crearemos una carpeta donde queramos situar la estructura del mismo. Yo la he creado en Documentos y la he llamado blog.

![](/img/tuto1.png)

Una vez creada, entramos en la carpeta desde la terminal de comandos, y creamos un nuevo skeleton de HUGO con el siguiente comando:

**hugo new site <nombredelblog>**

Al ejecutarlo, nos saldrá el siguiente mensaje:

![](/img/tuto2.png)

Indicándonos que ya tenemos la estructura de directorios lista para usar, y que tendremos que descargar un tema para darle un aspecto visual llamativo al blog, además de indicarnos que podemos lanzar el servidor de desarrollo para poder ver los cambios que realicemos en el sitio web en tiempo real.

Una vez realizado, cambiaremos de directorio a la carpeta correspondiente con el nombre del proyecto, en este caso, test, y podremos visualizar la estructura de directorios de HUGO.

La estructura de directorios de HUGO es la siguiente:

![](/img/tuto3.png)

Los temas o plantillas para la web los podemos conseguir aquí: [https://themes.gohugo.io/](https://themes.gohugo.io/)

Para este ejemplo elegiremos uno para realizar un CV online ([https://themes.gohugo.io/hugo-devresume-theme/](https://themes.gohugo.io/hugo-devresume-theme/)), pero tenemos también temas disponibles para realizar un blog, etc.

Para instalar el tema, simplemente cambiaremos al directorio themes de la estructura basica de HUGO y clonaremos el tema desde Github en esa carpeta con el comando **git clone** [https://github.com/cowboysmall-tools/hugo-devresume-theme](https://github.com/cowboysmall-tools/hugo-devresume-theme)

Una vez clonado, entramos en la carpeta del tema, y dentro de la misma, nos encontraremos una llamada **exampleSite**, esta contiene la configuración necesaria para que funcione el tema en un archivo llamado **config.toml** que deberemos copiar y pegar en la raíz de la carpeta de nuestro proyecto.

Una vez lo tengamos copiado, ya podremos ejecutar el servidor web interno de HUGO, con el cual, podremos ver los cambios que hagamos en nuestra web,en vivo y en directo. Este servidor lo arracaremos con el comando **hugo serve** y accederemos a la web mediante nuestro navegador web favorito y poniendo **localhost:1313** en la barra de direcciones.

![](/img/tuto4.png)

Recomiendo antes que nada, cambiar en el config.toml, la variable **baseURL** para que apunte a **/**, por que si no, da problemas al cambiar de página en Firebase y cambiar la variable **languagecode** por el lenguaje que estemos usando en el blog, por ejemplo, **es**.

Ahora vamos a pasar a la creación y modificación del contenido del blog o web. Vale, y como hacemos esto?

En HUGO, para facilitar la tarea de creación de contenido, disponemos de Archetypes, que son templates o plantillas situadas en el directorio Archetypes de nuestro proyecto, que contienen parámetros ya preconfigurados y también una disposición del contenido apropiada para nuestras webs. Estas se pueden usar ejecutando un comando dentro de la carpeta del proyecto: **hugo new posts/hola-mundo.md.**

Esto creará un archivo nuevo de contenido en la ruta content/posts/hola-mundo.md usando el primer archivo de archetype que encuentre de esta lista:

archetypes/posts.md
archetypes/default.md
themes/my-theme/archetypes/posts.md
themes/my-theme/archetypes/default.md

También podemos realizarlo de forma manual, como, por ejemplo, en el caso de mi blog, donde copio y pego el **front matter** o las cabeceras del fichero Markdown que identifican al contenido de dentro como un post.

![](/img/tuto5.png)

Cada tema llevará sus archetypes especificos, y los podremos copiar a nuestro proyecto desde su carpeta.

También puede diferir de que el tema, como es con el caso del CV online, no tenga contenido y este esté definido en la propia configuración del sitio, en el archivo **config.toml** y lo tengamos que modificar directamente desde ahí.

El **front matter** lleva sus propias variables definidas, donde, por ejemplo, como es en el caso de un blog, podemos decir a que categoría pertenece el post, que alias tiene, las taxonomías, el autor del post, la fecha del mismo y el titulo y su descripción.

![Muestra del config.toml del CV online](/img/tuto6.png)

Todo el contenido se guarda y distribuye en archivos de formato Markdown (.md), con lo cual, es aconsejable mirar esta guía del marcado para tener una ayuda a la hora de escribir los posts: [https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)

El contenido que se escriba en los posts va despúes del **front matter**, el comienzo del cual he delimitado en la imagen con una flecha roja, y salvo alguna variación en forma de shortcodes para insertar imagenes más rápido o cierto contenido dinámico, el resto del marcado funciona igual que en la guía.

![Despúes de la flecha roja es donde empezaría el contenido del post](/img/tuto7.png)

Algunos de los shortcodes disponibles están en esta web: [https://gohugo.io/content-management/shortcodes/](https://gohugo.io/content-management/shortcodes/)

Con esto, ya tendríamos lo básico para realizar un blog o página web con HUGO.

Ahora, solo nos falta subir la web, podemos hacerlo en cualquier plataforma que admita static pages, pero en este caso, elegiré Firebase por la comodidad y sencillez del mismo.

Para poder subir el sitio web a Firebase, primeramente, deberemos entrar en el panel y crear un nuevo proyecto.

Una vez lo tengamos creado, nos iremos a nuestra terminal, e instalaremos Node.JS si no lo tenemos instalado en el sistema.

Una vez instalado, ejecutaremos el siguiente comando para instalar de forma global las firebase-tools, que nos serán necesarias para subir el sitio web a la plataforma: **npm install -g firebase-tools**

Una vez instaladas las herramientas, nos loguearemos en firebase con el comando **firebase login.**

Una vez logueados, nos iremos a la raíz de nuestro proyecto de HUGO, e inicializaremos el proyecto de firebase con el comando **firebase init**, y seguiremos los siguientes pasos en el asistente que nos saldrá en la consola:

Elegimos Hosting en la primera pantalla.
Elegimos el proyecto que habremos creado anteriormente en Firebase
Aceptamos la ruta por defecto para el fichero de bases de datos
Aceptamos la ruta por defecto de publicación de la web, que es **public**
Elegiremos que no a la pregunta de si estamos desplegando una app de una sola página.

Ahora, solo nos queda desplegarlo. Esto lo haremos con el comando **hugo && firebase deploy**, y al terminar de ejecutarse el comando, nos dará la dirección web donde estará disponible nuestra web, que en todos los casos, es el ID del proyecto de firebase, seguido de **web.app.**

Con esto concluimos el tutorial de HUGO! Espero que os haya gustado y os sea de utilidad para crear un blog, web corporativa, etc...
