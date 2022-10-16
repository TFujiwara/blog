+++
title = "HUGO: Qué es y como realizar nuestro propio blog - Primera parte"
description = "Un tutorial sobre el framework de static pages HUGO, como configurarlo y desplegarlo en firebase"
date = "2020-06-08"
categories = ["Tutoriales"]
author = "Cristian Garcia Quevedo"
lastmod = "2020-06-08"
+++
Hoy, en esta serie de tutoriales, nos enfocaremos directamente en el framework HUGO y como realizar un sencillo blog o sitio web estático con el.

HUGO es un generador de páginas web estáticas rápido y moderno, escrito en el lenguaje de programación Go, y diseñado para hacer la creación de sitios web más amena.

Los sitios web que se realizan con Hugo se pueden hostear en cualquier lugar, incluyendo Heroku, Gihub Pages, Gitlab Pages, Firebase, Google Cloud Storage, Amazon S3, Azure y muchos más, y además funciona bien con CDNs. Estos sitios web se ejecutan sin la necesidad de una base de datos o dependencias como Ruby, Python o PHP.

El uso principal que se le da a este framework es para construir un blog, un sitio web empresarial, un portfolio, documentación sobre algo, una landing page o una web con miles de páginas.

Es indicado para la gente que quiera escribir en un editor de texto como Atom o Visual Studio Code en vez de un navegador, sin introducir ningún tipo de credenciales y que quiera diseñar su página web a su gusto.

Ahora, procederemos a instalar HUGO, y aquí tenemos varias opciones, las cuales, como siempre, sois libres de elegir según os convenga:

Binarios precompilados para los siguientes sistemas operativos:

- macOS
- Windows
- Linux
- OpenBSD
- FreeBSD

También lo tenemos disponible en Docker y mediante homebrew.

###Instalación mediante binarios precompilados

Descargamos la versión apropiada para el sistema operativo que poseamos desde [https://github.com/gohugoio/hugo/releases](https://github.com/gohugoio/hugo/releases). Una vez descargado, el binario se puede ejecutar desde cualquier lugar. No es necesario instalarlo en una localización global. Esto funciona muy bien en hosts compartidos y otros sistemas donde no tengamos una cuenta con privilegios de super usuario.

Idealmente, si usas Windows, tendrias que incluir el ejecutable en el PATH para poder ejecutar en cualquier sitio.

En Linux, se aconseja instalarlo en /usr/bin/ para poder ejecutar HUGO directamente desde cualquier directorio de nuestra distribución favorita.

###Instalación mediante Docker

Descargaremos la imagen de Docker mediante el comando docker pull klakegg/hugo.

###Instalación mediante Homebrew

Si usamos homebrew, podemos instalar el framework mediante el comando brew install hugo.

Con esto, terminamos la primera parte del tutorial, que cubre la instalación de este framework. En el siguiente post, veremos como crear la estructura de directorios del blog, y aprenderemos un poco a crear y modificar contenido.
