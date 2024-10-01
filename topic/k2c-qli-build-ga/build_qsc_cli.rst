.. _sync_build_flash:

Use QSC CLI
------------------

This section explains how to configure, download, compile, and flash Qualcomm Linux using the QSC CLI.

.. _section_ez3_31x_v1c:

Download
^^^^^^^^^^^^^^

-  To download both proprietary and open source software from different sources, specify the input parameters --workspace-path <workspace path>, --product <Product_ID>, --distribution <Distribution>, and --release <release>. For example:

   ::

      qsc-cli download --workspace-path '<absolute_workspace_path>' --product '<Product_ID>' --distribution '<Distribution>' --release '<Release_ID>'
      # Example, qsc-cli download --workspace-path '/local/mnt/workspace/sample_workspace' --product 'QCM6490.LE.1.0' --distribution 'Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem' --release 'r00263.4'

   .. note::
      - For the Product_ID, Distribution, and Release_ID values, see the **QSC-CLI Input Parameters** table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
      - For more information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#id7>`__.
      - If you are downloading more than one distribution, a new workspace is required for each distribution or create a new workspace for each distribution that you download.

.. _section_yhy_11w_q1c_vinayjk_03-07-24-006-28-270:

Build default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _compile_qsc_cli:

Compile
''''''''''

.. note:: For information on the default configurations, see the *Applicable MACHINE, DISTRO and QCOM_SELECTED_BSP with defaults for QSC* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

Start compilation after the download is complete:

.. note:: Depending on the size of the software and host machine configuration, the compilation process may take a few hours.

::

   qsc-cli compile --workspace-path '<absolute_workspace_path>'
    
   # Example
   qsc-cli compile --workspace-path '/local/mnt/workspace/sample_workspace'

This process builds the necessary Qualcomm firmware and also completes the Qualcomm Linux build.

.. note:: If you encounter a BitBake fetcher error, try recompiling to resolve the issue. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

.. _section_qb2_sp1_q1c_vinayjk_03-04-24-129-15-322:

Recompile
'''''''''''

Recompile your workspace if you already have a workspace built using QSC CLI:

::

   # qsc-cli compile --image '<software_image_name>' --workspace-path '<absolute_workspace_path>'
    
   # Example
   qsc-cli compile --image LE.QCLINUX.1.0.r1 --workspace-path '/local/mnt/workspace/sample_workspace'

.. note:: 
    For information on software image names (``--image``), see *QSC-CLI Input Parameters* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

.. _section_x2k_vnf_w1c:

Flash
'''''''''

.. note:: For the QSC-CLI to detect the connected devices and flash the software builds, ensure that the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) are installed on the host machine. Install PCAT and QUD using ``qpm-cli``:

::

  qpm-cli --login
  qpm-cli --install pcat --activate-default-license
  qpm-cli --install qud --activate-default-license

The ``qpm-cli --help`` command lists the help options.

When installing QUD on Ubuntu 22.04, you might have to enroll the public key on your Linux host to complete the installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QUIC/sign/`` directory.

.. note::
   - If your reference kit matches the :ref:`configuration compiled <compile_qsc_cli>`, then continue with the following steps. Else, skip to :ref:`build your own configuration <build_own_config>`.
   - Before you flash the software, ensure that the device is in Emergency Download (EDL) mode. For more information on how to force the device into EDL mode, see :ref:`Move to EDL mode <section_vgg_mly_v1c>`.
  
1. Flash a device.

   ::

      qsc-cli flash --workspace-path <workspace path> --buildflavor "sa2150p_emmc" --serial <serial number>

   The ``--buildflavor`` argument is optional and only required for devices that have multiple flavors. To list the build flavors, run the following command:
      
   ::

      qsc-cli flash --workspace-path <workspace path> --list-buildflavor

   .. note::
      - To know the `<serial number>`, run the following command:

        ::
      
          pcat -devices

        **Sample output**
        .. container:: screenoutput

            Searching devices in Device Manager, please wait for a moment…
            ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
            NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

      - To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.

.. _build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. For optional advanced customization, open a build environment for a workspace and build the software image:

   ::

      qsc-cli open-build-env --workspace-path <absolute_workspace_path> --image <software image>

   .. note::
      - This will set up an environment for you to execute your own build commands for a given software image. QSC will not track the status of input workspaces in the future releases and flash via ``qsc-cli`` will not be supported for these workspaces.
      - For information on the supported configurations, see the *Applicable MACHINE, DISTRO and QCOM_SELECTED_BSP with defaults for QSC* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

2. Update the highlighted command with your configuration and run it on the terminal:

   .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

   For example, to build for Qualcomm® RB3 Gen 2 Vision Development Kit, update the ``MACHINE`` in the default build command to ``qcs6490-rb3gen2-vision-kit``.

3. After a successful build, check that the ``system.img`` file is in the ``<Base Workspace Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory. For example:

   ::

      cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      ls -al system.img

Flash your own configuration
-------------------------------

1. Check if ``QTI_HS-USB_QDLoader`` driver is available in the installed directory:

   ::

      ls –la /dev/Q*

   **Sample output**

   .. container:: screenoutput

       crw-rw-rw- 1 root 242 0 Dec 10 10:51 /dev/QTI_HS-USB_QDLoader_9008_3-8:1.0

2. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   .. container:: screenoutput

       Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

3. Check if the device is recognized by PCAT:

   ::

      pcat -devices

   **Sample output**

   .. container:: screenoutput

       Searching devices in Device Manager, please wait for a moment…
       ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
       NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

4. Download the build:

   ::

      PCAT –PLUGIN SD -DEVICE <device_serial_number> -BUILD “<build_images_path>” -MEMORYTYPE UFS -FLAVOR asic

      # Example
      PCAT -PLUGIN SD -DEVICE be116704 -BUILD "<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image" -MEMORYTYPE UFS -FLAVOR asic

   If the software has been flashed successfully, you see the following message:

   .. container:: screenoutput

       xxxx INFO] [ FIRMWARE DOWNLOAD LOG ] Process Finished                                                  
       xxxx INFO] Status   - TRUE
       xxxx INFO] Response - Downloading software images completed on the device Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

   After a successful flashing operation, run the ``lsusb`` command to see the device information on the terminal window as shown in line 4 of the following message:

   .. container:: screenoutput

       ThinkPad-T490s:<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image$ lsusb
       Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
       Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
       Bus 002 Device 006: ID 05c6:901d Qualcomm, Inc. QCM6490_fd2913cf
       Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

.. note::

   -  To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.
   -  If the software has been flashed successfully, the device reboots. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.