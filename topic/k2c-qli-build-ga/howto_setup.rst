.. _howto_setup:

Setup
------------------

.. _section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684:

How to connect to a UART shell?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To set up the debug UART connection and view diagnostic messages, connect the micro-USB cable from the micro-USB port on the RB3 Gen 2 device to the Linux host.

.. image:: ../../media/k2c-qli-build-ga/micro_usb_port.jpg

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

4. Press the Down arrow key to select the **Serial port setup** option.
   Use the Up and Down arrow keys to navigate through the menu.

   .. image:: ../../media/k2c-qli-build-ga/serial_port_setup.jpg
      :align: center

5. Set up the serial device configuration:

   a. Press **A** on your keyboard to set up the serial device name such
      as ``/dev/ttyUSB0``.

   #. Press **Enter** to save the changes.

   #. Press **E** on your keyboard to set the baud rate. If the baud
      rate is not set to **115200**, press the **E** key again.

   #. Press **Q** on your keyboard to set the configuration to **8N1**.

   #. Press **Enter** to save the changes.

   #. Press **F** on your keyboard to set the **Hardware Flow Control**
      to ``No``.

      .. note:: Ensure that the letters A, E, Q, and F are in uppercase.

      .. image:: ../../media/k2c-qli-build-ga/serial_device_configuration.png
         :align: center

   #. Press **Enter** to save the changes.

6. Select the **Save setup as dfl** option and press **Enter**.

   .. image:: ../../media/k2c-qli-build-ga/save_setup_as_dfl.png
      :align: center

7. Select **Exit** to open the UART console.

8. Log in to the UART console:

   -  Login: ``root``
   -  Password: ``oelinux123`` 
     
      .. note:: If the login console does not display as expected, verify the USB connection. If the issue persists, disconnect and then reconnect the micro-USB.

.. note:: 
    If you want to run sample applications from the UART shell, remount the root file system with write permissions:

    ::

      mount -o rw,remount /

.. _section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279:

How to SSH?
^^^^^^^^^^^^^^

.. _howto_setup_wifi_sub:

**Set up Wi-Fi**

Wi-Fi is operational in Station mode. The Wi-Fi host driver and the authentication for network management are initialized during the device boot up.

To update the Wi-Fi configuration, perform the following from the debug :ref:`UART serial console <section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684>`:

1. Stop the ``wpa_supplicant``:

   ::

      killall wpa_supplicant

#. Open the default ``/etc/wpa_supplicant.conf`` file using your preferred text editor and modify the content of the file to match the SSID and password of your router:

   .. note::
        You can check the configurations of security types specified in the default ``/etc/wpa_supplicant.conf`` file to add your required router configurations.

   ::

      # Only WPA-PSK is used. Any valid cipher combination is accepted.
      ctrl_interface=/var/run/sockets

      network={
      #Open
      #       ssid="example open network"
      #       key_mgmt=NONE
      #WPA-PSK-Configuration
      #  Update the SSID to match the Wi-Fi SSID of your router. 
      ssid="QSoftAP"
      #       proto=WPA RSN
      #       key_mgmt=WPA-PSK
      #       pairwise=TKIP CCMP
      #       group=TKIP CCMP
      # Update the password to match the Wi-Fi password of your router.
      psk="1234567890"
      #WEP-Configuration
      #       ssid="example wep network"
      #       key_mgmt=NONE
      #       wep_key0="abcde"
      #       wep_key1=0102030405
      #       wep_tx_keyidx=0
      }

#. Save the modified ``wpa_supplicant.conf`` file and verify its
   content:

   ::

      cat /etc/wpa_supplicant.conf

#. Reboot or power cycle the device. Wait for approximately one minute
   to establish a WLAN connection with the updated SSID and password.

#. **(Optional)** If you prefer not to reboot the device, run the
   following commands:

   ::

      wpa_supplicant -Dnl80211 -iwlan0 -ddd -c /etc/wpa_supplicant.conf -f /tmp/wpa_supplicant-log.txt &
      dhcpcd wlan0

#. Check the WLAN connection status and IP address:

   ::

      ifconfig wlan0

   .. image:: ../../media/k2c-qli-build-ga/setup_wifi_2.png

#. Ping the router to confirm the connection:

   ::

      ping qualcomm.com

**Connect to SSH**

Ensure that a :ref:`Wi-Fi connection <howto_setup_wifi_sub>` is established before connecting to SSH.

1. Find the IP address of the RB3 Gen 2 device in the UART console on the Windows host:

   ::

      ifconfig wlan0

#. Use the IP address obtained from **step 1** to SSH the device from the remost host:

   ::

      ssh root@ip-address

   **Example**

   ``ssh root@10.92.180.250``

#. Connect to the SSH shell using the following password:

   ::

      oelinux123

.. note:: Ensure that the remote host is connected to the same Wi-Fi access point.

.. _section_j5g_rds_5bc_vinayjk_06-21-24-1739-53-921:

How to configure Ethernet with RJ45 port?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Ethernet/RJ45 port is enabled as a downstream port of PCIe to USB
controller (``renesas``). Ensure that ``renesas_usb_fw.mem`` is
available at the ``/lib/firmware`` directory.

.. note:: 
   - If ``renesas_usb_fw.mem`` firmware is not available at the ``/lib/firmware`` directory, then :ref:`connect to UART <section_ags_ssh_p1c_vinayjk_03-01-24-1109-49-684>` and :ref:`enable the Wi-Fi <howto_setup_wifi_sub>`.
   - After getting SSH and IP address, :ref:`update PCIe to USB controller firmware <section_nsb_5gs_5bc_vinayjk_06-21-24-1803-34-149>`.

To check if USB to ETH controller is enumerated, run the following
command:

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

How to update USB and Ethernet controller firmware?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you encounter any USB or Ethernet connectivity issues on the RB3 Gen
2 device, a firmware update for the PCIe to USB controller could be a
solution.

**Prerequisite**: The device should be connected to the SSH terminal.

1. To download the
   `firmware <https://www.renesas.com/us/en/products/interface/usb-switches-hubs/upd720201-usb-30-host-controller>`__,
   you must register and log in with your ID.

#. Transfer the files using SCP to the following path:
   ``/lib/firmware``.

   .. note:: 
        The firmware name must be ``renesas_usb_fw.mem`` and if not, rename it. The driver identifies the firmware based on this specific filename, and other firmware names will not function.

   **Example**:

   ::

      scp renesas_usb.mem root@<ip address>:/lib/firmware

   .. note:: 
          When prompted for a password, enter ``oelinux123``.

.. note:: 
      After flashing the firmware, reboot the device. Upon subsequent boot-up, the driver reads the firmware and activates the PCIe-to-USB controller.



