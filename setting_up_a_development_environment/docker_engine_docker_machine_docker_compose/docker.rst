Docker
======

|logo|

Understand how Docker works and how you can use it.

What is Docker?
---------------

.. raw:: html

    <div style="width: 560px; height: 340px; margin: 0 auto;">
        <iframe width="560" height="315" src="https://www.youtube.com/embed/cRczhEvSH2A" frameborder="0" allowfullscreen></iframe>
    </div>

For more, visit: https://www.docker.com/what-docker

Install Docker
--------------

Log into your Ubuntu installation as a user with sudo privileges.


Verify that you have wget installed.

.. code-block:: sh

    $ which wget

If wget isn’t installed, install it after updating your manager:

.. code-block:: sh

    $ sudo apt-get update
    $ sudo apt-get install wget

Get the latest Docker package.

.. code-block:: sh

    $ wget -qO- https://get.docker.com/ | sh

The system prompts you for your sudo password. Then, it downloads and installs Docker and its dependencies.

.. note:: If your company is behind a filtering proxy, you may find that the apt-key command fails for the Docker repo during installation.

To work around this, add the key directly using the following:

.. code-block:: sh

    $ wget -qO- https://get.docker.com/gpg | sudo apt-key add -

Verify docker is installed correctly.

.. code-block:: sh

    $ docker run hello-world
    Unable to find image 'hello-world:latest' locally
    511136ea3c5a: Pull complete
    31cbccb51277: Pull complete
    e45a5af57b00: Pull complete
    hello-world:latest: The image you are pulling has been verified.
    Important: image verification is a tech preview feature and should not be
    relied on to provide security.
    Status: Downloaded newer image for hello-world:latest
    Hello from Docker.
    This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:

    * The Docker client contacted the Docker daemon.

    * The Docker daemon pulled the "hello-world" image from the Docker Hub. (Assuming it was not already locally available.)

    * The Docker daemon created a new container from that image which runs the executable that produces the output you are currently reading.

    * The Docker daemon streamed that output to the Docker client, which sent it to your terminal.

To try something more ambitious, you can run an Ubuntu container with:

.. code-block:: sh

    $ docker run -it ubuntu bash


For more examples and ideas, visit: http://docs.docker.com/userguide/

.. |logo| image:: docker-logo.png
