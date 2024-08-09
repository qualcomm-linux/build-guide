.. _flash_images_unregistered:

****************************************
Flash images for unregistered users
****************************************

Unregistered users must flash the software using the following steps:

1. Update ``udev`` rules (one-time prerequisite).
2. Move the device to Emergency Download (EDL) mode.
3. Flash software using QDL tool.

.. _section_wxy_mty_v1c:

Update ``udev`` rules
---------------------

Ensure that the ``udev`` USB rules for the Qualcomm manufacturing vendor
ID **05c6** are configured on the Linux host:

1. Navigate to the ``udev`` USB rules directory:

   ::

      cd /etc/udev/rules.d

2. List the contents of the directory:

   ::

      ls

   -  If the ``51-qcom-usb.rules`` file is not present, use
      ``sudo vi 51-qcom-usb.rules`` to create it and add the following
      content to the file:

      ::

         SUBSYSTEMS=="usb", ATTRS{idVendor}=="05c6", ATTRS{idProduct}=="9008", MODE="0666", GROUP="plugdev"

   -  If the file exists, then check for the previous content:

      ::

         cat51-qcom-usb.rules

3. Restart ``udev``:

   ::

      sudo systemctl restart udev

If the USB cable is already connected to the host, unplug the cable and
then reconnect it for the updated rules to take effect.

.. _section_vgg_mly_v1c:

Move to EDL mode
----------------

The device must be in EDL mode before you flash the software. The
Qualcomm RB3 Gen 2 platform enters EDL mode if there is no image on the
device after power up or if it is corrupted. To force the device into
EDL mode, use any one of the following methods:

**Using UART**

**Note:** The RB3 Gen 2 device must have a Qualcomm Linux build image.

1. `Connect device to UART
   shell <howto_setup.rst#section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684>`__.

2. Move the device to EDL mode:

   ::

      reboot edl

3. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   ::

      Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

**Note:** This procedure is verified on an Ubuntu host machine and not
on a VM.

**Using ADB**

**Note:** The RB3 Gen 2 device must have a Qualcomm Linux build image.

1. `Install
   QUD <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/connect_to_ssh.html#sub$qsg_instal_qud>`__
   on the host device.

2. `Install
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/connect_to_ssh.html#sub$qsg_install_adb>`__
   on the host device.

3. `Connect
   ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/connect_to_ssh.html#sub$qsg_connect_to_adb>`__
   to the RB3 Gen 2 device.

4. Move the device to EDL mode:

   ::

      adb shell reboot edl

5. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   ::

      Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

**Note:** This procedure is verified on an Ubuntu host machine and not
on a VM.

**Manual**

1. Press and hold the **F_DL** button:

   |image1|

2. Connect the device to a +12 V wall power supply.

3. Connect the device to the host system using a Type-C cable through
   the USB Type-C connector.

4. Release the **F_DL** button. The device should now be in Qualcomm
   download (QDL) mode. For this task, QDL is used interchangeably with
   EDL.

5. Verify whether the device has entered the QDL mode:

   ::

      lsusb

   **Sample output**

   ::

      Bus 002 Device 014: ID 05c6:9008 Qualcomm, Inc. Gobi Wireless Modem (QDL mode)

.. _section_byn_pdj_x1c:

Flash software using QDL
------------------------

1. Ensure ModemManager is not running.

   Some Linux distributions come with ModemManager, a tool for
   configuring mobile broadband. When the device is connected in USB
   mode, it is identified as a Qualcomm modem, and ModemManager tries to
   configure the device. As this interferes with QDL flashing, you must
   disable ModemManager before connecting your device. If you are using
   a Linux distribution with ``systemd``, then ModemManager can be
   stopped using the following command:

   ::

      sudo systemctl stop ModemManager

   If you need ModemManager, you can start it again after the flashing
   is complete.

2. Navigate to the following location in the workspace to find the QDL
   tool and flash the images:

   ::

      # Usage:
      # cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/<MACHINE>/<IMAGE>
      # build_path: For DISTRO=qcom-wayland, it is build-qcom-wayland. 
      #             For DISTRO=qcom-robotics-ros2-humble, it is build-qcom-robotics-ros2-humble
      # qdl <prog.mbn> [<program> <patch> ...]
      # Example: build_path is build-qcom-wayland
      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image
      ./qdl prog_firehose_ddr.elf rawprogram*.xml patch*.xml

   Flashing is successful if you see *partition 1 is now bootable* on
   the terminal window as shown in the following message:

   ::

      LOG: INFO: Calling handler for setbootablestoragedrive
      LOG: INFO: Using scheme of value = 1 
      partition 1 is now bootable
      LOG: INFO: Calling handler for power
      LOG: INFO: Will issue reset/power off 100 useconds, if this hangs check if watchdog is enabled
      LOG: INFO: bsp_target_reset() 1

   After a successful flashing operation, run the ``lsusb`` command to
   see the device information on the terminal window as shown in line 4
   of the following message:

   ::

      ThinkPad-T490s:<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image$ lsusb
      Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
      Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
      Bus 002 Device 006: ID 05c6:901d Qualcomm, Inc. QCM6490_fd2913cf
      Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub

**Note:**

If flashing fails, perform the following steps and retry the flashing
procedure:

1. Power off the device.
2. Disconnect from the host.
3. Reboot the host.

Do not move the QDL tool from this location to another alternate path or
host PC. If you must use the standalone QDL, see `How to build a
standalone QDL <howto_build.rst#how_to_build_qdl_standalone>`__.

To connect to the device, see `How to
SSH <howto_setup.rst#section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`__.

**Note:** The device reboots on successful completion of the flashing
procedure. To verify the updated software version, see `Check software
version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.

.. |image1| image:: ../../media/k2c-qli-build-ga/RB3Gen2_device.jpg
