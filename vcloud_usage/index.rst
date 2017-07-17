================================================================================
Usage vCloud Driver for OpenNebula 
================================================================================

.. toctree::
   :maxdepth: 2

First Steps
===========

The first step is import to OpenNebula the resources hosted in vCloud using the onevcloud script.

**Import Hosts,Templates, Networks and Datastore using ``onevcloud`` script**

When vCloud Driver is installed correctly, it's time to use onevcloud script to import the resources of vCloud.

.. code-block:: bash

  /var/lib/one/remotes/vmm/vcloud/onevcloud [hosts | templates | networks | datastores] --vcloud [vcloud-uri.com] --vuser [user@organization] --vpass [password] --vdc [VDC_name]

Hosts
=====

.. code-block:: bash

  /var/lib/one/remotes/vmm/vcloud/onevcloud hosts --vcloud [vcloud-uri.com] --vuser [user@organization] --vpass [password] --vdc [VDC_name]
  

.. image:: /images/vcloud_host.png
    :width: 100%
    :align: center

Templates
==========
  
.. code-block:: bash
    
    /var/lib/one/remotes/vmm/vcloud/onevcloud hosts --vcloud [vcloud-uri.com] --vuser [user@organization] --vpass [password] --vdc [VDC_name]

.. image:: /images/vcloud_templates.png
    :width: 100%
    :align: center

Networks
=========

For the current version, only POOL addressment is supported. OpenNebula generates MAC addresses and passes them to vCloud. 
For that reason, when you use the script to import networks to OpenNebula you must to choose [E]thernet type network.

.. code-block:: bash 
    
    /var/lib/one/remotes/vmm/vcloud/onevcloud networks --vcloud [vcloud-uri.com] --vuser [user@organization] --vpass [password] --vdc [VDC_name]

.. image:: /images/vcloud_networks.png
    :width: 100%
    :align: center

Intanciate Templates
=====================

When the resources are imported correctly, we can start to instanciate templates!

.. image:: /images/vcloud_vm.png
    :width: 100%
    :align: center

.. image:: /images/vcloud_vm2.png
    :width: 100%
    :align: center

Templates Context Variables
============================

During the instanciation, vcloud driver takes some variables from the template to configure capabilities such as CUSTOMIZATION. If this variables are not in the template, the capabilities won't be configured correctly. Please add or modify the variables that you need.

* CUSTOMIZATION = "YES"                     The DEFAULT value is NO. If you want to activate customization, change the value to "YES".
* OS = "LINUX | WINDOWS | OTHER"            Check this variable and modify the value for the OS of the template. This value is important because is used during the customization process.
* WHITE_TCP_PORTS = "PORT,PORT,..."         If you add coma-separated TCP port numbers, during the instanciation the driver will open that ports in the vShield. Only if the network is enrouted by vShield.
* STORAGE_PROFILE = "STORAGE_PROFILE_NAME"  You can select the storage profile. If you don't select anyone, the vm will be storaged in the default storage profile defined in vcloud vapp template.
 
Users Context Variables
========================

If you activate the customization and the template supports it, during the instaciation a user will be created. This variables must be set in the "User Settings" tab in Sunstone.

* PASS      [Hash]        This information will be the password for your user in LINUX OS      
* PASS_WIN  [Clear Text]  This information will be the password for your user in WINDOWS OS   
* ROOT_PASS [Clear Text]  This information will be the password for the root/Administrator in LINUX/WINDOWS OS

If you add a Public SSH Key for that user, it will be added to the Linux VM during its instanciation.

Attach Disks
=============

To attach disk you must indicate the options disk in the VM template

.. code-block:: bash

  DISK = [
    DISK_ID = "1",
    SIZE = "51200",
    TYPE = "fs" 
  ]

DISK_ID = Specify a id to this disk, each disk id must be different.
SIZE = The disk size in MB
TYPE = Indicate "fs"

Detach Disks
=============

The detach disk option is disabled. OpenNebula can attach disk to vCloud vApp, but can not detach disks. This option is disabled to avoid accidentally removes.
If you want to enable de detach option, you can activate it uncommenting some lines in the ruby_vcloud_sdk library.

Open ``/var/lib/gems/1.9.1/gems/ruby_vcloud_sdk-[last_version]/lib/ruby_vcloud_sdk/vm.rb`` and go to line 254. Remove the =begin and =end clauses. Save it.

Now if you remove a disk in OpenNebula, the changes will take effect in vCloud vApp associated.

