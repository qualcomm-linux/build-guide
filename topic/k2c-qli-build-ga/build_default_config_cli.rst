Build the default configuration
--------------------------------

.. _compile_qsc_cli:

Compile
''''''''

For information on the default configurations, see the table *Default values of MACHINE and DISTRO parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.

When the download is complete, start the compilation process. Depending on the size of the software and the host computer configuration, the compilation process can take a few hours.

.. container:: nohighlight
   
   ::

      qsc-cli chip-software compile --workspace-path '<Base_Workspace_Path>'
      
      # Example, qsc-cli chip-software compile --workspace-path '/local/mnt/workspace/sample_workspace'

This process builds the necessary Qualcomm firmware and completes the Qualcomm Linux build.

.. note:: If you get a BitBake fetcher error, try :ref:`recompiling <recompile_qsc_cli>` to resolve the issue. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

.. _recompile_qsc_cli:

Recompile
'''''''''''

To recompile after any modifications to the software release, use your existing workspace built using QSC CLI:

.. container:: nohighlight
   
   ::

      qsc-cli chip-software compile --image '<Software_Image_Name>' --workspace-path '<Base_Workspace_Path>'
      
      # Example, qsc-cli chip-software compile --image LE.QCLINUX.2.0 --workspace-path '/local/mnt/workspace/sample_workspace'

For information on software image names (``--image``), see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.

Flash
'''''''''

.. note:: For QSC CLI to detect the connected devices and flash the software builds, install the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) on the host computer:

   .. container:: nohighlight
      
      ::

         qsc-cli chip-software delete-workspace
         qsc-cli login
         qsc-cli tool install --name quts --activate-default-license
         qsc-cli tool install --name qud --activate-default-license
         qsc-cli tool install --name pcat --activate-default-license

The ``qsc-cli --help`` command lists the help options.

For Ubuntu 22.04, you might see an issue while installing QUD, where you must enroll the public key on your Linux host for a successful QUD installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QTI/sign/`` directory.

.. note:: Before you flash the software, ensure the following:

   1. Device is in :ref:`Emergency Download (EDL) mode <move_to_EDL>`.
   #. :ref:`Provision UFS <provision_ufs>`.
   #. :ref:`Choose CDT <Choose_cdt>`.
   #. :ref:`Flash SAIL <flash_sail>`.
  
Flash a device:

.. container:: nohighlight
      
   ::

      qsc-cli chip-software flash --workspace-path <Base_Workspace_Path> --buildflavor "sa2150p_emmc" --serialnumber <serial number>
         
      # Example, qsc-cli chip-software flash --workspace-path '/local/mnt/workspace/sample_workspace' --serialnumber 'be116704'
      
The ``--buildflavor`` argument is optional and only required for devices that have multiple flavors. To list the build flavors, run the following command on the host computer:
      
.. container:: nohighlight
      
   ::

      qsc-cli chip-software flash --workspace-path <workspace path> --list-buildflavor

   - To find the `<serial number>`, run the following command on the host computer:

     .. container:: nohighlight
         
        ::
      
           pcat -devices

     **Sample output**
        
     .. container:: screenoutput

        Searching devices in Device Manager, please wait for a moment…
        ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
        NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

   - The device reboots after the flashing procedure completes successfully. To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-251/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.
