Use QSC CLI
--------------

This section explains how to configure, download, compile, and flash Qualcomm Linux using the QSC CLI.

.. _qsc_cli_software_download:

Download the software
^^^^^^^^^^^^^^^^^^^^^^

-  Download a software release by specifying the absolute workspace path, product ID, distribution, and release ID:

   .. note:: If you are downloading more than one distribution, create a new workspace for each distribution that you download.

   .. container:: nohighlight

      ::

         qsc-cli chip-software download --workspace-path '<Base_Workspace_Path>' --product '<Product_ID>' --distribution '<Distribution>' --release '<Release_ID>'
         # Example, qsc-cli chip-software download --workspace-path '/local/mnt/workspace/sample_workspace' --product 'QCM6490.LE.1.0' --distribution 'Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem' --release 'r00270.1'

   .. note::
      - For the Product_ID, Distribution, and Release_ID values, see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.
      - For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27/qualcomm_linux_metadata_layers_overview.html#qualcomm-linux-metadata-layers>`__.

Build the default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _compile_qsc_cli:

Compile
''''''''
.. note:: For information on the default configurations, see the table *Default values of MACHINE and QCOM_SELECTED_BSP parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

When the download is complete, start the complilation process:

.. note:: Depending on the size of the software and the host computer configuration, the compilation process can take a few hours.

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
      
      # Example, qsc-cli chip-software compile --image LE.QCLINUX.1.0.r1 --workspace-path '/local/mnt/workspace/sample_workspace'

.. note:: For information on software image names (``--image``), see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

Flash
'''''''''

.. note:: For the QSC CLI to detect the connected devices and flash the software builds, install the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) on the host computer. Use the ``qsc-cli chip-software delete-workspace`` command to install PCAT and QUD:

   .. container:: nohighlight
      
      ::

         qsc-cli login
         qsc-cli tool install --name quts --activate-default-license
         qsc-cli tool install --name qud --activate-default-license
         qsc-cli tool install --name pcat --activate-default-license

The ``qsc-cli --help`` command lists the help options.

For Ubuntu 22.04, you might see an issue while installing QUD, where you must enroll the public key on your Linux host for a successful QUD installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QTI/sign/`` directory.

.. note:: Before you flash the software, ensure the following:

   1. Device is in :ref:`Emergency Download (EDL) mode <move_to_EDL>`.
   #. :ref:`Provision UFS <provision_ufs>`.
   #. :ref:`Flash CDT <flash_cdt>`.
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

.. note::
   - To find the `<serial number>`, run the following command on the host computer:

     .. container:: nohighlight
         
        ::
      
           pcat -devices

     **Sample output**
        
     .. container:: screenoutput

        Searching devices in Device Manager, please wait for a moment…
        ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
        NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

   - The device reboots after the flashing procedure completes successfully. To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-253/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.

.. _build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To build your own configuration, you must compile the build for default machine configuration and compile the LE.QCLINUX.1.0.r1 image with your own MACHINE and QCOM_SELECTED_BSP parameter values.

1. Compile the build for the default machine configuration:

   a. :ref:`Download the software <qsc_cli_software_download>`.
   
   #. :ref:`Compile the default build <compile_qsc_cli>`.
   
2. Compile the ``LE.QCLINUX.1.0.r1`` image with your own MACHINE and QCOM_SELECTED_BSP parameter values.
   
   .. note:: For information on the supported machine configurations of the development kit, see the table *Default values of MACHINE and QCOM_SELECTED_BSP parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.
   
   a. Run the build commands for a specific configuration:

      .. container:: screenoutput
         
         ::

            qsc-cli chip-software open-build-env --workspace-path <Base_Workspace_Path> --image <Software_Image_Name>
            # Example, qsc-cli chip-software open-build-env --workspace-path '/local/mnt/workspace/sample_workspace' --image 'LE.QCLINUX.1.0.r1' 

      This command opens the terminal.
   
      .. note:: An environment is set up to run your own build commands for a specific software image. QSC won't track the status of input workspaces in the future releases and flash using ``qsc-cli`` won't be supported for these workspaces.

   b. Update the highlighted command according to your own machine configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for the Qualcomm Dragonwing™ RB3 Gen 2 Core Development Kit, change the value of ``MACHINE`` in the preceding build command to ``qcs6490-rb3gen2-core-kit``.
   
   c. After a successful build, check that the ``system.img`` file is in the ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory with an updated timestamp. For example:

      .. container:: nohighlight
      
         ::

            cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image
            ls -al system.img
         
   .. note::      
       When compiling a software image other than ``LE.QCLINUX.1.0.r1``, ensure that you also compile both the software product and ``LE.QCLINUX.1.0.r1`` in the same order.
 
       For example, if you compile ``BOOT.MXF.1.0.c1``, ensure that you also compile the software product (such as ``QCM6490.LE.1.0``) and then ``LE.QCLINUX.1.0.r1``.
      
3. To flash your build, see :ref:`Flash software images <flash_images>`.

   .. note::
      - Before flashing, update the build images path to the compiled build images workspace at ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image``.

        For example, ``<Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image``.

Related topics
---------------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`