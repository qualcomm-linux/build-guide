.. _sync_build_flash:

Use QSC CLI
--------------

This section explains how to configure, download, compile, and flash Qualcomm Linux using the QSC CLI.

.. _qsc_cli_software_download:

Software download
^^^^^^^^^^^^^^^^^^^

-  Download a software release by specifying the absolute workspace path, product ID, distribution, and release ID as shown in the following example:

   ::

      qsc-cli download --workspace-path '<Base_Workspace_Path>' --product '<Product_ID>' --distribution '<Distribution>' --release '<Release_ID>'
      # Example, qsc-cli download --workspace-path '/local/mnt/workspace/sample_workspace' --product 'QCM6490.LE.1.0' --distribution 'Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem' --release 'r00270.1'

   .. note::
      - If you are downloading more than one distribution, create a new workspace for each distribution that you download.
      - For the Product_ID, Distribution, and Release_ID values, see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
      - For more information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#qualcomm-linux-metadata-layers-overview>`__.

.. _section_yhy_11w_q1c_vinayjk_03-07-24-006-28-270:

Build default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _compile_qsc_cli:

Compile
''''''''
.. note:: For information on the default configurations, see the table *Default values of MACHINE and QCOM_SELECTED_BSP parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

Start the compilation process after the download is complete:

.. note:: Depending on the size of the software and the host machine configuration, the compilation process may take a few hours.

::

   qsc-cli compile --workspace-path '<Base_Workspace_Path>'
    
   # Example
   qsc-cli compile --workspace-path '/local/mnt/workspace/sample_workspace'

This process builds the necessary Qualcomm firmware and completes the Qualcomm Linux build.

.. note:: If you encounter a BitBake fetcher error, try recompiling to resolve the issue. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

.. _section_qb2_sp1_q1c_vinayjk_03-04-24-129-15-322:

Recompile
'''''''''''

To recompile after any modifications to the software release, use your existing workspace built using QSC CLI:

::

   # qsc-cli compile --image '<Software_Image_Name>' --workspace-path '<Base_Workspace_Path>'
    
   # Example
   qsc-cli compile --image LE.QCLINUX.1.0.r1 --workspace-path '/local/mnt/workspace/sample_workspace'

.. note:: For information on software image names (``--image``), see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

.. _section_x2k_vnf_w1c:

Flash
'''''''''

.. note:: For the QSC CLI to detect the connected devices and flash the software builds, ensure that the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) are installed on the host machine. Use the ``qpm-cli`` command to install PCAT and QUD:

   ::

      qpm-cli --login
      qpm-cli --install pcat --activate-default-license
      qpm-cli --install qud --activate-default-license

The ``qpm-cli --help`` command lists the help options.

For Ubuntu 22.04, you may encounter an issue while installing QUD, where you might be asked to enroll the public key on your Linux host for a successful QUD installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QTI/sign/`` directory.

.. note:: Before you flash the software, ensure that the device is in Emergency Download (EDL) mode. For more information on how to force the device into EDL mode, see :ref:`Move to EDL mode <section_vgg_mly_v1c>`.
  
1. Flash a device.

   ::

      qsc-cli flash --workspace-path <Base_Workspace_Path> --buildflavor "sa2150p_emmc" --serial <serial number>
      # Example:
      qsc-cli flash --workspace-path '/local/mnt/workspace/sample_workspace' --serial 'be116704'
   
   The ``--buildflavor`` argument is optional and only required for devices that have multiple flavors. To list the build flavors, run the following command on the host machine:
      
   ::

      qsc-cli flash --workspace-path <workspace path> --list-buildflavor

   .. note::
      - To find the `<serial number>`, run the following command on the host machine:

        ::
      
          pcat -devices

        **Sample output**
        
        ::

            Searching devices in Device Manager, please wait for a moment…
            ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
            NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

      - The device reboots after the flashing procedure is completed successfully. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-253/ubuntu_host.html#check-software-version>`__.

.. _build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To build your own configuration, you must compile the build for default machine configuration and compile the LE.QCLINUX.1.0.r1 image with your own MACHINE and QCOM_SELECTED_BSP parameter values.

1. Compile the build for the default machine configuration:

   a. :ref:`Download the software <qsc_cli_software_download>`.
   
   #. :ref:`Compile the default build <compile_qsc_cli>`.
   
2. Compile the ``LE.QCLINUX.1.0.r1`` image with your own MACHINE and QCOM_SELECTED_BSP parameter values.
   
   .. note:: For information on the supported machine configurations of the development kit, see the table *MACHINE and QCOM_SELECTED_BSP parameter value* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
   
   a. Run the build commands for a specific configuration:

      ::

         qsc-cli open-build-env --workspace-path <Base_Workspace_Path> --image <Software_Image_Name>
         # Example
         qsc-cli open-build-env --workspace-path '/local/mnt/workspace/sample_workspace' --image 'LE.QCLINUX.1.0.r1' 

      This command opens the terminal.
   
      .. note:: An environment is setup to run your own build commands for a specific software image. QSC will not track the status of input workspaces in the future releases and flash using ``qsc-cli`` will not be supported for these workspaces.

   b. Update the highlighted command according to your own machine configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for Qualcomm® RB3 Gen 2 Core Development Kit, change the value of ``MACHINE`` in the preceding build command to ``qcs6490-rb3gen2-core-kit``.
   
   c. After a successful build, check that the ``system.img`` file is in the ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory with an updated timestamp. For example:

      ::

         cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image
         ls -al system.img
         
   .. note::      
       When compiling a software image other than ``LE.QCLINUX.1.0.r1``, ensure that you also compile both the software product and ``LE.QCLINUX.1.0.r1`` in the same order.
 
       For example, if you compile ``BOOT.MXF.1.0.c1``, ensure that you also compile the software product (such as ``QCM6490.LE.1.0``) and then ``LE.QCLINUX.1.0.r1``.
      
3. To flash your build, see :ref:`Flash images <flash_images>`.

   .. note::
      - Before flashing, update the build images path to the compiled build images workspace at ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image``.

        For example, ``<Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image``.

To establish UART and network connections, see :ref:`Connect to UART and network <connect_uart_network>`.