Adding a New User
=================

In the Getting Started guide, we asked you to login to your Linode as the root user, the most powerful user of all. The problem with logging in as root is that you can execute any command - even a command that could accidentally break your server. For this reason and others, we recommend creating another user account and using that at all times. After you log in with the new account, you’ll still be able to execute superuser commands with the sudo command.

To add a new user, log in to your Linode via SSH.

* Ubuntu 14.04

    1. Create the user with the following command. Replace exampleuser with your desired username::

        adduser exampleuser

    2. Add the user to the sudo group so you’ll have administrative privileges::

        usermod -a -G sudo exampleuser

    With your new user assigned, log out of your Linode as root::

        logout

    Log back in to your Linode as your new user. Replace exampleuser with your username, and the example IP address with your Linode’s IP address::

        ssh exampleuser@123.456.78.90

Now you can administer your Linode with the new user account instead of ``root``. When you need to execute superuser commands in the future, preface them with ``sudo``. For example, later in this guide you’ll execute ``sudo iptables -L`` while logged in with your new account. Nearly all superuser commands can be executed with ``sudo``, and all commands executed with ``sudo`` will be logged to ``/var/log/auth.log``.
