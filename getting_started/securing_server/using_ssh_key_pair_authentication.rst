Using SSH Key Pair Authentication
=================================

You’ve used password authentication to connect to your Linode via SSH, but there’s a more secure method available: key pair authentication. In this section, you’ll generate a public and private key pair using your desktop computer and then upload the public key to your Linode. SSH connections will be authenticated by matching the public key with the private key stored on your desktop computer - you won’t need to type your account password. When combined with the steps outlined later in this guide that disable password authentication entirely, key pair authentication can protect against brute-force password-cracking attacks.

Here’s how to use SSH key pair authentication to connect to your Linode:

1. Generate the SSH keys on a desktop computer running Linux or Mac OS X by entering the following command in a terminal window on your desktop computer. PuTTY users can generate the SSH keys by following the windows specific instructions in the Use Public Key Authentication with SSH Guide.

.. code:: bash

    ssh-keygen

2. The SSH keygen utility appears. Follow the on-screen instructions to create the SSH keys on your desktop computer. To use key pair authentication without a passphrase, press Enter when prompted for a passphrase.

.. note::

    Two files will be created in your \~/.ssh directory: ``id_rsa`` and ``id_rsa.pub``. The public key is ``id_rsa.pub``
    - this file will be uploaded to your Linode. The other file is your private key. Do not share this file with anyone!

3. Upload the public key to your Linode with the secure copy command (``scp``) by entering the following command in a terminal window on your desktop computer. Replace ``example_user`` with your username, and ``123.456.78.90`` with your Linode’s IP address. If you have a Windows desktop, you can use a third-party client like ``WinSCP`` to upload the file to your home directory.

.. code:: bash

    scp ~/.ssh/id_rsa.pub example_user@123.456.78.90:

4. Create a directory for the public key in your home directory (``/home/yourusername``) by entering the following command on your Linode:

.. code:: bash

    mkdir .ssh

5. Move the public key in to the directory you just created by entering the following command on your Linode:

.. code:: bash

    mv id_rsa.pub .ssh/authorized_keys

6. Modify the permissions on the public key by entering the following commands, one by one, on your Linode. Replace ``example_user`` with your username.

.. code:: bash

    chown -R example_user:example_user .ssh
    chmod 700 .ssh
    chmod 600 .ssh/authorized_keys

The SSH keys have been generated and the public key has been installed on your Linode. You’re ready to use SSH key pair authentication! To try it, log out of your terminal session and then log back in. The new session will be authenticated with the SSH keys and you won’t have to enter your account password. (You’ll still need to enter the passphrase for the key, if you specified one.)
