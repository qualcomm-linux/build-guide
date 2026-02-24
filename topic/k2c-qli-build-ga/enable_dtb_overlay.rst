.. note::

   When you flash and boot ``qcom-multimedia-proprietary-image`` the upstream
   camera stack is enabled by default. This can be changed by updating the
   EFI variable setting.

   EFI variable setting needs to be done at runtime, after booting the device.
   On first boot, set the EFI variable via the sysfs node and reboot to get
   CAMX enabled in subsequent boots.

   Run the following commands to apply the DTB Overlay:

   .. admonition:: Commands
      :class: note

      .. code-block:: bash

         # The -n option is mandatory because EFI variables
         # expect exact data without trailing newline characters.
         echo -n "camx" > /tmp/overlay

         # The EFI variable used to control the overlays loaded during UEFI
         # boot is 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays.
         # Use efivar to write the data into the runtime EFI variable
         efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -w -f /tmp/overlay

         # Print the EFI variable for confirmation
         efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -p

         # Reboot for the changes to be reflected
         sync
         reboot
