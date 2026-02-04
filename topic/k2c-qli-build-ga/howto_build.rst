.. _howto_build:

Build
-------

Alternative Build Instrucions via Manifest
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Repo is a tool which can be used to download a list of git repositories from a `manifest <https://github.com/qualcomm-linux/qcom-manifest/>`__. Repo can be used
for syncing the Yocto meta layers needed for the build.

1. Install these packages in addition to the base system requirements to perform a repo-manifest based build 

   .. container:: nohighlight
      
      ::

         sudo apt install repo python3-yaml

#. Download Qualcomm Yocto and supporting layer

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>

         # Example, <manifest-release-tag> is qcom-6.6.200-QLI.2.0-Ver.1.0.xml
         repo init -u https://github.com/qualcomm-linux/qcom-manifest -m <manifest-release-tag>.xml

         repo sync

#. Set up the Yocto build environment:

  .. container:: nohighlight
    
    ::

        # setup-environment provides a help section for instructions
        # Run the script with --help to view all supported flags
        setup-environment --help

        # machine and distro flags refer to machine and distro configuration files present in `meta-qcom/ci` directory.
        # setup-environment: Sets the environment settings, creates the build directory build,
        # and enters into build directory.
        source setup-environment --machine meta-qcom/ci/qcs9100-ride-sx.yml --distro meta-qcom/ci/qcom-distro.yml

#. Build the software image:

  .. container:: nohighlight
    
    ::

        # Build required image using bitbake `bitbake qcom-multimedia-image` 
        bitbake <image-recipe>

Check if the build is complete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         # meta-qcom uses qcomflash IMAGE_FSTYPE to create a single tarball
         # containing all the relevant files to perform a full clean flash,
         # including partition metadata, boot firmware, ESP # partition and
         # the rootfs.
         cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs-<DATE>.qcomflash/
         ls -al rootfs.img

.. _how_to_build_generate_sdk:

Generate an eSDK
^^^^^^^^^^^^^^^^^^^^^^^^^

**Get a Docker shell and shell prompt**

1. List the Docker images:

   .. container:: nohighlight
      
      ::

         docker images

   The output for this command is as follows:

   .. container:: screenoutput

      .. line-block::

         REPOSITORY                                                           TAG                         IMAGE ID       CREATED        SIZE
         032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/ql-tool     20.04.20231220102843864.9   864b345bd707   2 months ago   715MB
         032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/le.um-k2c   20.04.20231215014450998.7   4678dda58a91   2 months ago   929MB

#. Attach the container:

   .. container:: nohighlight
      
      ::

         WORKSPACE=<WORKSPACE_PATH> && SRC_DIR=<SoftwareImage> && docker run --rm  -it -v ~/.qualcomm_launcher_workspace_config:/var/tmp/.docker_qualcomm_launcher_setup/ -v $WORKSPACE:$WORKSPACE -e LOCAL_USER_NAME=`id -u -n` -e LOCAL_USER_ID=`id -u` -e USER=`id -u` -e WORKSPACE=$WORKSPACE -w $WORKSPACE/$SRC_DIR <REPOSITORY:TAG> bash

         # Example
         WORKSPACE=/local/mnt/workspace/Qworkspace/DEV && SRC_DIR=LE.QCLINUX.1.0.r1 && docker run --rm  -it -v ~/.qualcomm_launcher_workspace_config:/var/tmp/.docker_qualcomm_launcher_setup/ -v $WORKSPACE:$WORKSPACE -e LOCAL_USER_NAME=`id -u -n` -e LOCAL_USER_ID=`id -u` -e USER=`id -u` -e WORKSPACE=$WORKSPACE -w $WORKSPACE/$SRC_DIR 032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/le.um-k2c:20.04.20231215014450998.7 bash

#. Check if you are in a workspace that has ``.repo`` in it.

**Set up the environment and generate eSDK**

1. After building the ``meta-qcom-hwe`` with QSC CLI:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake -c do_populate_sdk_ext <image>
         # Example, bitbake -c do_populate_sdk_ext qcom-multimedia-image

   To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.    

#. After building with ``meta-qcom-extras`` and firmware sources with
   QSC CLI:

   .. note:: This step isn't applicable for public developers (unregistered).

   .. container:: nohighlight
      
      ::

         # Example
         export EXTRALAYERS="meta-qcom-extras"
         export CUST_ID="213195"
         export FWZIP_PATH="/local/mnt/workspace/extras/DEV/QCM6490.LE.1.0/common/build/ufs/bin"
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake -c do_populate_sdk_ext qcom-multimedia-image

#. After building standalone instructions within the same shell (shell
   where the build is successful):

   .. container:: nohighlight
      
      ::

         bitbake -c do_populate_sdk_ext <image>

         # Example
         bitbake -c do_populate_sdk_ext qcom-multimedia-image

#. After building with standalone instructions and with a new shell
   (assuming the build workspace exists):

   .. container:: nohighlight
      
      ::

         cd <workspace_path>
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake -c do_populate_sdk_ext <image>

         # Example
         cd /local/mnt/workspace/LE.QCLINUX.1.0.r1
         MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake -c do_populate_sdk_ext qcom-multimedia-image

#. After building with standalone instructions using Dockerfile.

   a. Move the control to the workspace directory:

      .. container:: nohighlight
      
         ::

            cd /local/mnt/workspace/qcom-download-utils/<release>

            # Example
            cd /local/mnt/workspace/qcom-download-utils/qcom-6.6.116-QLI.1.7-Ver.1.1

   #. Set up the environment and issue an eSDK build:

      .. container:: nohighlight
      
         ::

            MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            bitbake -c do_populate_sdk_ext <image>

            # Example
            MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            bitbake -c do_populate_sdk_ext qcom-multimedia-image

When the eSDK generation is complete, you can see the images in the following directory: ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/sdk``.

**Troubleshoot eSDK generation – basehash mismatch**

**Error excerpt**

.. container:: screenoutput

   .. line-block::

      ERROR: When reparsing /local/mnt/workspace/extras/DEV/LE.QCLINUX.1.0.r1/build-qcom-wayland/conf/../../layers/meta-qcom-distro/recipes-products/images/qcom-multimedia-image.bb:do_populate_sdk_ext, the basehash value changed from 7bce27b0510cb666f1bba1d03f055cfef48f9db2eabc17d490e14bbe4c632eba to 48ccd9d7370e0bf2435aa8b5067162932e07a3832adfa6ca037aa0ddb765c8de. The metadata isn't deterministic and this needs to be fixed.
      ERROR: The following commands may help:
      ERROR: $ bitbake qcom-multimedia-image -cdo_populate_sdk_ext -Snone
      ERROR: Then:
      ERROR: $ bitbake qcom-multimedia-image -cdo_populate_sdk_ext -Sprintdiff

**Solution**

Rebuild the image and generate the eSDK again.

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
     - Image recipe with upstream multimedia software components
   * - ``qcom-multimedia-proprietary-image`` 
     - Image recipe with proprietary multimedia software components
