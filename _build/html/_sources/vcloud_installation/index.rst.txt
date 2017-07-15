================================================================================
Install vCloud Driver for OpenNebula 
================================================================================

.. toctree::
   :maxdepth: 2


Step 1. Download and copy the files in their respective directories:
====================================================================
    - 5.0/remotes/vmm/vcloud 				  	-> /var/lib/one/remotes/vmm/
    - 5.0/remotes/im/vloud.d 					-> /var/lib/one/remotes/im/
    - 5.0/remotes/datastore/vcloud 				-> /var/lib/one/datastore
    - 5.0/remotes/hooks/dv            -> /var/lib/one/remotes/hooks
    - 5.0/remotes/vmm/vcloud/vcloud_driver.rb 	-> /usr/lib/one/ruby

Step 2. Change permissions and owner:
=====================================

.. code-block:: bash

   chown -R oneadmin:oneadmin /var/lib/one/remotes/vmm/vcloud /var/lib/one/remotes/im/vcloud.d /var/lib/one/remotes/datastore/vcloud /var/lib/one/remotes/hooks/dv

   chmod -R +x /var/lib/one/remotes/vmm/vcloud /var/lib/one/remotes/im/vcloud.d /var/lib/one/remotes/datastore/vcloud /var/lib/one/remotes/hooks/dv

Step 3. Modify the ``/etc/one/oned.conf`` configuration file adding the following new lines
=======================================================================================

* In IM_MAD section:

.. code-block:: bash

 		 #-------------------------------------------------------------------------------
   		 #  VMware vCloud Director Information Driver Manager Configuration
  		 #-------------------------------------------------------------------------------
		 IM_MAD = [
      		name       = "vcloud",
      		executable = "one_im_sh",
      		arguments  = "-c -t 15 -r 0 vcloud"]
   		#-------------------------------------------------------------------------------

* In VM_MAD section:

.. code-block:: bash

         #-------------------------------------------------------------------------------
    	 #  VMware vCloud Director Virtualization Driver Manager Configuration
    	 #-------------------------------------------------------------------------------
     	 VM_MAD = [
    		name       = "vcloud",
    		executable = "one_vmm_sh",
    		arguments  = "-p -t 15 -r 0 vcloud -s sh",
    		type       = "xml" ]
 	 #------------------------------------------------------------------------------

* In HOOK section:
 
.. code-block:: bash
     
       #-------------------------------------------------------------------------------
       #   VMware vCloud Director Hook Manager Configuration
       #-------------------------------------------------------------------------------
       VM_HOOK = [
        name = "delete_poweroff_vms",
        on = "DONE",
        command = "dv/delete_poweroff_vms.rb",
        arguments = "$TEMPLATE" ]
       #-------------------------------------------------------------------------------
       VM_HOOK = [
        name      = "delete_failed_vms",
        on        = "CUSTOM",
        state     = "ACTIVE",
        lcm_state = "BOOT_FAILURE",
        command   = "dv/delete_failed_vms.rb",
        arguments = "$TEMPLATE" 
       ]
       #-------------------------------------------------------------------------------
      
* In TM_MAD section:

We need to add vcloud as a argument.This section must be like this:

.. code-block:: bash
    
    	TM_MAD = [
    		executable = "one_tm",
    		arguments = "-t 15 -d dummy,lvm,shared,vcloud,fs_lvm,qcow2,ssh,vmfs,ceph,dev"
		]

* In DS_MAD section:

We need to add vcloud as a argument.This section must be like this:

.. code-block:: bash
    
 		DATASTORE_MAD = [
    		executable = "one_datastore",
    		arguments  = "-t 15 -d dummy,vcloud,fs,vmfs,lvm,ceph,dev"
		]

* Add the following lines at the end of the file in the Transfer Manager Driver Behavior Configuration section:

.. code-block:: bash

    	TM_MAD_CONF = [
    		name = "vcloud", ln_target = "NONE", clone_target = "NONE", shared = "yes"
		]

		DS_MAD_CONF = [
    		NAME = "vcloud", REQUIRED_ATTRS = "VCLOUD_HOST", PERSISTENT_ONLY = "YES",
    		MARKETPLACE_ACTIONS = "export"
		] 

Step 4. Restart ONE service.
============================

.. code-block:: bash

 	service opennebula restart


