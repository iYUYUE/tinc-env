===================
TincVPN environment
===================


Getting started
===============

First, install tinc

::

    apt-get install tinc
    pacman -S tinc
    yum install tinc

Clone this repository

::

    mkdir /etc/tinc
    git clone git://github.com/2-B/tinc-env.git /etc/tinc/bsodvpn
    mkdir /etc/tinc/bsodvpn/hosts

Create the main tinc configuration ``/etc/tinc/bsodvpn/tinc.conf``

::

    Name = <your node name>
    AddressFamily = any
    DecrementTTL = yes
    Forwarding = internal

    #GraphDumpFile = |/usr/bin/dot -Tsvg -o /var/www/tinc-graph.svg

    #ConnectTo = <host1>
    #ConnectTo = <host2>

Create keys for node

::

    tincd -n bsodvpn -K

Create some hosts files

::

    Address = tinc.example.com
    Subnet = 172.16.1.1/32
    Subnet = 192.168.1.0/24

    -----BEGIN RSA PUBLIC KEY-----
    ...
    -----END RSA PUBLIC KEY-----

