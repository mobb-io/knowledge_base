Setting the Hostname
====================

You’ll need to set your system’s hostname and fully qualified domain name (FQDN). Your hostname should be something unique. Some people name their servers after planets, philosophers, or animals. Note that the system’s hostname has no relationship to websites or email services hosted on it, aside from providing a name for the system itself. Your hostname should not be “www” or anything too generic.

.. note::

    If you’re unfamiliar with Linux, one of the first things you’ll need to learn is how to use `nano <https://www.linode.com/docs/linux-tools/text-editors/nano>`_, a text editor included with most distributions. To open a file for editing, type ``nano file.txt`` where “file.txt” is the name of the file you want to create or edit. When you’re finished editing, press ``Control-X``, then ``Y`` to save the changes and ``Enter`` to confirm.

For a walkthrough of setting system’s hostname and timezone, see the following video:

.. raw:: html


    <iframe width="560" height="315" src="https://www.youtube.com/embed/KFd66g4k4i8" frameborder="0" allowfullscreen></iframe>


.. note::

    This video was created by `Treehouse <https://teamtreehouse.com>`_, which is offering Linode customers a free one month trial. `Click here <https://teamtreehouse.com/join/free-month?utm_source=linode&utm_medium=partnership&utm_campaign=linode-2013&cid=1124>`_ to start your free trial and start learning web design, web development, and more.

* Ubuntu 14.04

    Replace ``hostname`` with one of your choice.

    .. code:: bash

        echo "hostname" > /etc/hostname
        hostname -F /etc/hostname

    Check if the file ``/etc/default/dhcpcd`` exists.

    .. code:: bash

        ls /etc/default

    If so, edit it and comment out the ``SET_HOSTNAME`` directive:

    File excerpt: **/etc/default/dhcpcd**

    .. code:: bash

        #SET_HOSTNAME='yes'


Update /etc/hosts
-----------------

Next, edit your ``/etc/hosts`` file to resemble the following example, replacing ``hostname`` with your chosen hostname, ``example.com`` with your system’s domain name, and ``12.34.56.78`` with your system’s IP address. As with the hostname, the domain name part of your FQDN does not necessarily need to have any relationship to websites or other services hosted on the server (although it may if you wish). As an example, you might host “www.something.com” on your server, but the system’s FQDN might be “mars.somethingelse.com.”

File: **/etc/hosts**

.. code:: bash

    127.0.0.1 localhost.localdomain localhost
    12.34.56.78 hostname.example.com hostname

If you have IPv6 enabled on your Linode, you will also want to add an entry for your IPv6 address, as shown in this example:

File: **/etc/hosts**

.. code:: bash

    127.0.0.1 localhost.localdomain localhost
    12.34.56.78 hostname.example.com hostname
    2600:3c01::a123:b456:c789:d012 hostname.example.com hostname

The value you assign as your system’s FQDN should have an “A” record in DNS pointing to your Linode’s IPv4 address. For Linodes with IPv6 enabled, you should also set up a “AAAA” record in DNS pointing to your Linode’s IPv6 address. For more information on configuring DNS, see Adding `DNS Records <https://www.linode.com/docs/hosting-website#sph_adding-dns-records>`_.
