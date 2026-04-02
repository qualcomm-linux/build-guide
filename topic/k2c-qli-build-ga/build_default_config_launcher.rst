.. _launcher_build_default_config:

Build and flash default configuration
---------------------------------------

.. note:: Skip this section if you want to :ref:`Build your own configuration<launcher_build_own_config>`.

For information on the default configurations, see the table *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/doc/80-70029-300/>`__.

.. _launcher_step1:

1. Compile the default build.

   .. _launcher_compile_step:

   a. After the download of the selected compilable distribution or flashable binary is complete, select :guilabel:`Compile` to start compiling. Depending on the size of the downloaded software and the host computer configuration, the compilation process can take a few hours.

      .. image:: ../../media/k2c-qli-build-ga/download.png

   b. To view the compilation progress, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png
   
   .. note:: BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 1a <launcher_compile_step>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

2. Flash the software with the default configuration.

   .. note:: Before you flash the software, ensure the following:

      1. Device is in :ref:`Emergency download (EDL) mode <move_to_EDL>`.
      #. :ref:`Provision universal flash storage (UFS) <provision_ufs>`.
      #. :ref:`Flash configuration data table (CDT) <flash_CDT>`.
      #. :ref:`Flash SAIL <flash_sail>`.

   a. Flash the software by selecting the device on which you want to flash the compiled software. If you want to connect many devices, then select the correct target device.

      .. image:: ../../media/k2c-qli-build-ga/flash.png

   #. Select :guilabel:`Flash on device`. The page is updated and displays a progress bar as QSC Launcher begins flashing the software. Leave the device connected while you flash the software.

   #. To view the compilation progress of individual software images, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_logs.png

   #. When the process is complete, the page displays a *Flash Complete* message.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_flashComplete.png

   #. Select :guilabel:`Done`.

      .. note:: The device reboots after the flashing procedure completes successfully. To verify the updated software version, see `Verify the Qualcomm Linux version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70029-251/set_up_the_device.html#verify-the-qualcomm-linux-version>`__.
