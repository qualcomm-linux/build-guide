.. _howto_setup:

Setup
------------------

.. _connect_uart:

Connect to a UART shell
^^^^^^^^^^^^^^^^^^^^^^^^^

.. container:: persistenttab-soc

   .. tabs::

      .. group-tab:: QCS6490/QCS5430
         
         To set up the debug UART connection and view the diagnostic messages, connect the micro-USB cable from the micro-USB port on the RB3 Gen 2 device to the Linux host.

         .. image:: ../../media/k2c-qli-build-ga/micro_usb_port.jpg

      .. group-tab:: QCS9075

         .. tabs:: 

            .. group-tab:: Qualcomm® IQ-9 Beta Evaluation Kit

               To set up the debug UART connection and view the diagnostic messages, connect the debug-USB cable from the debug-USB port on the Qualcomm® IQ-9 Beta Evaluation Kit device to the Linux host.

               .. image:: ../../media/k2c-qli-build-ga/uart_ridesx.png

            .. group-tab:: Qualcomm® IQ-9075 EVK

               To set up the debug UART connection and view the diagnostic messages, connect the debug-USB cable from the debug-USB port on the Qualcomm® IQ-9075 EVK device to the Linux host.

               .. image:: ../../media/k2c-qli-build-ga/uart_iq9075_evk.png

      .. group-tab:: QCS8275   

         To set up the debug UART connection and view the diagnostic messages, connect the debug-USB cable from the debug-USB port on the Qualcomm® IQ-8 Beta Evaluation Kit device to the Linux host.

         .. image:: ../../media/k2c-qli-build-ga/uart_ridesx.png

1. Install Minicom on the Linux host:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install minicom

2. Check the USB port:

   .. container:: nohighlight
      
      ::

         ls /dev/ttyUSB*

   **Sample output**

   ``/dev/ttyUSB0``

3. Open Minicom:

   .. container:: nohighlight
      
      ::

         sudo minicom -s

4. Use the Down arrow key to select the :guilabel:`Serial port setup` option. Use the Up and Down arrow keys to navigate through the menu.

   .. image:: ../../media/k2c-qli-build-ga/serial_port_setup.jpg
      :align: center

5. Set up the serial device configuration:

   .. note:: Ensure that the letters are in uppercase.

   a. Select :guilabel:`A` on your keyboard to set up the serial device name such as ``/dev/ttyUSB0``.

   #. Select :guilabel:`Enter` to save the changes.

   #. Select :guilabel:`E` on your keyboard to set the baud rate and 8N1 configuration:
   
      i. Select the :guilabel:`E` key again if the baud rate isn't set to **115200**.

      #. Select the :guilabel:`Q` key if the configuration isn't set to **8N1**.

         .. image:: ../../media/k2c-qli-build-ga/option_Q.png
            :align: center

   #. Select :guilabel:`Enter` to save the changes.

   #. Select :guilabel:`F` on your keyboard to set the **Hardware Flow Control**
      to ``No``.

      .. image:: ../../media/k2c-qli-build-ga/serial_device_configuration.png
         :align: center

   #. Select :guilabel:`Enter` to save the changes.

6. Select the :guilabel:`Save setup as dfl` option and then select :guilabel:`Enter`.

   .. image:: ../../media/k2c-qli-build-ga/save_setup_as_dfl.png
      :align: center

7. Select :guilabel:`EXIT` to open the UART console and then select :guilabel:`Enter`.

8. Sign in to the UART console:

   -  Login: ``root``
   -  Password: ``oelinux123`` 
     
      .. note:: If the sign in console doesn't display as expected, verify the USB connection. If the issue persists, disconnect and then reconnect the micro-USB.

.. note:: 
    If you want to run sample applications from the UART shell, remount the root file system with write permissions:

    .. container:: nohighlight
      
       ::

         mount -o rw,remount /

Connect to ADB
^^^^^^^^^^^^^^^^
See `Install and connect to ADB <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-253/faqs.html>`__.

.. _connect_to_network:

Connect to the network
^^^^^^^^^^^^^^^^^^^^^^^

.. _howto_setup_wifi_sub:

**Set up Wi-Fi**

Wi-Fi is operational in Station mode. The Wi-Fi host driver and the authentication for network management are initialized during the device boot up.

1. Connect to the Wireless Access Point (Wi-Fi Router):

   .. container:: nohighlight
      
      ::

         nmcli dev wifi connect <WiFi-SSID> password <WiFi-password>

   **Example**

   .. container:: nohighlight
      
      ::

         root@qcs6490-rb3gen2-vision-kit:~# nmcli dev wifi connect QualcommWiFi password 1234567890

   .. container:: screenoutput

      .. line-block:: 

         Device ‘wlan0’ successfully activated with ‘d7b990bd-3b77-4b13-b239-b706553abaf8’.

#. Check the connection and device status:

   .. container:: nohighlight
      
      ::

         nmcli -p device
   
   .. image:: ../../media/k2c-qli-build-ga/status_of_devices.png

#. Check the WLAN connection status and IP address:

   .. container:: nohighlight
      
      ::

         ifconfig wlan0

   .. image:: ../../media/k2c-qli-build-ga/chk_ip.png

#. Ensure that the connection is active by pinging any website:

   .. container:: nohighlight
      
      ::

         ping google.com

**Switching networks**

If you are already connected to a network and need to reconnect to another network, do the following:

1. Disconnect from the current network:

   .. container:: nohighlight
      
      ::

         nmcli c down QualcommWiFi

   .. container:: screenoutput

      .. line-block:: 
         
         Connection ‘QualcommWiFi’ successfully deactivated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/1)

#. Check the disconnect status:

   .. container:: nohighlight
      
      ::

         nmcli -p device

   .. image:: ../../media/k2c-qli-build-ga/status_of_devices_switch_network.png

#. Connect to a different Wi-Fi network:

   .. container:: nohighlight
      
      ::

         nmcli dev wifi connect QualcommAP password XXXXXXXXX

   .. container:: screenoutput

      .. line-block:: 
         
         Device ‘wlan0’ successfully activated with ‘6159ac7c-58c2-44fa-938f-45dcb544fac3’.

.. _use-ssh:

**Sign in using SSH**

Establish the :ref:`network connectivity <connect_to_network>` before connecting to SSH.

1. Locate the IP address of the RB3 Gen 2 device according to the type of network connection, using the UART console on the Linux host:

   For Ethernet:

   .. container:: nohighlight
      
      ::

         ifconfig eth2

   For Wi-Fi:

   .. container:: nohighlight
      
      ::

         ifconfig wlan0

#. Use the IP address from the Linux host to establish an SSH connection to the device:

   .. container:: nohighlight
      
      ::

         ssh root@ip-address

   **Example**

   ``ssh root@192.168.0.222``

#. Connect to the SSH shell using the following password:

   .. container:: nohighlight
      
      ::

         oelinux123

.. note:: 
   
   - Connect the remote host to the same Wi-Fi access point as the device.
   - To create a non-root user account, see `Create a non-root user account <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-253/faqs.html>`__.

Configure Ethernet with RJ45 port
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ethernet/RJ45 port is enabled as a downstream port of PCIe to USB controller (``renesas``). Ensure that the ``renesas_usb_fw.mem`` file is available at the ``var/usbfw`` directory.

.. note:: 
   - If the ``renesas_usb_fw.mem`` firmware isn't available at the ``var/usbfw`` directory, then :ref:`update USB and Ethernet controller firmware <update_usb_eth_controller>`.

To check if the USB to Ethernet controller is enumerated, run the following command:

.. container:: nohighlight
      
   ::

      lsusb

**Sample output**:

.. container:: screenoutput

   Bus 002 Device 003: ID 0b95:1790 ASIX Electronics Corp. AX88179 Gigabit Ethernet
   Bus 002 Device 002: ID 05e3:0625 Genesys Logic, Inc. USB3.2 Hub
   Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
   Bus 001 Device 002: ID 05e3:0610 Genesys Logic, Inc. Hub
   Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

Connect an RJ45 cable to the RB3 Gen 2 device.

To check the Ethernet IP address, run the following command:

.. container:: nohighlight
      
   ::

      ifconfig

**Sample output**:

.. note:: 10.219.0.106 is the IP address.

.. container:: screenoutput

   enP1p4s0u1u1 Link encap:Ethernet HWaddr A6:CD:9B:FD:C1:B5
            inet addr:10.219.0.106  Bcast:10.219.1.255  Mask:255.255.254.0
            inet6 addr: fe80::a370:7a00:8131:5a03/64 Scope:Link
            UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
            RX packets:1071 errors:0 dropped:0 overruns:0 frame:0
            TX packets:132 errors:0 dropped:0 overruns:0 carrier:0
            collisions:0 txqueuelen:1000
            RX bytes:60711 (59.2 KiB)  TX bytes:18342 (17.9 KiB)

.. _update_usb_eth_controller:

Update USB and Ethernet controller firmware
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you face USB or Ethernet connectivity issues on the RB3 Gen 2 device, consider updating the firmware for the USB controller.

.. rubric:: Prerequisites

- Upgrade the software as described in `Update Software <hhttps://docs.qualcomm.com/bundle/publicresource/topics/80-70018-253/qsg-landing-page.html#update-the-softwarew>`__ before updating the Renesas firmware.
- Connect the device to the Linux host through the USB Type-C cable.

.. note:: The following procedure is applicable only to Ubuntu 22.04 host. If you are using a Windows or macOS host, set up an Ubuntu virtual machine by following the instructions described in the `Virtual Machine Setup Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-41/getting-started.html>`__.

1. Register and sign in to `Renesas <https://www.renesas.com/>`__.

#. `Download the firmware <https://www.renesas.com/en/products/interface/usb-switches-hubs/upd720201-usb-30-host-controller#design_development>`__.

#. Create the ``usb_fw.img`` image and copy the USB firmware:

   .. container:: nohighlight
      
      ::

         dd if=/dev/zero of=usb_fw.img bs=4k count=240
         mkfs -t ext4 usb_fw.img
         mkdir usb_fw
         sudo mount -o loop usb_fw.img usb_fw/
         sudo cp -rf renesas_usb_fw.mem usb_fw
         sudo umount usb_fw

#. Start the device in Fastboot mode:

   .. container:: nohighlight
      
      ::

         adb root
         adb shell
         reboot bootloader

#. Check if the device is in Fastboot mode:

   .. container:: nohighlight
      
      ::

         fastboot devices

   .. container:: screenoutput

      .. line-block::

         7dc85f5e     fastboot

#. Flash the ``usb_fw.img`` image to the device:

   .. container:: nohighlight
      
      ::

         fastboot erase usb_fw
         fastboot flash usb_fw usb_fw.img
         fastboot reboot

   .. container:: screenoutput

      c:\>fastboot erase usb_fw
      Erasing 'usb_fw' FAILED (remote: 'Check device console.')
      fastboot: error: Command failed

#. Verify if the firmware is successfully updated:

   .. container:: nohighlight

      ::

         dmesg

   Sample log after the firmware is successfully updated.

   .. container:: screenoutput

      [    6.589462] usbcore: registered new device driver onboard-usb-hub
      [    6.653277] usb 2-1: new SuperSpeed USB device number 2 using xhci_hcd
      [    7.013061] usb 2-1.1: new SuperSpeed USB device number 3 using xhci_hcd
      [    7.120657] ax88179_178a 2-1.1:1.0 eth0: register 'ax88179_178a' at usb-0001:04:00.0-1.1, ASIX AX88179 USB 3.0 Gigabit Ethernet, 3e:9e:5e:ff:d3:fb
      [    7.120767] usbcore: registered new interface driver ax88179_178a