Disabling SSH Password Authentication and Root Login
====================================================

You just strengthened the security of your Linode by adding a new user and generating SSH keys. Now it’s time to make some changes to the default SSH configuration. First, you’ll disable password authentication to require all users connecting via SSH to use key authentication. Next, you’ll disable root login to prevent the ``root`` user from logging in via SSH. While these steps are optional, they are strongly recommended.

.. note::

    You may want to leave password authentication enabled if you connect to your Linode from many different desktop computers. This will allow you to authenticate with a password instead of copying the private key to every computer.

Here’s how to disable SSH password authentication and root login:

1. Open the SSH configuration file for editing by entering the following command::

    sudo vi /etc/ssh/sshd_config

.. note::

    If you see a message similar to -bash: sudo: command not found, you’ll need to install ``sudo`` on your Linode. To do so, login as root by entering the ``su`` command, and type the ``root`` password when prompted. Next, install ``sudo`` by entering the following command: ``apt-get install sudo``. After ``sudo`` has been installed, logout as the ``root`` user by entering the ``exit`` command.

2. Change the ``PasswordAuthenticatio``n setting to ``no`` as shown below. Verify that the line is uncommented by removing the # in front of the line, if there is one::

    PasswordAuthentication no

3. Change the ``PermitRootLogin`` setting to ``no`` as shown below::

    PermitRootLogin no

4. Save the changes to the SSH configuration file by pressing ``<ESC>`` type ``:wq!`` and then press ``<ENTER>``.

5. Restart the SSH service to load the new configuration. Enter the following command::

    sudo service ssh restart

After the SSH service restarts, the SSH configuration changes will be applied.

