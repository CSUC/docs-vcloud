.. vCloud Driver Documentation documentation master file, created by
   sphinx-quickstart on Sat Jul 15 13:11:30 2017.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Welcome to addon vCloud Driver documentation!
=======================================================

.. toctree::
   :maxdepth: 1

   Release Notes <release_notes/index>
   Compatibility Guide <compatibility/index>
   vCloud Driver Installation <vcloud_installation/index>
   vCloud Driver Usage <vcloud_usage/index>


Introduction
==================
This addon gives Opennebula the posibility to manage resources in VMware vCloud Director infraestructures. It includes virtualization and monitoring drivers.

.. image:: /images/one_vcloud.png
    :width: 45%
    :align: center

========
Features
========

This addon has the following capabilities:

* Deploy, stop, shutdown, reboot, save, suspend, resume and delete VM's in the Virtual Data Centers hosted in vCloud.
* Create, delete and revert snapshots of VM's.
* Change RAM and CPU values of VM.
* It's able to hot-attach and detach NICs to VM's.
* Attach disks to VM, only in cold. To avoid accidental disk remove, the detach disk option is disabled in OpenNebula. If you want enable this option, see Guide section.
* Resize disks during the instantiation. Only increase the size, not decrease.
* Automatized customization of the VMs instanciated.
* It's able to choose wich storage profile will be storaged the VM's disk or disks.
* Port filtering via vShield during the VM instanciation.
* Obtain monitoring information from the VDC, Datastore and VM's.
* In this development version we manage vApps with one VMs inside (A VM in OpenNebula equals a vApp with one VM in vCloud).
* Each Virtual Data Center (VDC) in vCloud is managed as a Host in OpenNebula.
* Import networks, hosts, templates and datastores hosted in vCloud using onevcloud script.


===========
Demo
===========
In this video you can see how to instantiate a new imported vm template

.. image:: /images/vcloud_demo.gif
    :width: 100%
    :align: center

===========
Development
===========
To contribute bug patches or new features, you can use the github 
Pull Request model. It is assumed that code and documentation are 
contributed under the Apache License 2.0. 

=========
More info 
=========
* `How to Contribute <http://opennebula.org/software:addons#how_to_contribute_to_an_existing_add-on>`_
* Support: `OpenNebula user forum <https://forum.opennebula.org/c/support>`_
* Development: `OpenNebula developers forum <https://forum.opennebula.org/c/development>`_
* Issues Tracking: `Github issues <https://github.com/OpenNebula/addon-vcloud-driver/issues>`_

============
Authors
============

- **Cloud Systems Developer:** Arnau Colominas (arnau.colominas@csuc.cat)
- **Cloud Systems Engineer:**  Miguel Ángel Flores (miguel.angel.flores@csuc.cat)
- **Cloud Systems Engineer:** Xavier Peralta (xavier.peralta@csuc.cat)

