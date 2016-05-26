============================
ZenPacks.zenoss.RabbitMQ
============================

Description
===========

Forked from githug.com/zenoss, version 1.0.8.  See http://wiki.zenoss.org/ZenPack:RabbitMQ 

This version specifically customised to monitor RabbitMQ on Zenoss 5.x servers.


Features
========

Zenoss Device Classes
---------------------

None

Device and component object classes
-----------------------------------

Components for:

* RabbitMQNode
* RabbitMQVHost
* RabbitMQExchange
* RabbitMQQueue


New relationship is created directly on Device by __init__.py for rabbitmq_nodes.

Object files then declare relationships for:
* RabbitMQNode has many RabbitMQVhost
* RabbitMQVhost has many RabbitMQExchange
* RabbitMQVhost has many RabbitMQQueue

Properties
----------

New properties decalred in __init__.py for:
* zRabbitMQAdminUser
* zRabbitMQAdminPassword

Defaults on both are RabbitMQ.


Modeler Plugins
---------------

zenoss.ssh.RabbitMQ

This modeler is modified from the Zenoss-supplied version to run against the host Zenoss server
and to preface Rabbit commands with 

serviced service attach RabbitMQ su - root -c

And the Rabbit commands must then be in double quotes, on one line with $ escaped.

Monitoring Templates
--------------------

* Device templates
   
  - no device templates are shipped

* Component templates

  - Zenoss-supplied version has templates for RabbitMQNode and RabbitMQQueue at /Device.
  - This version supplies same templates at /Server/Linux with same serviced changes for all
    datasources except RabbitMQRates, which is disabled to avoid needing pre-reqs for rabbitmqadmin command.

Datasources
-----------

None.

Events
------

* /Status/RabbitMQ
* /Perf/RabbitMQ


GUI modifications
-----------------

This version has juggled column field sizes slightly on Queues to get more space for queue name.


Command Parsers
---------------

* ZenPacks.zenoss.RabbitMQ.parsers.RabbitMQCTL
* ZenPacks.zenoss.RabbitMQ.parsers.RabbitMQAdmin



Usage
=====


Original Zenoss ZenPack
-----------------------

To start monitoring your RabbitMQ server you will need to setup SSH access so that your Zenoss collector server 
will be able to SSH into your RabbitMQ server(s) as a user who has permission to run the rabbitmqctl command. 
This almost always means the root user. See the Using a Non-Root User section below for instructions on 
allowing non-root users to run rabbitmqctl. To do this you need to set the following zProperties for the 
RabbitMQ devices or their device class in Zenoss.

* zCommandUsername
* zCommandPassword
* zKeyPath

The zCommandUsername property must be set. To use public key authentication you must verify that the 
public portion of the key referenced in zKeyPath is installed in the `~/.ssh/authorized_keys` file 
for the appropriate user on the RabbitMQ server. If this key has a passphrase you should set it in the 
zCommandPassword property. If you'd rather use password authentication than setup keys, simply put 
the user's password in the zCommandPassword property.

You should then add the zenoss.ssh.RabbitMQ modeler plugin to the device, or device class containing 
your RabbitMQ servers and remodel the device(s). This will automatically find the node, vhosts, exchanges 
and queues and begin monitoring them immediately for the following metrics. 


This version of the ZenPack for Zenoss 5.x
------------------------------------------

It is assumed that the host Zenoss server is discovered as a device under the /Server/Linux class hierarchy.
It is also assumed that a zenoss user id has been setup on the base host, in accordance with the instructions
found here - http://zenpacklib.zenoss.com/en/latest/development-environment-5.html .  The zenoss user on the base
host must be able to run serviced commands and sudo, as documented in this URL.

Both modeler and command datasources have been modified in this version of the ZenPack so that they use 
serviced to access the RabbitMQ virtual host, as the root user, and then run the rabbitmqctl commands.

The RabbitMQRates datasource requires installation of rabbitmqadmin with additional prerequisites on the
underlying Erlang package.  To avoid these requirements, the RabbitMQRates datasource is configured as
disabled in this version of the ZenPack.

Requirements & Dependencies
===========================

* Zenoss Versions Supported:  
  - Original Zenoss version: 3.x, 4.x
  - This version: 5.x - do not install on V3 or V4
* External Dependencies: 



Download
========
Download the appropriate package for your Zenoss version from the list
below.

* Zenoss 4.0+ and 5.x  `Latest Package for Python 2.7`_

ZenPack installation
======================

This ZenPack can be installed from the .egg file using either the GUI or the
zenpack command line. 

To install in development mode, find the repository on github and use the *Download ZIP* button
(right-hand margin) to download a tgz file and unpack it to a local directory, say,
/code/ZenPacks .  Install from /code/ZenPacks with::
  zenpack --link --install ZenPacks.zenoss.RabbitMQ
  Restart zenoss after installation.


Limitations and Troubleshooting
===============================



Change History
==============
* 1.0.8
   - Latest Zenoss release
* 1.5.1jc
   - Designed to monitor Rabbit queues on Zenoss 5.x 
   - Modeler plugin and datasources modified to call serviced


Screenshots
===========

See the screenshots directory.


.. External References Below. Nothing Below This Line Should Be Rendered

.. _Latest Package for Python 2.7: https://github.com/jcurry/ZenPacks.zenoss.RabbitMQ/blob/5branch/dist/ZenPacks.zenoss.RabbitMQ-1.5.1jc-py2.7.egg?raw=true

Acknowledgements
================


