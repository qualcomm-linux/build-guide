.. _flash_images_unregistered:

Flash images
===============

Follow these steps to flash the software:

1. Update the ``udev`` rules (one-time prerequisite).
2. Move the device to Emergency Download (EDL) mode.
3. Flash the software:

   - Using QDL
   - Using PCAT

.. _update_udev_rules:

Update ``udev`` rules
------------------------------------

Configure the ``udev`` USB rules for the Qualcomm manufacturing vendor ID **05c6** on the Linux host:

1. Navigate to the ``udev`` USB rules directory:

   ::

      cd /etc/udev/rules.d

2. List the contents of the directory:

   ::

      ls

   -  If the ``51-qcom-usb.rules`` file is not present, use the ``sudo vi 51-qcom-usb.rules`` file to create it and add the following content to the file:

      ::

         SUBSYSTEMS=="usb", ATTRS{idVendor}=="05c6", ATTRS{idProduct}=="9008", MODE="0666", GROUP="plugdev"

   -  If the file exists, then check for the previous content:

      ::

         cat51-qcom-usb.rules

3. Restart ``udev``:

   ::

      sudo systemctl restart udev

If the USB cable is already connected to the host, unplug the cable and then reconnect it for the updated rules to take effect.

.. _section_vgg_mly_v1c:

Move to EDL mode
------------------------------------

The device must be in the EDL mode before you flash the software. The Qualcomm supported device enters the EDL mode if there is no image on the device after power up or if it is corrupted. To force the device into the EDL mode, use any one of the following methods.

**Using UART**

.. note:: The device must have a Qualcomm Linux build image.

1. :ref:`Connect the device to a UART shell <section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684>`.

2. Move the device to the EDL mode:

   ::

      reboot edl

3. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   .. container:: screenoutput

       Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

.. note:: This procedure is verified on an Ubuntu host machine and not on a VM.

**Using ADB**

.. note:: The device must have a Qualcomm Linux build image.

1. `Install
   QUD <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/additional_setup.html#sub$qsg_instal_qud>`__
   on the host device.

2. `Install
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/additional_setup.html#sub$qsg_install_adb>`__
   on the host device.

3. `Connect
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/additional_setup.html#sub$qsg_connect_to_adb>`__
   to the device.

4. Move the device to EDL mode:

   ::

      adb shell reboot edl

5. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   .. container:: screenoutput

       Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

.. note:: This procedure is verified on an Ubuntu host machine and not on a VM.

**Manual**

.. container:: persistenttab-soc

   .. tabs::

      .. group-tab:: QCS6490/QCS5430

         1. Press and hold the **F_DL** button.

            .. image:: ../../media/k2c-qli-build-ga/RB3Gen2_device.jpg

         #. Connect the device to a +12 V wall power supply.

         #. Connect the device to the host system using a Type-C cable through the USB Type-C connector.
         
         #. Release the **F_DL** button. The device should now be in the Qualcomm download (QDL) mode. For this task, QDL is used interchangeably with EDL.
         
         #. Verify whether the device has entered the QDL mode:

            ::

               lsusb

            **Sample output**

            .. container:: screenoutput

                Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

      .. group-tab:: QCS9075

         1. Switch on the dip switch S5-4 to put the device in the EDL mode.

            .. image:: ../../media/k2c-qli-build-ga/qcs9075_qdl_mode_manual.png

         #. Verify whether the device has entered the QDL mode:

            ::

               lsusb

            **Sample output**

            .. container:: screenoutput

                Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

         #. Switch off the dip switch S5-4 after the flashing is complete.

.. _section_byn_pdj_x1c:

Flash software using QDL
------------------------------------
.. note::
   - This process is available for both registered and unregistered users.
   - **Prerequisites**
      - The modules ``make`` and ``gcc`` must be available.
      - Install the following dependent packages:

        ::

          sudo apt-get install git libxml2-dev libusb-1.0-0-dev pkg-config

1. Ensure that the ModemManager tool is not running.

   Some Linux distributions include the ModemManager tool, which allows you to configure the mobile broadband. When the device is connected in USB mode, it is identified as a Qualcomm modem, and ModemManager tries to configure the device. As this interferes with QDL flashing, you must disable ModemManager before connecting your device.
   
   If you are using a Linux distribution with ``systemd``, stop the ModemManager tool using the following command:

   ::

      sudo systemctl stop ModemManager

   If you need ModemManager, you can start it again after the flashing is complete.
  
#. Download the QDL tool, compile it, and flash the images:

   ::

      # Download and compile QDL
      cd <workspace_path>
      git clone --depth 1 --branch master https://github.com/linux-msm/qdl qdl_tool
      cd qdl_tool
      git checkout cbd46184d33af597664e08aff2b9181ae2f87aa6
      make

      # Flash images
      # Built images are under <workspace_path>/<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/<IMAGE>
      # build_path: For DISTRO=qcom-wayland, it is build-qcom-wayland. 
      #             For DISTRO=qcom-robotics-ros2-humble, it is build-qcom-robotics-ros2-humble
      # qdl <prog.mbn> [<program> <patch> ...]
      # Example: build_path is build-qcom-wayland
      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      <workspace_path>/qdl_tool/qdl prog_firehose_ddr.elf rawprogram*.xml patch*.xml

   Flashing is successful if you see *partition 1 is now bootable* on the terminal window as shown in the following message:

   ::

      LOG: INFO: Calling handler for setbootablestoragedrive
      LOG: INFO: Using scheme of value = 1 
      partition 1 is now bootable
      LOG: INFO: Calling handler for power
      LOG: INFO: Will issue reset/power off 100 useconds, if this hangs check if watchdog is enabled
      LOG: INFO: bsp_target_reset() 1

   After a successful flashing operation, run the ``lsusb`` command to see the device information on the terminal window as shown in line 4 of the following message:

   .. container:: screenoutput
      
       # Sample output for QCM6490
       Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
       Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
       Bus 002 Device 006: ID 05c6:901d Qualcomm, Inc. QCM6490_fd2913cf
       Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

.. note:: If flashing fails, perform the following steps and retry the flashing procedure:

          1. Power off the device.
          2. Disconnect from the host.
          3. Reboot the host.

          Do not move the QDL tool from this location to an alternate path or host machine. If you must use the standalone QDL, see :ref:`How to build a standalone QDL <how_to_build_qdl_standalone>`. To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.


.. note:: The device reboots after the flashing procedure is completed successfully. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.

Flash software using PCAT
------------------------------------
.. note:: This procedure is available for registered users only.

1. To detect the connected devices and flash the software builds, ensure that the Qualcomm PCAT and QUD tools are installed on the host machine. Run the following commands to use ``qpm-cli`` to install PCAT and QUD:

   ::

    qpm-cli --login
    qpm-cli --install pcat --activate-default-license
    qpm-cli --install qud --activate-default-license

   .. note:: For Ubuntu 22.04, you may encounter an issue while installing QUD, where you might be asked to enroll the public key on your Linux host for a successful QUD installation. For additional details, follow the steps provided in the README file available in the ``/opt/QUIC/sign/signReadme.txt`` directory.

2. Check if the ``QTI_HS-USB_QDLoader`` driver is available in the installed directory:

   ::

      ls –la /dev/Q*

   **Sample output**

   .. container:: screenoutput

       crw-rw-rw- 1 root 242 0 Dec 10 10:51 /dev/QTI_HS-USB_QDLoader_9008_3-8:1.0

3. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   .. container:: screenoutput

       Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

4. Check if the device is recognized by PCAT:

   ::

      pcat -devices

   **Sample output**

   .. container:: screenoutput

       Searching devices in Device Manager, please wait for a moment…
       ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
       NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

5. Download the build:

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
 
       # Sample output for QCM6490
       Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
       Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
       Bus 002 Device 006: ID 05c6:901d Qualcomm, Inc. QCM6490_fd2913cf
       Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

.. note::

   -  To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.
   -  The device reboots on successful completion of the flashing procedure. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.