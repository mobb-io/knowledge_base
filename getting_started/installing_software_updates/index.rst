Installing Software Updates
===========================

The first thing you should do when connecting to your Linode is update the Linux distribution’s software. This applies the latest security patches and bug fixes to help protect your Linode against unauthorized access.

Installing software updates should be performed *regularly*. If you need help remembering, try creating a monthly alert with the calendar application on your desktop computer.

* Ubuntu 14.04

.. code:: bash

    apt-get update
    apt-get upgrade
    apt-get dist-upgrade


Distribution Supplied Kernel on Linode with KVM
-----------------------------------------------

This is useful if you’d like to enable specific kernel features.

.. toctree::
    :maxdepth: 2

    run_a_distribution_supplied_kernel_on_a_kvm_linode
