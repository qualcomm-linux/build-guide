.. note::

   When you flash and boot ``qcom-multimedia-proprietary-image`` for the first time,
   the upstream camera stack is enabled by default. Build time blacklisting cannot
   help here since blacklisting upstream CAMSS modules does not lead to CAMX being
   loaded.

   On first boot, the user needs to set the EFI variable via the sysfs node and
   reboot to get CAMX enabled in subsequent boots. EFI variable setting can only be
   done after booting up once.

   Run the following commands to apply the DTB Overlay:

   .. admonition:: Commands
      :class: note

      .. code-block:: bash

         # The -n option is mandatory because EFI variables
         # expect exact data without trailing newline characters.
         echo -n "camx" > /var/data

         # Use efivar to write the data into the runtime EFI variable
         # Format: <GUID>-<Variable Name>
         cd /sys/firmware/efi/efivar/
         efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -w -f /var/data

         # Print the EFI variable for confirmation
         efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -p

         # Reboot for the changes to be reflected
         sync
         reboot
