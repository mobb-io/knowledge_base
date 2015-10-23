Docker-Compose
==============

What is Docker-Compose?
-----------------------

Compose is a tool for defining and running multi-container applications with Docker. With Compose, you define a multi-container application in a single file, then spin your application up in a single command which does everything that needs to be done to get it running.

|logo|

**For more, see**: https://docs.docker.com/compose

Install Docker-Compose
----------------------

You can run Compose on OS X and 64-bit Linux. It is currently not supported on the Windows operating system. To install Compose, you’ll need to install Docker first.

Depending on how your system is configured, you may require sudo access to install Compose. If your system requires sudo, you will receive “Permission denied” errors when installing Compose. If this is the case for you, preface the install commands with sudo to install.

To install Compose, do the following:

    Install Docker Engine version 1.7.1 or greater:

        * Mac OS X installation (installs both Engine and Compose)

        * Ubuntu installation

        * other system installations

    Mac OS X users are done installing. Others should continue to the next step.

        * Go to the `repository release page`_.

        * Enter the curl command in your termial.

        * The command has the following format:

        .. code-block:: sh

            curl -L https://github.com/docker/compose/releases/download/VERSION_NUM/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

        * If you have problems installing with curl, you can use pip instead:

        .. code-block:: sh

            $pip install -U docker-compose

        * Apply executable permissions to the binary:

        .. code-block:: sh

            $ chmod +x /usr/local/bin/docker-compose

        .. note:: Optionally, install command completion for the bash and zsh shell.

        * Test the installation.

        .. code-block:: sh

            $ docker-compose --version


Upgrading
---------

If you’re upgrading from Compose 1.2 or earlier, you’ll need to remove or migrate your existing containers after upgrading Compose. This is because, as of version 1.3, Compose uses Docker labels to keep track of containers, and so they need to be recreated with labels added.

If Compose detects containers that were created without labels, it will refuse to run so that you don’t end up with two sets of them. If you want to keep using your existing containers (for example, because they have data volumes you want to preserve) you can migrate them with the following command:

.. code-block:: sh

    $ docker-compose migrate-to-labels

Alternatively, if you’re not worried about keeping them, you can remove them &endash; Compose will just create new ones.

.. code-block:: sh

    $ docker rm -f -v myapp_web_1 myapp_db_1 ...

Uninstallation
--------------

To uninstall Docker Compose if you installed using curl:

.. code-block:: sh

    $ rm /usr/local/bin/docker-compose

To uninstall Docker Compose if you installed using pip:

.. code-block:: sh

    $ pip uninstall docker-compose

.. note:: If you get a “Permission denied” error using either of the above methods, you probably do not have the proper permissions to remove docker-compose. To force the removal, prepend sudo to either of the above commands and run again.

Where to go next
User guide
Get started with Django
Get started with Rails
Get started with Wordpress
Command line reference
Yaml file reference
Compose environment variables
Compose command line completion



.. code-block:: sh

    $ docker-machine create -d generic \
    --generic-ssh-user ubuntu \
    --generic-ssh-key ~/.ssh/id_rsa.pub \
    --generic-ip-address 0.0.0.0 \
    mcaputo

.. _repository release page: https://github.com/docker/compose/releases/download

.. |logo| image:: docker-compose-log.png