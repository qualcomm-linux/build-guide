.. _enable_dtb_overlay:

Enable camera overlays
^^^^^^^^^^^^^^^^^^^^^^

.. note:: This section applies only to QCS6490, IQ-9075, and IQ-8275. 

When you flash and boot ``qcom-multimedia-proprietary-image``, the upstream camera stack is enabled by default. You can change this setting by updating the EFI variable.

Update the EFI variable at runtime after the device boots. On the first boot, set the EFI variable using the sysfs node, then reboot to enable CAMX on subsequent boots.

Apply the DTB Overlay:

.. container:: nohighlight

  ::

     # The -n option is mandatory for EFI variables
     # Expect the exact data without trailing newline characters.
     echo -n "camx" > /tmp/overlay

     # The EFI variable used to control the overlays loaded during UEFI
     # boot is 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays.
     # Use efivar to write the data into the runtime EFI variable
     efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -w -f /tmp/overlay

     # Print the EFI variable for confirmation
     efivar -n 882f8c2b-9646-435f-8de5-f208ff80c1bd-VendorDtbOverlays -p

     # Reboot to reflect the changes
     sync
     reboot

Enable upstream video driver
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: This section applies only to the IQ‑615 EVK.

IQ‑615 EVK supports only the upstream video driver ``venus``. The video overlay functionality is not supported.

When you flash and boot ``qcom-multimedia-proprietary-image``, the upstream video driver is not enabled by default. This can be changed by updating the kernel module configuration.

Enable the upstream video driver:

.. container:: nohighlight

   ::

      update-alternatives --install /etc/modprobe.d/blacklist-video.conf blacklist-video /etc/modprobe.d/blacklist-video.conf.vidc 200

After rebooting the device, you can run ``lsmod`` to confirm if the changes have been applied.

.. container:: nohighlight

   ::

      lsmod | grep venus
      # Expected Output
      # venus_enc           28672   0
      # venus_dec           28672   0