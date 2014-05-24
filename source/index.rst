
Reduciendo El acoplamiento entre Aplicaciones con RabbitMQ
==========================================================

About Me
========

Luis Mayta | @slovacus

http://luismayta.github.com


Porque necesito usar Mensajes
-----------------------------


Galeria de Imagenes
-------------------

un ejemplo puede ser una Galeria de Imagenes.


¿Recontra sencillo no es asi?
-----------------------------


Hasta que LLega el AF (sin documentación)
-----------------------------------------


LLegan los Nuevos Requerimientos
--------------------------------

* podemos notificar a los amigos del usuario sobre sus nuevas imagenes.
* premios a los usuarios por cada foto que suben.
* Esta de moda Twitter, enviemos notificaciones.


Problemas!!
-----------

* Estamos mostrando las imagenes sin redimencionar.
* Ya tenemos un proceso de redimencion en Java usemos eso (pero la aplicación esta en php :).
* Otro desarrollador dice: necesito llamar tus sistemas PHP pero desde Python.
* y tambien a Java y Ruby.


Performance en la aplicación
----------------------------

**el usuario**
* La aplicación se demora demasiado publicando una imagen.
* y es por esto:
    * Redimención.
    * Notificación.
    * Premios.
    * ....
    * ....
    * clases que contienen 6000 lineas de codigo.
    * Programadores que piensan en escalabilidad Vertical.

**usuario:** a mi no me interesa, yo quiero publicar mi imagen!!!

Nosotros
--------


Nuestro codigo
--------------

IndexController:: 

    <?php
            //Image Controller solo es un ejemplo
            $this->validateParameters($params);
            $this->isActiveProxy();
            $ruta = $this->serverElements . $this->config['rest']['image'];
            $request = \Requests::post($ruta, array(), $params, $this->options);
            // Resize image

            $this->notifyFriends();
            $this->awardUser();
            $this->tweetNewImage();

Preguntas
---------

* ¿Nuestro codigo puede aceptar nuevos requerimientos?
* Que pasa si:
    * necesitamos incrementar la velocidad de la redimención.
    * las notificaciones de los usuarios se tienen que enviar por email.
    * debemos quitar el servicio de twitter para las nuevas imagenes.
    * en la redimención se tiene que usar Java o C

¿Que Hacemos?
-------------

* Usamos Crones?
    * no son inteligentes.
    * no sirven para escalabilidad.
    * Como lo haces en PHP no puedes usar Java.

**Los cambios lo quieren para Ayer**

**Que Hacemos!!!**


Usamos Mensajeria
-----------------

Imagen de Mensajeria

Diseño
------

Publish / Suscribe Pattern

[Imagen]


Implementación
--------------

IndexController::

    <?php
            //Image Controller solo es un ejemplo
            $this->validateParameters($params);
            $this->isActiveProxy();
            $ruta = $this->serverElements . $this->config['rest']['image'];
            $request = \Requests::post($ruta, array(), $params, $this->options);
            // Resize image

            $this->notifyFriends();
            $this->awardUser();
            $this->tweetNewImage();

Otras Implementaciones
----------------------


**No hay otras Implementaciones**


¿Que nos Permite Hacer La Mensajeria?
-------------------------------------

* Compartir Datos entre procesos.
* Procesos Pueden Ser Parte de diferentes Aplicaciones.
* las Aplicaciones Pueden Vivir en Diferentes Servidores.
* Redundancia.
* Disponibilidad.
* Desacoplamiento.
* Escalabilidad.
* Elasticidad.


Conceptos
---------

* Los mensajes son enviados por **publicacdores**
* Los mensajes se envian a **Consumidores**
* Los mensajes pasan a través de un **Chanel**


RabbitMQ
--------


¿Que es RabbitMQ?
----------------


RabbitMQ
--------

* Sistema de Mensajeria Empresarial.
* Codigo Libre.
* Escrito en Erlang.
* Soporte Comercial.
* Mensajeria via AMQP.


Instalación
-----------

Mac OS X::

    brew install rabbitmq

Debian::
    
    $ sudo apt-get update
    ...
    $ sudo apt-get install rabbitmq-server

Windows::

    Descarga y next next :p



Caracteristicas
---------------

* confiable y altamente Escalable.
* Fácil de Instalar.
* Fácil de Clusterizar.
* Multiplataforma.
* AMQP 0.8 - 0.9.1


Clientes AMQP
-------------

* Java
* .NET/C#
* Erlang
* Ruby
* Python
* PHP, PERL ...


Administración
--------------


Debian::

    $ rabbitmq-plugins enable rabbitmq_management
    $ sudo service rabbitmq-server start

Mac OS X::

    $ brew services start rabbitmq
