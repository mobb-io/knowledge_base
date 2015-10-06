.. _logging_in_for_the_first_time:

Logging in for the First Time
=============================

The following instructions are written for Linux and Mac OS X. If you’re using PuTTY as your SSH client in Windows, follow `these instructions <https://www.linode.com/docs/networking/ssh/using-putty/>`_.

1. Enter the following into your terminal window or application. Be sure to replace the example IP address with your Linode’s IP address.

.. code:: bash

    ssh root@mobb.io

2. If this is the first time connecting to your Linode, you’ll see the authenticity warning below. This is because your SSH client has never encountered the server’s key fingerprint before. Type ``yes`` and press **Enter** to continue connecting.

.. code:: bash

    The authenticity of host 'mobb.io (123.456.78.90)' can't be established.
    RSA key fingerprint is 11:eb:57:f3:a5:c3:e0:77:47:c4:15:3a:3c:df:6c:d2.
    Are you sure you want to continue connecting (yes/no)?

3. The login prompt appears for you to enter the password you created for the ``root`` user above.

.. code:: bash

    root@mobb.io's password:

4. The SSH client initiates the connection. You’ll know you’re logged in when the following prompt appears:

.. code:: bash

    Warning: Permanently added 'mobb.io,123.456.78.90' (ECDSA) to the list of known hosts.
    root@mobb.io:~#

.. note::

    If you recently rebuilt an existing Linode, you might receive an error message when you try to reconnect via SSH. SSH clients try to match the remote host with the known keys on your desktop computer, so when you rebuild your Linode, the remote host key changes.

    * Revoking the key for that IP address will fix the problem. For Linux and Mac OS X:

    .. code:: bash

        ssh-keygen -R mobb.io

    * PuTTY users must remove the old host IP addresses manually. PuTTY’s known hosts are in the registry entry:

    .. code:: bash

        HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\SshHostKeys

