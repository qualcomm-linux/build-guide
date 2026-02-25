.. _howto_build:

Build
-------

.. _build_tip:

Build meta-qcom tip
^^^^^^^^^^^^^^^^^^^^^^

The :ref:`build from source <build_from_source_github>` instructions explain how to build a milestone release
tag for ``meta-qcom``. The milestone release provides locked revisions for the well-tested dependent meta layers. If you wish to build ``meta-qcom`` with the branch tips for the dependent meta-layers, then follow these commands:

.. note:: Ensure that all the packages defined in the :ref:`host computer requirements <host_machine_requirements_reg_unreg>` are installed on your computer.

#. Clone the ``qualcomm-linux/meta-qcom`` repository. This holds the kas configuration files required to build.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>

         git clone https://github.com/qualcomm-linux/meta-qcom -b master 

#. Build the software image. Build targets are defined based on machine and distro combinations. 

   .. container:: nohighlight
      
      ::

         kas build meta-qcom/ci/<machine.yml>:meta-qcom/ci/<distro.yml>:meta-qcom/ci/linux-qcom-6.18.yml

         # Example, kas build meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml

   For various ``<machine>`` and ``<distro>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.

.. _build_manifest:

Alternative build instructions using Manifest
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Repo is a tool which can be used to download a list of git repositories from a `manifest <https://github.com/qualcomm-linux/qcom-manifest/>`__. Use the Repo to sync the Yocto meta layers that the build requires.

1. Install these packages in addition to the base system requirements to perform a repo-manifest based build:

   .. container:: nohighlight
      
      ::

         sudo apt install repo python3-yaml

#. Download the Qualcomm Yocto and supporting layers:

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>

         # Example, <manifest-release-tag> is qli-2.0-rc1.xml
         repo init -u https://github.com/qualcomm-linux/qcom-manifest -b main -m <manifest-release-tag>.xml

         repo sync

#. Set up the Yocto build environment:

  .. container:: nohighlight
    
    ::

        # setup-environment provides a help section for instructions
        # Run the script with --help to view all the supported flags
        setup-environment --help

        # machine and distro flags refer to the machine and distro configuration files present in `meta-qcom/ci` directory.
        # setup-environment sets the environment settings, creates the build directory build, and enters the build directory.
        source setup-environment --machine meta-qcom/ci/qcs9100-ride-sx.yml --distro meta-qcom/ci/qcom-distro-prop-image.yml --kernel meta-qcom/ci/linux-qcom-6.18.yml

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
         cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs.qcomflash/
         ls -al rootfs.img

Modifying recipes
^^^^^^^^^^^^^^^^^^

#. Modify and compile a recipe from the same workspace:

   .. container:: nohighlight
      
      ::

         # You can use devtool to modify the source-code used in any of the recipes
         kas shell -c "devtool modify <recipe>" meta-qcom/ci/<machine.yml>:meta-qcom/ci/<distro.yml>:meta-qcom/ci/lock.yml
         # Example, kas shell -c "devtool modify linux-qcom" meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/lock.yml

         # Build your recipe
         kas shell -c "bitbake <recipe>" meta-qcom/ci/<machine.yml>:meta-qcom/ci/<distro.yml>:meta-qcom/ci/lock.yml
         # Example, kas shell -c "bitbake linux-qcom" meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/lock.yml

.. _how_to_build_generate_sdk:

Generate an SDK
^^^^^^^^^^^^^^^^

Set up the environment and generate SDK from the same workspace:

.. container:: nohighlight
      
   ::

      kas shell -c "bitbake -c do_populate_sdk <image>" meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/lock.yml

When the SDK generation is complete, you can see the images in the ``<workspace-dir>/build/tmp/deploy/sdk`` directory.

Generate an eSDK
^^^^^^^^^^^^^^^^^

Set up the environment and generate eSDK from the same workspace:

.. container:: nohighlight
      
   ::

      kas shell -c "bitbake -c do_populate_sdk_ext <image>" meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/lock.yml

When the eSDK generation is complete, you can see the images in the ``<workspace-dir>/build/tmp/deploy/sdk`` directory.

Clean build artifacts
^^^^^^^^^^^^^^^^^^^^^^

1. Clean the build artifacts:

   .. container:: nohighlight

      ::

         kas shell -c "bitbake -fc do_cleansstate <recipe>" meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/lock.yml

#. Rebuild the recipe after cleaning up the artifacts: 

   .. container:: nohighlight
      
      ::

         kas shell -c "bitbake <recipe>" meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/lock.yml

Build a standalone QDL
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Prerequisites**

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

Image recipes supported in the Source workflow
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
