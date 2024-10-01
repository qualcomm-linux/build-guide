.. _concept_n2t_tjn_w1c:

Use QSC Launcher
-------------------------

Download
^^^^^^^^^^^^^^

1. To open the QSC Launcher desktop application, launch **Qualcomm Software Center** from the **Applications** menu.

   .. note:: 
       For the Launcher workflow to detect the connected devices and flash the software builds, ensure that the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) are installed on the host machine. Click **PCAT** to install PCAT and **QUD** to install QUD.

      .. image:: ../../media/k2c-qli-build-ga/QSC_has_PCAT_QUD_install_info.png

      **Or**

      Install PCAT and QUD using ``qpm-cli``:

      ::

         qpm-cli --login
         qpm-cli --install pcat --activate-default-license
         qpm-cli --install qud --activate-default-license

      The ``qpm-cli --help`` command lists the help options.

      When installing QUD on Ubuntu 22.04, you might have to enroll the public key on your Linux host to complete the installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QUIC/sign/`` directory.

2. Use your Qualcomm ID to log in to the QSC desktop application. A dashboard page appears.

   .. image:: ../../media/k2c-qli-build-ga/start_launcher_ab.png

   -  If you do not have a connected device, click **Start Launcher** (A) on the top panel.
   -  If you have a connected device, click **Start Launcher** (B) for the appropriate device in the **Connected devices** panel.

3. On the Specify Environment page, select the appropriate values for **Category**, **Chipset**, **Host Operating System**, and **Target Operating System**:

   .. note:: For more information on the supported chip products, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

  .. image:: ../../media/k2c-qli-build-ga/specify_env.png

4. Click **Next**.
   
   The **Select Resources** page appears.

   .. image:: ../../media/k2c-qli-build-ga/prebuilt_software_options.png

   a. In the **Base Workspace Path** text box, specify a directory where you want to download the software.

   b. Select the **Software Product**.

   c. Select the **Distribution** and the **Release Tag**.

      .. note::
         
         - For information on the supported distributions for your chipset, see the **Access Controlled Distribution** table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
         - For information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#id7>`__.         
         - For information on the QIMP and QIRP SDKs, see the following guides:

           - `QIMP SDK Quick Start Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-51>`__
           - `QIRP SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-265>`__

5.  Click **Download** to download the selected compilable distribution or flashable binary.

    The **Download** page displays the download progress as shown in the following figure:

    .. image:: ../../media/k2c-qli-build-ga/prebuilt_download.png

    .. note:: Download progress is also available in the top menu bar Downloads option.

    Flashable binaries do not require compilation. If a flashable binary is selected, follow the onscreen prompts to flash to a connected device:

    .. image:: ../../media/k2c-qli-build-ga/prebuilt_compile.png

.. _concept_n2t_tjn_w1c_step6:

Build and flash default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Compile the default build.

   .. note::
      - For information on the default configurations, see the **Applicable "MACHINE", "DISTRO" and "QCOM_SELECTED_BSP" for QSC** table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

   a. After the download is complete, select **Compile** to start compiling (depending on the size of the downloaded software and host machine configuration, the compilation process may take a few hours).

    .. image:: ../../media/k2c-qli-build-ga/download.png

   b. To view the compilation progress of individual software images, expand the logs panel.

    .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png

    .. note::
      
       BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 6 <concept_n2t_tjn_w1c_step6>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

2. Flash with default configuration.

   .. note::
      - If your reference kit matches the configuration compiled in previous step, continue with these steps. Else, skip to step 8 to build your own configuration.
      - Before you flash the software, ensure that the device is in Emergency Download (EDL) mode. For more information on how to force the device into EDL mode, see :ref:`Move to EDL mode <section_vgg_mly_v1c>`.

   a. To flash the software, select the device on which you want to flash the compiled software from the list of connected devices. Select the correct target device when multiple devices are connected to the host machine:

    .. image:: ../../media/k2c-qli-build-ga/flash.png

   b. Click **Flash on device**. The page updates and displays a progress bar as Launcher begins flashing the software. Leave the device connected while the software is being flashed.

   c. To view logs, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_logs.png

   d. When the process is finished, *Flash Complete* and other similar messages are displayed on the page.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_flashComplete.png

   e. Click **Done**. To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.


.. _build_launcher_step7:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. Compile the default build.

   .. note::
      - For information on the default configurations, see the **Applicable "MACHINE", "DISTRO" and "QCOM_SELECTED_BSP" for QSC** table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

   a. After the download is complete, select **Compile** to start compiling (depending on the size of the downloaded software and host machine configuration, the compilation process may take a few hours).

    .. image:: ../../media/k2c-qli-build-ga/download.png

   b. To view the compilation progress of individual software images, expand the logs panel.

    .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png

    .. note::
      
        BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 6 <concept_n2t_tjn_w1c_step6>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.


2. Build your own configuration.

   .. note:: For information on the supported configurations, see the **Applicable "MACHINE", "DISTRO" and "QCOM_SELECTED_BSP" for QSC** table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

   a. To execute the build commands for a specific configuration, click **Compile using terminal** next to LE.QCLINUX.1.0.r1:

      .. note:: Compilations executed through the terminal are not tracked by the Qualcomm Software Center and the ability to monitor their status on the Download page is lost.

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal.png

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_default.png

   b. Update the highlighted command with your configuration and run it on the terminal: 

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for Qualcomm® RB3 Gen 2 Vision Development Kit, update the ``MACHINE`` in the above Default Build Command to ``qcs6490-rb3gen2-vision-kit`` after opening the terminal.
   
   c. After a successful build, check that the ``system.img`` is in the ``<Base Workspace Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory. For example:

      ::

         cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

Flash
'''''''''

1. Check if ``QTI_HS-USB_QDLoader`` driver is available in the installed
   directory:

   ::

      ls –la /dev/Q*

   **Sample output**

   ::

      crw-rw-rw- 1 root 242 0 Dec 10 10:51 /dev/QTI_HS-USB_QDLoader_9008_3-8:1.0

2. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   ::

      Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

3. Check if the device is recognized by PCAT:

   ::

      pcat -devices

   **Sample output**

   ::

      Searching devices in Device Manager, please wait for a moment…
      ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
      NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

4. Download the build:

   ::

      PCAT –PLUGIN SD -DEVICE <device_serial_number> -BUILD “<build_images_path>” -MEMORYTYPE UFS -FLAVOR asic

      # Example
      PCAT -PLUGIN SD -DEVICE be116704 -BUILD "<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image" -MEMORYTYPE UFS -FLAVOR asic

   Flashing is successful if you see the following message:

   ::

      xxxx INFO] [ FIRMWARE DOWNLOAD LOG ] Process Finished                                                  
      xxxx INFO] Status   - TRUE
      xxxx INFO] Response - Downloading software images completed on the device Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

   After a successful flashing operation, run ``lsusb`` command to see
   the device information on the terminal window as shown in line 4 of
   the following message:

   ::

      ThinkPad-T490s:<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image$ lsusb
      Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
      Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
      Bus 002 Device 006: ID 05c6:901d Qualcomm, Inc. QCM6490_fd2913cf
      Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

.. note::

   -  To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.
   -  The device reboots on successful completion of the flashing procedure. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.