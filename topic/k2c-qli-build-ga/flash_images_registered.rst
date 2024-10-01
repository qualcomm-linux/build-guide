.. _flash_images:

Flash images for registered users
========================================

Registered users must flash the software using the following steps:

1. Install PCAT and QUD tools on host machine (one-time prerequisite).
2. Update ``udev`` rules (one-time prerequisite).
3. Move the device to EDL mode.
4. Flash the software using QDL, PCAT, or QSC Launcher.

.. _section_wxy_mty_v1c:

Install PCAT and QUD
---------------------------

To detect the connected devices and flash the software builds, ensure that the Qualcomm PCAT and QUD tools are installed on the host machine. Run the following commands to use ``qpm-cli`` to install PCAT and QUD:

::

   qpm-cli --login
   qpm-cli --install pcat --activate-default-license
   qpm-cli --install qud --activate-default-license

.. note:: For Ubuntu 22.04, you may encounter an issue while installing QUD, where you might be asked to enroll the public key on your Linux
          host for a successful QUD installation. For additional details, follow the steps provided in the README file available in the
          ``/opt/QUIC/sign/signReadme.txt`` directory.

.. _update_udev_rules:

Update ``udev`` rules
-----------------------------------------

See :ref:`Update udev rules <section_wxy_mty_v1c>`.

.. _section_az4_rdn_1cc_vinayjk_07-12-24-1201-27-226:

Move to EDL mode
-----------------------------------------

See :ref:`Move to EDL mode <section_vgg_mly_v1c>`.

.. _section_jpb_5dn_1cc_vinayjk_07-12-24-1202-10-915:

Flash software using QDL
-----------------------------------------

See :ref:`Flash software using QDL <section_byn_pdj_x1c>`.

.. _section_mcc_yp5_qbc_vinayjk_06-07-24-1834-57-144:

Flash software using PCAT
-----------------------------------------

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

.. _section_mcj_21d_rbc_vinayjk_06-08-24-2217-8-100:

Flash software using QSC Launcher
-----------------------------------------

See :ref:`Flash using QSC Launcher <section_cmp_qbj_x1c>`.

.. note:: The device reboots on successful completion of the flashing procedure. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-253/ubuntu_host.html#sub$check_sw_version_uart>`__.
