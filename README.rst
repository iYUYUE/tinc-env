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

Configuration tree

::

    /etc/tinc/bsodvpn/
    ├── hosts
    │   ├── host1
    │   ├── host2
    │   └── yournodename
    ├── rsa_key.priv
    ├── subnet-down
    ├── subnet-up
    ├── tinc.conf
    ├── tinc-down
    └── tinc-up

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

    #ConnectTo = <host1> # use node names for connectto
    #ConnectTo = <host2>

Create keys for node

::

    tincd -n bsodvpn -K

Create some hosts files (and exchange some host files with other people)

::

    Address = tinc.example.com
    Subnet = 172.16.1.1/32
    Subnet = 192.168.1.0/24

    -----BEGIN RSA PUBLIC KEY-----
    ...
    -----END RSA PUBLIC KEY-----

SystemD Service Unit

::

    [Unit]
    Description=Tinc net %i
    PartOf=tinc.service
    ReloadPropagatedFrom=tinc.service

    [Service]
    Type=simple
    WorkingDirectory=/etc/tinc/%i
    ExecStart=/usr/sbin/tincd -n %i -D
    ExecReload=/usr/sbin/tincd -n %i -kHUP
    ExecStop=/usr/sbin/tincd -n %i -k
    TimeoutStopSec=5
    Restart=always
    RestartSec=60

    [Install]
    WantedBy=tinc.service
