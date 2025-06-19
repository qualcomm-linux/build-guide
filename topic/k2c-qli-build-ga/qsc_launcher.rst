Use QSC Launcher
-----------------

.. _launcher_download_sw:

Download the software
^^^^^^^^^^^^^^^^^^^^^^

1. To open the QSC Launcher desktop application, launch :guilabel:`Qualcomm Software Center` from the :guilabel:`Applications` menu.

   .. note:: 
      For the Launcher workflow to detect connected devices and flash software builds, install the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) tools on the host computer. Select :guilabel:`PCAT` to install PCAT and :guilabel:`QUD` to install QUD as shown in the following image:

      .. image:: ../../media/k2c-qli-build-ga/QSC_has_PCAT_QUD_install_info.png

      **Or**

      Install PCAT and QUD using ``qsc-cli``:

      .. container:: nohighlight
         
         ::

            qsc-cli login
            qsc-cli tool install --name quts --activate-default-license
            qsc-cli tool install --name qud --activate-default-license
            qsc-cli tool install --name pcat --activate-default-license

      The ``qsc-cli --help`` command lists the help options.

      For Ubuntu 22.04, you might see an issue while installing QUD where you must enroll the public key on your Linux host for a successful QUD installation. For more details, follow the steps provided in the ``signReadme.txt`` file available at the ``/opt/QTI/sign/`` directory.

2. Use your registered email ID to sign in to the QSC desktop application. The QSC Launcher dashboard page (for example, when you connect the QCS6490 Qualcomm Dragonwing\ :sup:`™` RB3 Gen 2 Core Development Kit) appears as shown in the following image:

   .. note:: To register, go to https://www.qualcomm.com/support/working-with-qualcomm.

   .. image:: ../../media/k2c-qli-build-ga/start_launcher_ab.png

   -  If you don't have a connected device, click :guilabel:`Start Launcher` (A).
   -  If you have a connected device, select :guilabel:`Start Launcher` (B) for the appropriate device in the :guilabel:`Connected devices` panel.

3. On the :guilabel:`Specify Environment` page, select the appropriate values for :guilabel:`Category`, :guilabel:`Chipset`, :guilabel:`Host Operating System`, :guilabel:`Target Operating System`, and then select :guilabel:`Next`.

   .. note:: See `chipsets (hardware SoCs) <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-115/soc.html>`__ that are supported on Qualcomm Linux.

   .. image:: ../../media/k2c-qli-build-ga/specify_env.png

4. In the :guilabel:`Select Resources` window, do the following:
   
   .. image:: ../../media/k2c-qli-build-ga/select_resource_page.png

   a. In the :guilabel:`Base Workspace Path` text box, specify a directory where you want to download the software.

   b. Select the :guilabel:`Software Product`.

   c. Select the :guilabel:`Distribution` and the :guilabel:`Release Tag`.

      .. note::
         
         - For information about the supported distributions for your hardware SoCs, see the table *Access Controlled Distribution* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.
         - For information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27/qualcomm_linux_metadata_layers_overview.html#qualcomm-linux-metadata-layers>`__.        
         - For information about the Qualcomm IM and QIR SDKs, see the following guides:

           - `Qualcomm IM SDK Quick Start Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-51>`__
           - `QIR SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-265>`__

5. To download the selected compilable distribution or flashable binary, select :guilabel:`Download`. After the software download is successful, a *Download complete* status appears.

   .. image:: ../../media/k2c-qli-build-ga/software_download_complete.png

   
   .. note:: You can also track the download progress through the :guilabel:`Downloads` option in the top menu bar.

   You don't have to compile flashable binaries. If you have selected a flashable binary, follow the on-screen instructions to flash to a connected device.

   .. image:: ../../media/k2c-qli-build-ga/prebuilt_compile.png

.. _launcher_build_default_config:

Build and flash default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _launcher_step1:

1. Compile the default build.

   .. note:: For information on the default configurations, see the table *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   .. _launcher_compile_step:

   a. After the download of the selected compilable distribution or flashable binary is complete, select :guilabel:`Compile` to start compiling. Depending on the size of the downloaded software and the host computer configuration, the compilation process can take a few hours.

      .. image:: ../../media/k2c-qli-build-ga/download.png

   b. To view the compilation progress, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png
   
   .. note:: BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 1a <launcher_compile_step>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

2. Flash the software with the default configuration.

   .. note:: Before you flash the software, ensure the following:

      1. Device is in :ref:`Emergency Download (EDL) mode <move_to_EDL>`.
      #. :ref:`Provision UFS <provision_ufs>`.
      #. :ref:`Flash CDT <flash_CDT>`.
      #. :ref:`Flash SAIL <flash_sail>`.

   a. Flash the software by selecting the device on which you want to flash the compiled software. If you connected many devices, then select the correct target device.

      .. image:: ../../media/k2c-qli-build-ga/flash.png

   b. Select :guilabel:`Flash on device`. The page is updated and displays a progress bar as QSC Launcher begins flashing the software. Leave the device connected while you flash the software.

   c. To view the compilation progress of individual software images, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_logs.png

   d. When the process is complete, the page displays a *Flash Complete* message.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_flashComplete.png

   e. Select :guilabel:`Done`.

      .. note:: The device reboots after the flashing procedure completes successfully. To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-253/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.

.. _launcher_build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
To build your own configuration, compile the build for default machine configuration and then compile the LE.QCLINUX.1.0.r1 image with your own MACHINE and QCOM_SELECTED_BSP parameter values.

1. :ref:`Compile the build for the default machine configuration <launcher_build_default_config>`.

2. Compile the `LE.QCLINUX.1.0.r1` image with your own MACHINE and QCOM_SELECTED_BSP parameter values.
   
   .. note:: For information on the supported machine configurations of the development kit, see the table *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   a. To run the build commands for a specific configuration, select :guilabel:`Compile using terminal`.

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_page.png

      .. note:: Compilations initiated through the terminal aren't tracked by the Qualcomm Software Center, making it impossible to track their progress on the Download page.

         .. image:: ../../media/k2c-qli-build-ga/compile_terminal_dialog.png
      
   b. Select :guilabel:`Open Terminal`.

   c. Update the highlighted command with your own configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for the Qualcomm Dragonwing™ RB3 Gen 2 Core Development Kit, change the value of ``MACHINE`` in the preceding build command to ``qcs6490-rb3gen2-core-kit``.
   
   d. After a successful build, check that the ``system.img`` file is in the ``<Base Workspace Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory with an updated timestamp. For example:

      ::

         cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image
         ls -al system.img
   
   .. note::      
      When compiling a software image other than ``LE.QCLINUX.1.0.r1``, ensure that you also compile both the software product and ``LE.QCLINUX.1.0.r1`` in the same order.
 
      For example, if you compile ``BOOT.MXF.1.0.c1``, ensure that you also compile the software product (such as ``QCM6490.LE.1.0``) and then ``LE.QCLINUX.1.0.r1``.

3. To flash your build, see :ref:`Flash software images <flash_images>`.

Related topics
---------------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`