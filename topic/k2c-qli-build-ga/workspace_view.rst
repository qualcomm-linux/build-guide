.. _troubleshoot_howto_annex:

Workspace view
-----------------

This section provides sample workspace structures with ``qsc-cli`` and GitHub workflow standalone use cases for QCS6490 and QCS5430.

.. _section_bsj_xhm_q1c_vinayjk_03-05-24-1606-7-713:

Workspace structure with qsc-cli
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  The directory structure before the
   ``Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK`` distribution build
   is shown in the following figure (``LE.QCLINUX.1.0.r1`` contains the
   Yocto workspace):

         .. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_1.png

-  The directory structure after
   ``Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK`` distribution build
   is shown in the following figure:

         .. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_2.png

-  The directory structure before
   ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build
   with firmware and extras is shown in the following figure:

         .. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_3.png

-  The directory structure after
   ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build
   with firmware and extras is shown in the following figure:

        .. image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_4.png

.. _section_ibb_xkc_t1c_vinayjk_03-15-24-054-26-366:

Workspace structure with GitHub workflow standalone instructions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  The directory structure before GitHub workflow QIMP SDK standalone build
   is shown in the following figure:

        .. image:: ../../media/k2c-qli-build-ga/ws_standalone_1.png

-  The directory structure after GitHub workflow QIMP SDK standalone build
   is shown in the following figure:

        .. image:: ../../media/k2c-qli-build-ga/ws_standalone_2.png

-  The directory structure after building firmware of
   ``qualcomm-linux-spf-1-0_ap_standard_oem_nomodem`` is shown in the
   following figure:

   .. note::
      - ``qualcomm-linux-spf-1-0_ap_standard_oem_nomodem`` contains the downloaded select firmware sources.
      - ``LE.QCLINUX.1.0.r1`` contains the built Yocto workspace. 
   
   |ws_standalone_3|

-  The directory structure after building firmware of
   ``qualcomm-linux-spf-1-0_amss_standard_oem_nomodem`` is shown in the
   following figure:

   .. note::
      - ``qualcomm-linux-spf-1-0_amss_standard_oem_nomodem`` contains the downloaded select firmware sources.
      - ``LE.QCLINUX.1.0.r1`` contains the built Yocto workspace. 
  
   |ws_standalone_4|

.. _section_pg4_nzt_vbc_vinayjk_06-25-24-1332-59-291:

Images directory structure after successful build
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  The following figure shows the Images directory after a successful build:

   .. image:: ../../media/k2c-qli-build-ga/ws_standalone_5.png

   The files in the images directory are described in the following table:

   +-----------------------------+----------------------------------------+
   | Filename                    | Description                            |
   +=============================+========================================+
   | ``.mbn`` and ``*.elf``      | Boot critical images                   |
   +-----------------------------+----------------------------------------+
   | ``gpt_main*.bin``           | GUID partition table binaries for the  |
   |                             | primary partition table                |
   +-----------------------------+----------------------------------------+
   | ``gpt_backup*.bin``         | GUID partition table binaries for the  |
   |                             | secondary partition table              |
   +-----------------------------+----------------------------------------+
   | ``system.img``              | Rootfs image                           |
   +-----------------------------+----------------------------------------+
   | ``rawprogram*.xml``         | Image ``lun`` and start sector ``lba`` |
   |                             | values                                 |
   +-----------------------------+----------------------------------------+
   | ``efi.bin``                 | EFI system partition image. For more   |
   |                             | information, see `Qualcomm Linux Yocto |
   |                             | Guide <ht                              |
   |                             | tps://docs.qualcomm.com/bundle/publicr |
   |                             | esource/topics/80-70015-27/platform_so |
   |                             | ftware_features.html#partitioning>`__. |
   +-----------------------------+----------------------------------------+
   | ``qdl``                     | Flashing tool binary                   |
   +-----------------------------+----------------------------------------+
   | ``dtb.bin``                 | Binary image that bundles all DTB      |
   |                             | binaries generated during build        |
   +-----------------------------+----------------------------------------+
   | ``vmlinux``                 | Compile kernel ``elf`` binary          |
   +-----------------------------+----------------------------------------+
   | ``Image``                   | Linux kernel ARM64 boot executable     |
   |                             | image                                  |
   +-----------------------------+----------------------------------------+






.. |ws_standalone_3| image:: ../../media/k2c-qli-build-ga/ws_standalone_3.png
.. |ws_standalone_4| image:: ../../media/k2c-qli-build-ga/ws_standalone_4.png

