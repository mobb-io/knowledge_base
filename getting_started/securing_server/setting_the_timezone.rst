Setting the Timezone
====================

By default, a Linode’s Linux image will be set to UTC time (also known as Greenwich Mean Time) but this can be changed. It may be better to use the same timezone which a majority of your users are located in, or that you live in to make log file timestamps more sensible.

* Ubuntu 14.04

    dpkg-reconfigure

    .. code:: bash

        #dpkg-reconfigure tzdata
        echo "America/Sao_Paulo" > /etc/timezone
        dpkg-reconfigure -f noninteractive tzdata

    View the current date and time according to your server.

    .. code:: bash

        date

    The output should look similar to: Mon Oct  5 03:37:10 BRT 2015.