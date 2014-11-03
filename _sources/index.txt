
Usando Vagrant
==============
.. image:: ./_static/images/slides/upload_image.png
            :width: 80%
            :align: center

About Me
========

Luis Mayta | @slovacus

http://luismayta.github.com


¿Que es Vagrant?
================

* Herramienta Open Source
* Multiplataforma
* Permite Virtualizar ambientes
* Soporta Virtualbox ...
* Configurable

¿Porque lo necesito?
====================

* Probar server
* Ambientes de Desarrollo Repicables
* Optimizacion
* Tener las mismas dependencias que Produccion

Ejemplos
========

===============     =============
Proyecto A          Output
---------------     -------------
PHP 5.3             PHP 5.4  
MySQL 5             MongoDB 2.6
Apache Solr 4.4     SQLServer
                    Apache Solr 4.8
                    Redis
===============     =============

Casos de la vida real:
======================

**RRHH :** Hoy empieza el nuevo desarrollador

Cual es su primera tarea:

**JP (Junior Programmer):** Levantar su Entorno de Desarrollo.

Tiempo de tarea
===============

**1 dia o mas**


.. slide:: Nos Ayuda a bajar la Barrera de entrada al Proyecto
    :level: 2


.. slide:: Cambios en la Tecnologia
    :level: 2

    **Salio la nueva Version de Apache Solr**
    o
    **ya no usaremos apache solr vamos a usar ElasticSearch**

.. slide:: Vagrant es nuestro Amigo Fiel
    :level: 2

.. slide:: Como lo Instalamos
    :level: 2

    .. code-block:: enlace de descarga

        http://downloads.vagrantup.com/

    Vagrant usa como dependencia un virtualizador de software (virtualbox)

    .. code-block::

        https://www.virtualbox.org/

.. slide:: Como Comienzo
    :level: 2

    * Crear el VagrantFile (Describe los recursos, tipo de Maquina y Software que vamos a usar)

    .. code-block::

        mkdir test-vagrant
        cd test-vagrant
        vagrant init

.. slide:: Box
    :level: 2

    Es la imagen del sistema operativo que usaremos.

    podemos descargar de este enlace:

    .. code-block::
        http://www.vagrantbox.es/

     como somos amantes de **debian** usaremos algo parecido ubuntu

     .. code-block::

        http://files.vagrantup.com/precise32.box

.. slide:: Agregamos el Box
    :level: 2

    .. code-block::

         vagrant box add nombre_del_box http://url_del_box.box

    .. code-block::

         vagrant box add precise32 http://files.vagrantup.com/precise32.box

    **comprobemos**

    .. code-block::

         vagrant box list

.. slide:: Usemos el Box
    :level: 2

    **cambiemos cosas en el Vagrantfile**

    busquemos:
    
    .. code-block::

        config.vm.box = "base"

    y lo cambiamos por:
        
    .. code-block::

        config.vm.box = "precise32"
        
        
.. slide:: Levantemos el Ambiente
    :level: 2

    **Ahora si preparados**

    .. code-block::

        vagrant up

    ahora para instalar las dependencias es:
        
    .. code-block::

        vagrant ssh
        

.. slide:: Ejemplo de instalar Git
    :level: 2

    .. code-block::

        vagrant ssh
        sudo apt-get update
        sudo apt-get install git

    Listo eso es Todo, Aplausos :P
        
.. slide:: Gracias
    :level: 1
    
 
.. slide:: Preguntas
    :level: 2

    * Esto me ayuda a tener entornos repicables?
    * La instalacion pesa mucho, no lo puedo tener en un repo.
    * para que hacer todo esto si puedo usar simplemente VirtualBox.
    * Donde esta la Automatizacion? ...
    * Que estafador ... (El Gringo de Go ...) 

.. slide:: Exacto esto no es todo
    :level: 1

.. slide:: Comandos de Vagrant
    :level: 2

    .. code-block::

        vagrant up
        vagrant init
        vagrant destroy
        vagrant halt
        vagrant provision
        vagrant ssh
        vagrant status
        ...

.. slide:: Configuracion de Vagrant
    :level: 1

.. slide:: Ejemplo de config Vagrant
    :level: 1

.. slide:: Configuracion network
    :level: 2

    .. code-block::

         guest_config.vm.network :private_network, ip: "192.168.33.10"
         guest_config.vm.network "public_network"
        
.. slide:: Enrutamiento de Puertos
    :level: 2

    .. code-block::

         guest_config.vm.network :forwarded_port, guest: 80, host: 8888, auto_correct: true
         guest_config.vm.network :forwarded_port, guest: 3306, host: 8889, auto_correct: true
         guest_config.vm.network :forwarded_port, guest: 5432, host: 5433, auto_correct: true

.. slide:: Configuracion de Pc
    :level: 2

    .. code-block::
         guest_config.vm.hostname = "guest"
         guest_config.vm.provider :virtualbox do |v|
             v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
             v.customize ["modifyvm", :id, "--memory", "1024"]
         end

.. slide:: Sincronizacion de Carpetas con NFS
    :level: 2

    .. code-block::
        guest_config.vm.synced_folder "./", "/var/www", {:mount_options => ['dmode=777','fmode=777']}

.. slide:: Exportar el Box
    :level: 2

    .. code-block::
       vagrant up
       (setup)
       vagrant halt
       vagrant package
       mv package.box ~/boxes/my_box.box


.. slide:: Provision
    :level: 1


.. slide:: Tipos de Provision
    :level: 2

    * Shell
    * Puppet
    * Puppet Server
    * Chef
    * Chef Server
    * Ansible
    * Fabric

.. slide:: Usando Puppet
    :level: 2

    .. code-block::

    guest_config.vm.provision :puppet do |puppet|
        puppet.manifests_path = "provision/puppet/manifests"
        puppet.manifest_file  = "init.pp"
        puppet.module_path = "provision/puppet/modules"
    end

.. slide:: Demo
    :level: 1

.. slide:: Vagrant halt
    :level: 2

    Preguntas?
    ----------
