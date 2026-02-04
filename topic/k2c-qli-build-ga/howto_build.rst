.. _howto_build:

Build
-------

Check if the build is complete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         cd <workspace-dir>/build/tmp/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image-rb3gen2-core-kit.rootfs.qcomflash/
         ls -al rootfs.img

Rebuild using a Docker environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Run the commands to connect to Docker for your environment setup and then use the BitBake commands to rebuild:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/DEV/<softwareimage>
         # Example, cd /local/mnt/workspace/Qworkspace/DEV/LE.QCLINUX.1.0.r1 for making changes to Yocto layers
         # Make code changes

#. Get to a Docker shell as mentioned in :ref:`Generate an eSDK <how_to_build_generate_sdk>`.

#. Rebuild using your source changes:

   .. container:: nohighlight
      
      ::

         # Rebuild commands
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake qcom-multimedia-image

   To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. Build image ``qcom-multimedia-test-image``:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake qcom-multimedia-test-image

Build a standalone QDL
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Prerequisites:**

  - The modules ``make`` and ``gcc`` must be available.

  - Install the following dependent packages:

    .. container:: nohighlight
      
       ::

         sudo apt-get install git libxml2-dev libusb-1.0-0-dev pkg-config

1. Download and compile the Linux flashing tool (QDL):

   .. container:: nohighlight
      
      ::

         mkdir <qdl_download_path>
         git clone --branch master https://github.com/linux-msm/qdl
         cd qdl
         make

2. Flash using the generated QDL:

   .. container:: nohighlight
      
      ::

         # Built images are under <workspace_path>/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/<IMAGE>
         # build_path: For DISTRO=qcom-wayland, it's build-qcom-wayland. 
         #             For DISTRO=qcom-robotics-ros2-humble, it's build-qcom-robotics-ros2-humble
         # qdl <prog.mbn> [<program> <patch> ...]
         # Example: build_path is build-qcom-wayland
         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         # For UFS storage
         cp ./partition_ufs/gpt_main*.bin ./partition_ufs/gpt_backup*.bin ./partition_ufs/rawprogram[0-9].xml ./partition_ufs/patch*.xml ./partition_ufs/zeros_*sectors.bin ./
         <qdl_download_path>/qdl/qdl --storage ufs prog_firehose_ddr.elf rawprogram*.xml patch*.xml
         # For EMMC storage
         cp ./partition_emmc/gpt_main*.bin ./partition_emmc/gpt_backup*.bin ./partition_emmc/rawprogram[0-9].xml ./partition_emmc/patch*.xml ./partition_emmc/zeros_*sectors.bin ./
         <qdl_download_path>/qdl/qdl --storage emmc prog_firehose_ddr.elf rawprogram0.xml patch0.xml

.. _change_hex_tool_install_path:

Change the Hexagon tool install path
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``HEXAGON_ROOT`` environment variable must point to the path where the Hexagon tools are installed. By default, the ``qsc-cli`` tool installs a ``HEXAGON_ROOT`` variable in the ``$HOME`` directory. You can also choose an alternate installation directory.

Use the ``––path`` option in ``qsc-cli`` command to install Hexagon tools in a directory of your choice and export the ``HEXAGON_ROOT`` variable to the same directory.

Provide an absolute path for ``<TOOLS_DIR>`` in ``qsc-cli`` and export commands as shown in the following example:

.. container:: nohighlight
      
   ::

      # Example
      
      mkdir -p <TOOLS_DIR>
      qsc-cli tool extract --name hexagon8.4 --required-version 8.4.07 --path <TOOLS_DIR>/8.4.07
      export HEXAGON_ROOT=<TOOLS_DIR>

.. _image_recipes_github_workflow:

Image recipes supported in the GitHub workflow
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table:: 
   :header-rows: 1
   :align: center

   * - Image recipe
     - Description
   * - ``qcom-minimal-image``
     - A minimal rootfs image that boots to shell
   * - ``qcom-console-image``   
     - Boot to shell with package group to bring in all the basic packages
   * - ``qcom-multimedia-image``  
     - Image recipe includes recipes for multimedia software components, such as, audio, Bluetooth\ :sup:`®`, camera, computer vision, display, and video.
   * - ``qcom-multimedia-test-image`` 
     - Image recipe that includes tests
