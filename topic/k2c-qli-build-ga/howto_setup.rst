.. _howto_setup:

Setup
------------------

.. _section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684:

Connect to a UART shell
^^^^^^^^^^^^^^^^^^^^^^^^^

.. container:: persistenttab-soc

   .. tabs::

      .. group-tab:: QCS6490/QCS5430
         
         To set up the debug UART connection and view the diagnostic messages, connect the micro-USB cable from the micro-USB port on the RB3 Gen 2 device to the Linux host.

         .. image:: ../../media/k2c-qli-build-ga/micro_usb_port.jpg

      .. group-tab:: QCS9075/QCS8300

         To set up the debug UART connection and view the diagnostic messages, connect the debug-USB cable from the debug-USB port on the QCS9075 device to the Linux host.

         .. image:: ../../media/k2c-qli-build-ga/uart_ridesx.png

1. Install Minicom on the Linux host:

   ::

      sudo apt update
      sudo apt install minicom

2. Check the USB port:

   ::

      ls /dev/ttyUSB*

   **Sample output**

   ``/dev/ttyUSB0``

3. Open Minicom:

   ::

      sudo minicom -s

4. Press the Down arrow key to select the **Serial port setup** option. Use the Up and Down arrow keys to navigate through the menu.

   .. image:: ../../media/k2c-qli-build-ga/serial_port_setup.jpg
      :align: center

5. Set up the serial device configuration:

   .. note:: Ensure that the letters are in uppercase.

   a. Press **A** on your keyboard to set up the serial device name such as ``/dev/ttyUSB0``.

   #. Press **Enter** to save the changes.

   #. Press **E** on your keyboard to set the baud rate and 8N1 configuration:
   
      i. Press the **E** key again if the baud rate is not set to **115200**.

      #. Press the **Q** key if the configuration is not set to **8N1**.

      .. image:: ../../media/k2c-qli-build-ga/option_Q.png

   #. Press **Enter** to save the changes.

   #. Press **F** on your keyboard to set the **Hardware Flow Control**
      to ``No``.

      .. image:: ../../media/k2c-qli-build-ga/serial_device_configuration.png
         :align: center

   #. Press **Enter** to save the changes.

6. Select the **Save setup as dfl** option and press **Enter**.

   .. image:: ../../media/k2c-qli-build-ga/save_setup_as_dfl.png
      :align: center

7. Select **Exit** to open the UART console and press the ENTER key.

8. Log in to the UART console:

   -  Login: ``root``
   -  Password: ``oelinux123`` 
     
      .. note:: If the login console does not display as expected, verify the USB connection. If the issue persists, disconnect and then reconnect the micro-USB.

.. note:: 
    If you want to run sample applications from the UART shell, remount the root file system with write permissions:

    ::

      mount -o rw,remount /

.. _connect_to_network:

Connect to the network
^^^^^^^^^^^^^^^^^^^^^^^

.. _howto_setup_wifi_sub:

**Set up Wi-Fi**

Wi-Fi is operational in Station mode. The Wi-Fi host driver and the authentication for network management are initialized during the device boot up.

1. Connect to the Wireless Access Point (Wi-Fi Router):

   ::

      nmcli dev wifi connect <WiFi-SSID> password <WiFi-password>

   **Example**

   ::

      root@qcs6490-rb3gen2-vision-kit:~# nmcli dev wifi connect Qualcomm password 1234567890

   .. container:: screenoutput

      .. line-block:: 

         Device ‘wlan0’ successfully activated with ‘d7b990bd-3b77-4b13-b239-b706553abaf8’.

#. Check the connection and device status:

   ::

      execute <nmcli -p device>
   
   .. container:: screenoutput

      .. line-block:: 

         Status of devices
         DEVICE  TYPE      STATE        CONNECTION
         wlan0   wifi      connected    Qualcomm
         eth0    ethernet  unavailable  –
         eth1    ethernet  unavailable  –
         can0    can       unmanaged    –
         lo      loopback  unmanaged    –

#. Check the WLAN connection status and IP address:

   ::

      ifconfig wlan0

   .. container:: screenoutput

      .. line-block:: 

         wlan0 Link encap:Ethernet  HWaddr 00:03:7F:12:F7:F7
         inet addr:192.168.117.130  Bcast:192.168.117.255  Mask:255.255.255.0
         inet6 addr: 2401:4900:658c:d8b0:3213:94e9:a421:a15e/64 Scope:Global
         inet6 addr: fe80::b91:3f50:78fc:855a/64 Scope:Link
         inet6 addr: fe80::e17e:9961:c1a6:4524/64 Scope:Link
         inet6 addr: 2401:4900:658c:d8b0:3280:ef33:8f01:843d/64 Scope:Global
         UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
         RX packets:38 errors:0 dropped:0 overruns:0 frame:0
         TX packets:99 errors:0 dropped:0 overruns:0 carrier:0
         collisions:0 txqueuelen:1000
         RX bytes:4361 (4.2 KiB)  TX bytes:15141 (14.7 KiB)

#. Ensure that the connection is active by pinging any website:

   ::

      ping google.com

**Switching networks**

If you are already connected to a network and need to reconnect to another network, do the following:

1. Disconnect from the current network:

   ::

      nmcli c down Qualcomm

   .. container:: screenoutput

      .. line-block:: 
         
         Connection ‘Qualcomm’ successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/1)

#. Check the disconnect status:

   ::

      nmcli -p device

   .. container:: screenoutput

      .. line-block:: 
         
         Status of devices
         DEVICE  TYPE      STATE           CONNECTION
         wlan0   wifi      disconnected    Qualcomm
         eth0    ethernet  unavailable     –
         eth1    ethernet  unavailable     –
         can0    can       unmanaged       –
         lo      loopback  unmanaged       –

#. Connect to a different Wi-Fi network:

   ::

      nmcli dev wifi connect QualcommAP password XXXXXXXXX

   .. container:: screenoutput

      .. line-block:: 
         
         Device ‘wlan0’ successfully activated with ‘6159ac7c-58c2-44fa-938f-45dcb544fac3’.

.. _use-ssh:

**Log in using SSH**

Ensure that a :ref:`Wi-Fi connection <howto_setup_wifi_sub>` is established before connecting to SSH.

1. Find the IP address of the RB3 Gen 2 device in the UART console on the Linux host:

   ::

      ifconfig wlan0

#. Use the IP address to establish an SSH connection from the remote host to the device:

   ::

      ssh root@ip-address

   **Example**

   ``ssh root@10.92.180.250``

#. Connect to the SSH shell using the following password:

   ::

      oelinux123

.. note:: Ensure that the remote host is connected to the same Wi-Fi access point.

.. _section_j5g_rds_5bc_vinayjk_06-21-24-1739-53-921:

Configure Ethernet with RJ45 port
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ethernet/RJ45 port is enabled as a downstream port of PCIe to USB controller (``renesas``). Ensure that the ``renesas_usb_fw.mem`` file is available at the ``/lib/firmware`` directory.

.. note:: 
   - If the ``renesas_usb_fw.mem`` firmware is not available at the ``/lib/firmware`` directory, then :ref:`connect to UART <section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684>` and :ref:`enable the Wi-Fi <howto_setup_wifi_sub>`.
   - After getting the SSH and the IP address, :ref:`update USB and Ethernet controller firmware <section_nsb_5gs_5bc_vinayjk_06-21-24-1803-34-149>`.

To check if the USB to ETH controller is enumerated, run the following command:

::

   lsusb

**Sample output**:

::

   Bus 002 Device 003: ID 0b95:1790 ASIX Electronics Corp. AX88179 Gigabit Ethernet
   Bus 002 Device 002: ID 05e3:0625 Genesys Logic, Inc. USB3.2 Hub
   Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
   Bus 001 Device 002: ID 05e3:0610 Genesys Logic, Inc. Hub
   Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

Connect an RJ45 cable to the RB3 Gen 2 device.

To check the Ethernet IP address, run the following command:

::

   ifconfig

**Sample output**:

.. note:: 10.219.0.106 is the IP address.

::

   enP1p4s0u1u1 Link encap:Ethernet HWaddr A6:CD:9B:FD:C1:B5
             inet addr:10.219.0.106  Bcast:10.219.1.255  Mask:255.255.254.0
             inet6 addr: fe80::a370:7a00:8131:5a03/64 Scope:Link
             UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
             RX packets:1071 errors:0 dropped:0 overruns:0 frame:0
             TX packets:132 errors:0 dropped:0 overruns:0 carrier:0
             collisions:0 txqueuelen:1000
             RX bytes:60711 (59.2 KiB)  TX bytes:18342 (17.9 KiB)

.. _section_nsb_5gs_5bc_vinayjk_06-21-24-1803-34-149:

Update USB and Ethernet controller firmware
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you encounter USB or Ethernet connectivity issues on the RB3 Gen 2 device, consider updating the firmware for the USB controller.

.. rubric:: Prerequisites

- Ensure that the software is upgraded as described in `Update Software <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-253/set_up_the_device.html#ubuntu-up-sw>`__ before updating the Renesas firmware.
- The device should be connected to the Linux host through the USB Type-C cable.

.. note:: The following procedure is applicable only to Ubuntu 22.04 host. If you are using a Windows or Mac host, set up an Ubuntu virtual machine by following the instructions described in the `Virtual Machine Setup Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-41/getting-started.html>`__.

1. Register and log in to `Renesas <https://www.renesas.com/>`__.

#. `Download the firmware <https://www.renesas.com/en/products/interface/usb-switches-hubs/upd720201-usb-30-host-controller#design_development>`__.

#. Create the ``usb_fw.img`` image and copy the USB firmware:

   ::

      dd if=/dev/zero of=usb_fw.img bs=4k count=240
      mkfs -t ext4 usb_fw.img
      mkdir usb_fw
      sudo mount -o loop usb_fw.img usb_fw/
      sudo cp -rf renesas_usb_fw.mem usb_fw
      sudo umount usb_fw

#. Start the device in Fastboot mode:

   ::

      adb root
      adb shell
      reboot bootloader

#. Check if the device is in Fastboot mode:

   ::

      fastboot devices

   .. container:: screenoutput

      .. line-block::

         7dc85f5e     fastboot

#. Flash the ``usb_fw.img`` image to the device:

   ::

      fastboot erase usb_fw
      fastboot flash usb_fw  usb_fw.img