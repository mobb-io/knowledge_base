Configuring a Firewall
======================

Using a firewall to block unwanted inbound traffic to your Linode is a highly effective security layer. By being very specific about the traffic you allow in, you can prevent intrusions and network mapping from outside your LAN. A best practice is to allow only the traffic you need, and deny everything else.

``iptables`` is the controller for netfilter, the Linux kernel’s packet filtering framework. iptables is included in most Linux distros by default but is considered an advanced method of firewall control. Consequently, several projects exist to control iptables in a more user friendly way.

``FirewallD`` for the Fedora distro family and ``ufw`` for the Debian family are the two common iptables controllers. This section will focus on iptables but you can see our guides on ``FirewallD`` and ``ufw`` if you feel they may be a better choice for you.

View Your Current iptables Rules
--------------------------------

IPv4::

    sudo iptables -L

IPv6::

    sudo ip6tables -L

By default, iptables has no rules set for both IPv4 and IPv6. As a result, on a newly created Linode you will see what is shown below–three empty chains without any firewall rules. This means that all incoming, forwarded and outgoing traffic is allowed. It’s important to limit inbound and forwarded traffic to only what’s necessary.

.. code:: bash

    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination

Basic iptables Rulesets for IPv4 and IPv6
-----------------------------------------

Appropriate firewall rules heavily depend on the services being run. Below is an iptables ruleset to secure your Linode if you’re running a web server. This is given as an example! A real production web server may want or require more or less configuration and these rules would not be appropriate for a file or database server, Minecraft or VPN server, etc.

iptables rules can always be modified or reset later, but this basic ruleset serves only as a beginning demonstration. Outbound traffic is unregulated. All traffic forwarding is rejected and inbound traffic is limited to SSH, HTTP, HTTPS and ICMP type 8 (pings). Rejected packets are logged.

**/tmp/ipv4**

.. code:: sh

    *filter

    # Allow all loopback (lo0) traffic
    # and reject traffic to 127/8 that doesn't use lo0.
    -A INPUT -i lo -j ACCEPT
    -A INPUT -d 127.0.0.0/8 -j REJECT

    # Allow ping and traceroute.
    -A INPUT -p icmp --icmp-type 3 -j ACCEPT
    -A INPUT -p icmp --icmp-type 8 -j ACCEPT
    -A INPUT -p icmp --icmp-type 11 -j ACCEPT

    # Allow SSH connections.
    # The -dport number should be the same port number you set in sshd_config.
    -A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT

    # Allow HTTP and HTTPS connections from anywhere
    # (the normal ports for web servers).
    -A INPUT -p tcp --dport 80 -j ACCEPT
    -A INPUT -p tcp --dport 443 -j ACCEPT

    # Accept inbound traffic from established connections.
    -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

    # Log what was incoming but denied (optional but useful).
    -A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables_INPUT_denied: " --log-level 7

    # Reject all other inbound.
    -A INPUT -j REJECT

    # Log any traffic which was sent to you
    # for forwarding (optional but useful).
    -A FORWARD -m limit --limit 5/min -j LOG --log-prefix "iptables_FORWARD_denied: " --log-level 7

    # Reject all traffic forwarding.
    -A FORWARD -j REJECT

    COMMIT

**Optional:** If you plan on using the Linode Longview service, add this additional rule above the ``# Log what was incoming but denied`` section:

.. code:: sh

    # Allow incoming Longview connections
    -A INPUT -s longview.linode.com -j ACCEPT

The rules above in ``/tmp/ipv4`` can be used for IPv6, too; although IPv6 generally needs more ICMP capabilities than just echo requests. However, since IPv6 is not usually used on a webserver, we’ll reject all of it. If you intend to use your Linode’s IPv6 address, you would not want to do this.

Create a separate file for your IPv6 rules:


**/tmp/ipv6**


.. code:: sh

    *filter

    # Reject all ipv6 traffic on all chains.
    -A INPUT -j REJECT
    -A FORWARD -j REJECT
    -A OUTPUT -j REJECT

    COMMIT

How these IPv4 and IPv6 rules are deployed differs among the various Linux distros.

    * Ubuntu 14.04

    ufw is the iptables controller included with Ubuntu but is also available in Debian’s repositories. If you would prefer to use ufw instead of ipables, see our ufw guide to get a ruleset up and running.


    1. Create the files ``/tmp/ipv4`` and ``/tmp/ipv6``. Paste the above rulesets into their respective files.

    2. Import the rulesets into immediate use.

    .. code:: sh

        sudo iptables-restore < /tmp/ipv4
        sudo ip6tables-restore < /tmp/ipv6

    3. iptables-persistent automates loading iptables rules on boot for Debian and Ubuntu. Install it from the distro repositories.

    .. code:: sh

        sudo apt-get install iptables-persistent

    4. You’ll be asked if you want to save the current IPv4 and IPv6 rules. Answer ``yes`` to each prompt.

    5. Remove the temporary rule files.

    .. code:: sh

        sudo rm /tmp/{ipv4,ipv6}

Verify iptables Rulesets
------------------------

Recheck your Linode’s firewall rules:

.. code:: sh

    sudo iptables -L
    sudo ip6tables -L

The output should show for IPv4 rules:

.. code:: sh

    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination
    ACCEPT     all  --  anywhere             anywhere
    REJECT     all  --  anywhere             loopback/8           reject-with icmp-port-unreachable
    ACCEPT     icmp --  anywhere             anywhere             icmp destination-unreachable
    ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
    ACCEPT     icmp --  anywhere             anywhere             icmp time-exceeded
    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https
    ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
    LOG        all  --  anywhere             anywhere             limit: avg 5/min burst 5 LOG level debug prefix "iptables_INPUT_denied: "
    REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination
    LOG        all  --  anywhere             anywhere             limit: avg 5/min burst 5 LOG level debug prefix "iptables_FORWARD_denied: "
    REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination

Output for IPv6 rules will look like this:

.. code:: sh

    Chain INPUT (policy ACCEPT)
    target     prot opt source               destination
    REJECT     all      anywhere             anywhere             reject-with icmp6-port-unreachable

    Chain FORWARD (policy ACCEPT)
    target     prot opt source               destination
    REJECT     all      anywhere             anywhere             reject-with icmp6-port-unreachable

    Chain OUTPUT (policy ACCEPT)
    target     prot opt source               destination
    REJECT     all      anywhere             anywhere             reject-with icmp6-port-unreachable

Your firewall rules are now in place and protecting your Linode. Remember, you may need to edit these rules later if you install other packages which require network access.

Installing and Configuring Fail2Ban
-----------------------------------

Fail2Ban is an application that prevents dictionary attacks on your server. When Fail2Ban detects multiple failed login attempts from the same IP address, it creates temporary firewall rules that block traffic from the attacker’s IP address. Attempted logins can be monitored on a variety of protocols, including SSH, HTTP, and SMTP. By default, Fail2Ban monitors SSH only.

Here’s how to install and configure Fail2Ban:

1. Install Fail2Ban by entering the following command:

    * Ubuntu 14.04

    .. code:: sh

        sudo apt-get install fail2ban

2. Optionally, you can override the default Fail2Ban configuration by creating a new jail.local file. Enter the following command to create the file:

.. code:: sh

    sudo vi /etc/fail2ban/jail.local

.. note::

    To learn more about Fail2Ban configuration options, see this article on the Fail2Ban website.

3. Set the bantime variable to specify how long (in seconds) bans should last.


4. Set the maxretry variable to specify the default number of tries a connection may be attempted before an attacker’s IP address is banned.


5. Press ``<ESC>`` type ``:wq!`` and then press ``<ENTER>`` to save the changes to the Fail2Ban configuration file.


6. Then restart Fail2Ban:

If you’re using a distribution which uses systemd:

.. code:: sh

    sudo systemctl restart fail2ban


If your init system is SystemV or Upstart:

.. code:: sh

    sudo service fail2ban restart


Fail2Ban is now installed and running on your Linode. It will monitor your log files for failed login attempts. After an IP address has exceeded the maximum number of authentication attempts, it will be blocked at the network level and the event will be logged in /var/log/fail2ban.log.

