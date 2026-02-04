.. _launcher_build_own_config:

Build your own configuration
------------------------------
To build your own configuration, compile the build for default machine configuration and then compile the LE.QCLINUX.1.0.r1 image with your own MACHINE and QCOM_SELECTED_BSP parameter values. For information on the supported machine configurations of the development kit, see the table *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

When compiling a software image other than ``LE.QCLINUX.1.0.r1``, ensure that you also compile both the software product and ``LE.QCLINUX.1.0.r1`` in the same order. For example, if you compile ``BOOT.MXF.1.0.c1``, ensure that you also compile the software product (such as ``QCM6490.LE.1.0``) and then ``LE.QCLINUX.1.0.r1``.

1. :ref:`Compile the build for the default machine configuration <launcher_build_default_config>`.

2. Compile the `LE.QCLINUX.1.0.r1` image with your own MACHINE and QCOM_SELECTED_BSP parameter values.
   
   a. To run the build commands for a specific configuration, select :guilabel:`Compile using terminal`.

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_page.png

      .. note:: Compilations initiated through the terminal aren't tracked by the Qualcomm Software Center, making it impossible to track their progress on the Download page.

         .. image:: ../../media/k2c-qli-build-ga/compile_terminal_dialog.png
      
   b. Select :guilabel:`Open Terminal`.

   c. Update the highlighted command with your own configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for the Qualcomm Dragonwing™ RB3 Gen 2 Core Development Kit, change the value of ``MACHINE`` in the preceding build command to ``qcs6490-rb3gen2-core-kit``.
   
   d. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

      ::

         cd <workspace-dir>/build/tmp/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image-rb3gen2-core-kit.rootfs.qcomflash/
         ls -al rootfs.img

3. To flash your build, see :ref:`Flash software images <flash_images>`.

Related topics
---------------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`
