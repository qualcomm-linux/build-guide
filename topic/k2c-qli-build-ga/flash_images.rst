.. _flash_images:

Flash software images
======================

Obtain flashable binaries
-----------------------------

On build completion, flashable images can be obtained from the following paths:

* This path contains the flattened image that can be flashed directly:
  
  .. container:: nohighlight

     ::

         <Base_Workspace_Path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs.qcomflash

* This path contains the flashable image directory in compressed format:

  .. container:: nohighlight

     ::

         <Base_Workspace_Path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs.qcomflash.tar.gz

  If you are downloading the prebuilt flashable images from CLO artifactory, you'll need this option.

.. note:: To unpack the compressed image directory, follow these commands:

   .. container:: nohighlight

      ::

         untar tar.gz
         cd <Base_Workspace_Path>/build/tmp/deploy/images/<MACHINE>/
         tar -xvf <IMAGE>-<MACHINE>.rootfs.qcomflash.tar.gz
         
         # Command to flash
         <Base_Workspace_Path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>

Flash instructions
-------------------

.. note:: If prebuilt flashable images (tar.gz) are downloaded from CLO artifactory, then :ref:`force the device into EDL mode <move_to_edl>`.

Follow these steps to flash the software images:

1. Update the ``udev`` rules (one-time prerequisite).
#. Force the device into emergency download (EDL) mode.
#. Provision UFS (one-time prerequisite).
#. Flash SAIL (one-time prerequisite).
#. Choose CDT based on reference kit.
#. Flash the software:

   - Using QDL
   - Using PCAT

.. _update_udev_rules:

Update ``udev`` rules (one-time prerequisite)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configure the ``udev`` USB rules for the Qualcomm manufacturing vendor ID **05c6** on the Linux host:

1. Go to the ``udev`` USB rules directory:

   .. container:: nohighlight
      
      ::

         cd /etc/udev/rules.d

#. List the contents of the directory:

   .. container:: nohighlight
      
      ::

         ls

   -  If the ``51-qcom-usb.rules`` file isn't present, use the ``sudo vi 51-qcom-usb.rules`` file to create it and add the following content to the file:

      .. container:: nohighlight
      
         ::

            SUBSYSTEMS=="usb", ATTRS{idVendor}=="05c6", ATTRS{idProduct}=="9008", MODE="0666", GROUP="plugdev"

   -  If the file exists, then check for the earlier content:

      .. container:: nohighlight
      
         ::

            cat 51-qcom-usb.rules

#. Restart ``udev``:

   .. container:: nohighlight
      
      ::

         sudo systemctl restart udev

#. If the USB cable is already connected to the host, unplug and reconnect the cable for the updated rules to take effect.

.. _move_to_EDL:

Force the device into EDL mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The device must be in the EDL mode before you flash the software. The Qualcomm supported device by default enters EDL mode if there is no image on the device after power up or if it's corrupted. To force the device into EDL mode, use any one of the following methods.

Use UART
''''''''''

Use UART only if the device has a preloaded build. This procedure applies to the Ubuntu host environment.

1. :ref:`Connect the device to a UART shell <connect_uart>`.

2. In the UART shell, move the device into EDL mode:

   .. container:: nohighlight
      
      ::

         reboot edl

3. Verify if the device is in EDL mode:

   .. container:: nohighlight
      
      ::

         lsusb

   If the output says QDL mode, the device is in EDL mode:
   
   .. container:: screenoutput

      .. line-block::

          Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

Use ADB
''''''''

This procedure applies to the Ubuntu host environment. Use ADB only if the device has a preloaded build. 

1. `Install
   QUD <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/faqs.html#install-qud>`__
   on the host device.

2. `Install
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/faqs.html#install-adb>`__
   on the host device.

3. `Connect
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/faqs.html#install-adb>`__
   to the device.

4. Move the device into EDL mode in a terminal on the host computer:

   .. container:: nohighlight
      
      ::

         adb shell reboot edl

5. Verify if the device is in EDL mode:

   .. container:: nohighlight
      
      ::

         lsusb

   If the output says QDL mode, the device is in EDL mode:

   .. container:: screenoutput

      .. line-block::

          Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

Manual
''''''''

.. container:: persistenttab-soc

   .. tabs::

      .. group-tab:: QCS6490/QCS5430

         1. Press and hold the **F_DL** button.

            .. image:: ../../media/k2c-qli-build-ga/RB3Gen2_device.png

         #. Connect the device to a **+12 V wall power supply**.

         #. Connect the device to the host system using a Type-C cable through the USB Type-C connector.
         
         #. Release the **F_DL** button. The device is now EDL mode.
         
         #. Verify if the device is in EDL mode:

            .. container:: nohighlight
      
               ::

                  lsusb

            If the output says QDL mode, the device is in EDL mode:

            .. container:: screenoutput

               .. line-block::

                   Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

      .. group-tab:: IQ-9075

         .. tabs:: 

            .. group-tab:: IQ-9 Beta Evaluation Kit (EVK)

               1. Put the device in EDL mode by turning on the dip switch S5-4.

                  .. image:: ../../media/k2c-qli-build-ga/qcs9075_qdl_mode_manual.png

               #. Verify if the device is in EDL mode:

                  .. container:: nohighlight
      
                     ::

                        lsusb

                  **Sample output**

                  .. container:: screenoutput

                     .. line-block::

                        Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

               .. note:: Dip switch S5-4 must be turned off after the flashing is complete.

            .. group-tab:: IQ-9075 EVK

               1. Put the device in EDL mode by turning on the dip switch SW2-3.

                  .. image:: ../../media/k2c-qli-build-ga/IQ_9075_EVK.png

               #. Verify if the device is in EDL mode:

                  .. container:: nohighlight
      
                     ::

                        lsusb

                  If the output says QDL mode, the device is in EDL mode:

                  .. container:: screenoutput

                     .. line-block::

                        Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

               .. note:: Dip switch S2-8 must be turned off after the flashing is complete.
      
      .. group-tab:: IQ-8275

          .. tabs:: 

            .. group-tab:: IQ-8 Beta Evaluation Kit (EVK)

               1. Put the device in EDL mode by turning on the dip switch S5-4.

                  .. image:: ../../media/k2c-qli-build-ga/qcs9075_qdl_mode_manual.png

               #. Verify if the device is in EDL mode:

                  .. container:: nohighlight
      
                     ::

                        lsusb

                  If the output says QDL mode, the device is in EDL mode:

                  .. container:: screenoutput

                     .. line-block::

                        Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

               .. note:: Dip switch S5-4 must be turned off after the flashing is complete.

            .. group-tab:: IQ-8275 EVK

               1. Put the device in EDL mode by turning on the dip switch S2-8.

                  .. image:: ../../media/k2c-qli-build-ga/IQ_9075_EVK.png

               #. Verify if the device is in EDL mode:

                  .. container:: nohighlight
      
                     ::

                        lsusb

                  **Sample output**

                  .. container:: screenoutput

                     .. line-block::

                        Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

               .. note:: Dip switch S2-8 must be turned off after the flashing is complete.

.. _provision_ufs:

Provision UFS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Universal flash storage (UFS) provisioning helps to divide the storage into many LUNs, which stores different types of data separately. This improves access efficiency and system organization.

UFS is provisioned by default. If there are any changes in LUNs, UFS must be re-provisioned. To download the provision XML file and to check the applicability of UFS provisioning for different SoCs, see the table *UFS Provision* in `Release Specific Information <https://docs.qualcomm.com/doc/80-70023-300/topic/release_specific_information.html>`__.

1. Download the provision file.

   Based on the required SoC, download the respective ‘provision’ from the *UFS Provision* table of the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/topic/release_specific_information.html>`__.

   .. container:: nohighlight
      
      ::

         mkdir <provision_download_path>
         cd <provision_download_path>
         wget <provision_download_link>
         # Example, wget https://artifacts.codelinaro.org/artifactory/codelinaro-le/Qualcomm_Linux/QCS6490/provision.zip
         unzip <downloaded_zip_file>
         # Example, unzip provision.zip   

#. Download the Qualcomm Device Loader (QDL). QDL is a software tool that communicates with the Qualcomm USB devices to upload a flash loader and flash software images. Acquire the latest version of the QDL tool using one of the following methods:
   
   - Download the tool from https://softwarecenter.qualcomm.com/#/catalog/item/Qualcomm_Device_Loader.
   - Download the latest QDL using CLI based on the required operating system and architecture type:
   
     .. container:: nohighlight
      
        ::
            
           # Linux X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/Debian/latest.zip
           # Linux ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/ARM64/Debian/latest.zip
           # Windows X64     
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/latest.zip
           # Windows ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/ARM64/latest.zip
           # macOS X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/latest.zip
           # macOS ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/ARM64/latest.zip

#. Unzip QDL:

   .. container:: nohighlight
      
      ::
     
         unzip <qdl_downloaded_file>.zip

#. Run the following command to provide executable permission to QDL:

   .. container:: nohighlight
      
      ::
   
         chmod -R 777 <qdl_download_path>

#. Provision UFS:

   .. container:: nohighlight
      
      ::

         cd <provision_download_path>
         <qdl_download_path>/QDL_<version>_<operating_system>_<architecture_type>/qdl --storage ufs prog_firehose_ddr.elf <Provision file>
         # Example, <qdl_download_path>/QDL_2.3.9_Linux_x64/qdl --storage ufs prog_firehose_ddr.elf provision_1_3.xml

.. _flash_sail:

Flash SAIL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Safety Island (SAIL) is applicable only for the Qualcomm Dragonwing™ IQ-9075 and the Qualcomm Dragonwing™ IQ-8275 development kits. If you're not using one of these development kits, skip to :ref:`Flash CDT <flash_cdt>`.

1. Download the QDL tool. QDL is a software tool that communicates with the Qualcomm USB devices to upload a flash loader and flash software images. Acquire the latest version of the QDL tool using one of the following methods:
   
   - Download the tool from https://softwarecenter.qualcomm.com/#/catalog/item/Qualcomm_Device_Loader.
   - Download the latest QDL using CLI based on the required operating system and architecture type:
   
     .. container:: nohighlight
      
        ::
            
           # Linux X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/Debian/latest.zip
           # Linux ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/ARM64/Debian/latest.zip           
           # Windows X64     
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/latest.zip
           # Windows ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/ARM64/latest.zip
           # macOS X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/latest.zip
           # macOS ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/ARM64/latest.zip

#. Unzip QDL:

   .. container:: nohighlight
      
      ::
     
         unzip <qdl_downloaded_file>.zip

#. Run the following command to provide executable permission to QDL:

   .. container:: nohighlight
      
      ::
   
         chmod -R 777 <qdl_download_path>

#. Flash the SAIL:

   .. container:: nohighlight
      
      ::

        # SAIL image is under <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/sail_nor
        # qdl --storage spinor <prog.mbn> [<program> <patch> ...]
        cd <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/sail_nor
        <qdl_download_path>/QDL_<version>_<operating_system>_<architecture_type>/qdl --storage spinor prog_firehose_ddr.elf rawprogram0.xml patch0.xml
        # Example, <qdl_download_path>/QDL_2.3.9_Linux_x64/qdl --storage spinor prog_firehose_ddr.elf rawprogram0.xml patch0.xml
         
.. _Choose_cdt:

Choose CDT based on reference kit
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Configuration data table (CDT) provides platform/device-dependent data such as platform ID, subtype, version. Various Software (drivers/firmware) modules can use this information to perform dynamic detection and initialization of the platform.

1. Update CDT binary.
   
   Based on the reference kit, rename the respective reference kit CDT file as cdt.bin in flashable images path.

   .. note:: Default core kit CDT is ``cdt.bin``. Skip the CDT binary update if the reference kit is core kit.

   .. container:: nohighlight

      ::

         # Flashable images path is <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/
         cd <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/
         # Example, the following bin files are available for Kodiak: cdt_core_kit.bin, cdt_vision_kit.bin, and cdt_industrial_kit.bin
         # If reference kit is RB3Gen2 Vision Kit, then set up cdt_vision_kit.bin as cdt.bin
         cp cdt_vision_kit.bin cdt.bin

Flash software using QDL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Ensure that the ModemManager tool isn't running.

   Some Linux distributions include the ModemManager tool, which allows you to configure the mobile broadband. When you connect the device in the USB mode, it's identified as a Qualcomm modem, and the ModemManager tries to configure the device. Because this interferes with the QDL flashing, you must disable the ModemManager before connecting your device.
   
   If you are using a Linux distribution with ``systemd``, stop the ModemManager tool using the following command:

   .. container:: nohighlight
      
      ::

         sudo systemctl stop ModemManager

   If you need the ModemManager, you can restart it after the flashing is complete.
  
#. Download the QDL tool. QDL is a software tool that communicates with Qualcomm USB devices to upload a flash loader and flash software images. Acquire the latest version of the QDL tool using one of the following methods:
   
   - Download the tool from https://softwarecenter.qualcomm.com/#/catalog/item/Qualcomm_Device_Loader.
   - Download the latest QDL using CLI based on the required operating system and architecture type:
   
     .. container:: nohighlight
      
        ::
            
           # Linux X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/Debian/latest.zip
           # Linux ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Linux/ARM64/Debian/latest.zip           
           # Windows X64     
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/latest.zip
           # Windows ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/Windows/ARM64/latest.zip
           # macOS X64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/latest.zip
           # macOS ARM64
           wget https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Device_Loader/macOS/ARM64/latest.zip

#. Unzip QDL:

   .. container:: nohighlight
      
      ::
     
         unzip <qdl_downloaded_file>.zip

#. Run the following command to provide executable permission to QDL:

   .. container:: nohighlight
      
      ::
   
         chmod -R 777 <qdl_download_path>

#. Flash the images:

   .. container:: nohighlight
      
      ::

         # Built images are under <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/
         # qdl <prog.mbn> [<program> <patch> ...]
         # For UFS storage
         cp ./partition_ufs/gpt_main*.bin ./partition_ufs/gpt_backup*.bin ./partition_ufs/rawprogram[0-9].xml ./partition_ufs/patch*.xml ./partition_ufs/zeros_*sectors.bin ./
         <qdl_download_path>/QDL_<version>_<operating_system>_<architecture_type>/qdl --storage ufs prog_firehose_ddr.elf rawprogram*.xml patch*.xml
         # For EMMC storage
         cp ./partition_emmc/gpt_main*.bin ./partition_emmc/gpt_backup*.bin ./partition_emmc/rawprogram[0-9].xml ./partition_emmc/patch*.xml ./partition_emmc/zeros_*sectors.bin ./
         <qdl_download_path>/QDL_<version>_<operating_system>_<architecture_type>/qdl --storage emmc prog_firehose_ddr.elf rawprogram0.xml patch0.xml

   .. note:: Use QDL binary based on the host computer architecture. For example, linux_x64 supported qdl binary is ``qdl_<version>/QDL_Linux_x64/qdl``.

   Flashing is successful if you see *partition 1 is now bootable* on the terminal window as shown in the following message:

   .. container:: screenoutput

      .. line-block::

         LOG: INFO: Calling handler for setbootablestoragedrive
         LOG: INFO: Using scheme of value = 1 
         partition 1 is now bootable
         LOG: INFO: Calling handler for power
         LOG: INFO: Will issue reset/power off 100 useconds, if this hangs check if watchdog is enabled
         LOG: INFO: bsp_target_reset() 1

#. After a successful flashing operation, run the ``lsusb`` command to see the device information on the terminal window as shown in line 4 of the following message:
   
   .. container:: screenoutput

      .. line-block::
      
         # Sample output for QCS6490
         Bus 002 Device 003: ID 05c6:9135 Qualcomm, Inc. qcs6490-rb3gen2-vision-kit
         Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
         Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

   To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.

.. note:: If flashing fails, perform the following steps and retry the flashing procedure:

          1. Power off the device.
          2. Disconnect from the host.
          3. Restart the host.

Flash software using PCAT
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. note:: This procedure is available for registered users only.

1. :ref:`Install QSC CLI <install_qsc_cli>`.
 
#. To detect the connected devices and flash the software builds, install the Qualcomm PCAT and QUD tools on the host computer:

   .. container:: nohighlight
      
      ::

         qsc-cli login
         qsc-cli tool install --name quts --activate-default-license
         qsc-cli tool install --name qud --activate-default-license
         qsc-cli tool install --name pcat --activate-default-license

   .. note:: For Ubuntu 22.04, you might see an issue while installing QUD, where you must enroll the public key on your Linux host for a successful QUD installation. For more details, follow the steps provided in the README file available in the ``/opt/QTI/sign/signReadme.txt`` directory.

#. Check if the ``QTI_HS-USB_QDLoader`` driver is available in the installed directory:

   .. container:: nohighlight
      
      ::

         ls –la /dev/Q*

   **Sample output**

   .. container:: screenoutput

      .. line-block::

          crw-rw-rw- 1 root 242 0 Dec 10 10:51 /dev/QTI_HS-USB_QDLoader_9008_3-8:1.0

#. Verify if the device is in EDL mode:

   .. container:: nohighlight
      
      ::

         lsusb

   If the output says QDL mode, the device is in EDL mode:

   .. container:: screenoutput

      .. line-block::

          Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

#. Check if PCAT recognizes the device:

   .. container:: nohighlight
      
      ::

         pcat -devices

   **Sample output**

   .. container:: screenoutput

      Searching devices in Device Manager, please wait for a moment…
      ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
      NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

#. Flash the build:

   .. container:: nohighlight
      
      ::
         Note: <build_images_path> refering below for <workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/

         cd <build_images_path>
 
         # For UFS storage
         cp ./partition_ufs/gpt_main*.bin ./partition_ufs/gpt_backup*.bin ./partition_ufs/rawprogram[0-9].xml ./partition_ufs/patch*.xml ./partition_ufs/zeros_*sectors.bin ./
         PCAT –PLUGIN SD -DEVICE <device_serial_number> -BUILD "<build_images_path>"" -MEMORYTYPE UFS -FLAVOR asic
         
         # Example, PCAT –PLUGIN SD -DEVICE be116704 -BUILD "<workspace_path>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>/" -MEMORYTYPE UFS -FLAVOR asic
 
         # For EMMC storage
         cp ./partition_emmc/gpt_main*.bin ./partition_emmc/gpt_backup*.bin ./partition_emmc/rawprogram[0-9].xml ./partition_emmc/patch*.xml ./partition_emmc/zeros_*sectors.bin ./
         PCAT –PLUGIN SD -DEVICE <device_serial_number> -BUILD "<build_images_path>"" -MEMORYTYPE EMMC -FLAVOR asic -RAWPROG "<build_images_path>/rawprogram0.xml" -PATCHPROG "<build_images_path>/patch0.xml"
         

   If flashing the software is successful, the outputs is as follows:

   .. container:: screenoutput

      .. line-block::

         xxxx INFO] [ FIRMWARE DOWNLOAD LOG ] Process Finished
         xxxx INFO] Status   - TRUE
         xxxx INFO] Response - Downloading software images completed on the device Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

#. After a successful flashing operation, run the ``lsusb`` command to see the device information on the terminal window as shown in line 4 of the following message:

   .. container:: screenoutput

      .. line-block::
 
         # Sample output for QCS6490
         Bus 002 Device 003: ID 05c6:9135 Qualcomm, Inc. qcs6490-rb3gen2-vision-kit
         Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
         Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

   The device reboots after the flashing procedure completes successfully. To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.

Related topics
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`