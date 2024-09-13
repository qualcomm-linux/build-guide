.. _host_machine_requirements_reg_unreg:

Host machine requirements
-------------------------------

.. tabularcolumns:: |p{3cm}|p{3cm}|p{3cm}|p{3cm}|p{3cm}|

.. flat-table::
   :header-rows: 2
   :class: longtable table-wrap

   * - :cspan:`2` Configuration
      
      
     - :rspan:`1` Tools
     - :rspan:`1` Permissions

   * - **Linux**
     - **Windows**
     - **Mac**      

   * - x86 machine
     - x86 machine
     - x86/Arm\ :sup:`®` machine
     - Git 1.8.3.1 or later versions
     - :rspan:`4` A ``sudo`` permission is required to execute a few commands.

   * - Quad-core CPU, for example, Intel i7-2600 at 3.4 GHz
       (equivalent or better)
     - 8-core CPU
     - 8-core CPU
     - Git 1.8.3.1 or later versions
      

   * - 300 GB free disk space (swap partition > 32 GB)
     - 300 GB free space for the VirtualBox VM
     - 300 GB free space for UTM
     - Python 3.10.2 or later versions
      

   * - 16 GB RAM
     - 8 GB RAM
     - 8 GB RAM
     - GCC 7.5 or later versions
      

   * - Ubuntu 22.04
     - Microsoft Windows 11 OS
     - Apple\ :sup:`®` Mac\ :sup:`®` OS 14
     - GNU Make 4.0 or later versions
      

.. note:: To set up a virtual machine (VM) running Ubuntu 22.04 OS on
          Microsoft Windows or Apple Mac, see `Qualcomm Linux Virtual Machine
          Setup Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-41/>`__.
          Code compilation on a VM is a slow process and may take a few hours.
          Qualcomm recommends using an Ubuntu host machine for compilation.