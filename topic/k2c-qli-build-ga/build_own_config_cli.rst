.. _build_own_config:

Build your own configuration
-----------------------------
To build your own configuration, you must compile the build for default machine configuration and compile the LE.QCLINUX.1.0.r1 image with your own MACHINE and QCOM_SELECTED_BSP parameter values.

When compiling a software image other than ``LE.QCLINUX.1.0.r1``, ensure that you also compile both the software product and ``LE.QCLINUX.1.0.r1`` in the same order. For example, if you compile ``BOOT.MXF.1.0.c1``, ensure that you also compile the software product (such as ``QCM6490.LE.1.0``) and then ``LE.QCLINUX.1.0.r1``.

1. Compile the build for the default machine configuration:

   a. :ref:`Download the software <qsc_cli_software_download>`.
   
   #. :ref:`Compile the default build <compile_qsc_cli>`.
   
2. Compile the ``LE.QCLINUX.1.0.r1`` image with your own MACHINE and QCOM_SELECTED_BSP parameter values.
   
   For information on the supported machine configurations of the development kit, see the table *Default values of MACHINE and QCOM_SELECTED_BSP parameters for QSC* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.
   
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
   
   c. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

      .. container:: nohighlight
      
        ::

           # meta-qcom uses qcomflash IMAGE_FSTYPE to create a single tarball
           # containing all the relevant files to perform a full clean flash,
           # including partition metadata, boot firmware, ESP # partition and
           # the rootfs.
           cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs-<DATE>.qcomflash/
           ls -al rootfs.img
         
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
