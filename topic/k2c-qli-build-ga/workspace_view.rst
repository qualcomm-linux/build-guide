Workspace view
-----------------

This section provides sample workspace structures with ``qsc-cli`` and source workflow standalone use cases, for the supported `hardware SoCs <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-115/soc.html>`__.

Workspace structure with qsc-cli
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following figure shows the directory structure before ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build with firmware and extras:

.. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_3.png

The following figure shows the directory structure after ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build with firmware and extras:

.. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_4.png

Workspace structure with Source workflow standalone instructions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following figure shows the directory structure after building firmware of ``qualcomm-linux-spf-1-0_ap_standard_oem_nomodem``:

.. note::
    - ``qualcomm-linux-spf-1-0_ap_standard_oem_nomodem`` contains the downloaded select firmware sources.
    - ``LE.QCLINUX.2.0`` has the built Yocto workspace. 
   
|ws_standalone_3|

The following figure shows the directory structure after building firmware of ``qualcomm-linux-spf-1-0_amss_standard_oem_nomodem``:

.. note::
    - ``qualcomm-linux-spf-1-0_amss_standard_oem_nomodem`` contains the downloaded select firmware sources.
    - ``LE.QCLINUX.2.0`` has the built Yocto workspace.
  
|ws_standalone_4|

Images directory structure after successful build
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The following figure shows the Images directory after a successful build:

.. image:: ../../media/k2c-qli-build-ga/ws_standalone_5.png

The following table describes the files in the images directory:
   
.. list-table:: 
   :header-rows: 1
   :align: center

   * - Filename
     - Description
   * - ``.mbn`` and ``*.elf``
     - Boot critical images
   * - ``gpt_main*.bin``
     - GUID partition table binaries for the primary partition table
   * - ``gpt_backup*.bin``
     - GUID partition table binaries for the secondary partition table
   * - ``rootfs.img``
     - Rootfs image
   * - ``rawprogram*.xml``
     - Image ``lun`` and start sector ``lba`` values
   * - ``efi.bin``
     - EFI system partition image. For more information, see `Qualcomm Linux Yocto Guide <https://https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/managing_partitions_in_qualcomm_linux.html#linux-operating-system-partitions>`__.
   * - ``qdl``
     - Flashing tool binary
   * - ``dtb.bin``
     - Binary image that bundles all DTB binaries generated during build
   * - ``vmlinux``
     - Compile kernel ``elf`` binary
   * - ``Image``
     - Linux kernel ARM64 boot executable image

.. |ws_standalone_3| image:: ../../media/k2c-qli-build-ga/ws_standalone_3.png
.. |ws_standalone_4| image:: ../../media/k2c-qli-build-ga/ws_standalone_4.png
