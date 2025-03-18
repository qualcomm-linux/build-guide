.. _howto_build:

Build
-------

Check if the build is complete
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If your build instruction is ``bitbake qcom-multimedia-image``, check if the ``system.img`` is present in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

.. container:: nohighlight
      
   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      ls -al system.img

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

2. Attach the container:

   .. container:: nohighlight
      
      ::

         WORKSPACE=<WORKSPACE_PATH> && SRC_DIR=<SoftwareImage> && docker run --rm  -it -v ~/.qualcomm_launcher_workspace_config:/var/tmp/.docker_qualcomm_launcher_setup/ -v $WORKSPACE:$WORKSPACE -e LOCAL_USER_NAME=`id -u -n` -e LOCAL_USER_ID=`id -u` -e USER=`id -u` -e WORKSPACE=$WORKSPACE -w $WORKSPACE/$SRC_DIR <REPOSITORY:TAG> bash

         # Example
         WORKSPACE=/local/mnt/workspace/Qworkspace/DEV && SRC_DIR=LE.QCLINUX.1.0.r1 && docker run --rm  -it -v ~/.qualcomm_launcher_workspace_config:/var/tmp/.docker_qualcomm_launcher_setup/ -v $WORKSPACE:$WORKSPACE -e LOCAL_USER_NAME=`id -u -n` -e LOCAL_USER_ID=`id -u` -e USER=`id -u` -e WORKSPACE=$WORKSPACE -w $WORKSPACE/$SRC_DIR 032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/le.um-k2c:20.04.20231215014450998.7 bash

Check if you are in a workspace that has ``.repo`` in it.

Set up the environment and generate eSDK:

.. note:: When the eSDK generation build command is complete, the images
          are generated in the following directory: ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/sdk``.

1. After building the ``meta-qcom-hwe`` with QSC CLI:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake -c do_populate_sdk_ext <image>
         # Example, bitbake -c do_populate_sdk_ext qcom-multimedia-image

   .. note:: To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.    

2. After building with ``meta-qcom-extras`` and firmware sources with
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

3. After building standalone instructions within the same shell (shell
   where the build is successful):

   .. container:: nohighlight
      
      ::

         bitbake -c do_populate_sdk_ext <image>

         # Example
         bitbake -c do_populate_sdk_ext qcom-multimedia-image

4. After building with standalone instructions and with a new shell
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

5. After building with standalone instructions using Dockerfile.

   a. Move the control to the workspace directory:

      .. container:: nohighlight
      
         ::

            cd /local/mnt/workspace/qcom-download-utils/<release>

            # Example
            cd /local/mnt/workspace/qcom-download-utils/qcom-6.6.65-QLI.1.4-Ver.1.0

   #. Set up the environment and issue an eSDK build:

      .. container:: nohighlight
      
         ::

            MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            bitbake -c do_populate_sdk_ext <image>

            # Example
            MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            bitbake -c do_populate_sdk_ext qcom-multimedia-image

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

Run the commands to connect to Docker for your environment setup and then use the BitBake commands to rebuild:

.. container:: nohighlight
      
   ::

      cd <workspace_path>/DEV/<softwareimage>
      # Example, cd /local/mnt/workspace/Qworkspace/DEV/LE.QCLINUX.1.0.r1 for making changes to Yocto layers
      # Make code changes

.. note:: 
   Get to a Docker shell as mentioned in :ref:`Generate an eSDK <how_to_build_generate_sdk>`.

-  Rebuild using your source changes:

   .. container:: nohighlight
      
      ::

         # Rebuild commands
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         bitbake qcom-multimedia-image

   .. note:: To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

-  Build image ``qcom-multimedia-test-image``:

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

         git clone --branch master https://github.com/linux-msm/qdl
         cd qdl
         git checkout cbd46184d33af597664e08aff2b9181ae2f87aa6
         make

2. Flash using the generated QDL:

   .. container:: nohighlight
      
      ::

         ./qdl --storage ufs --include <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image/prog_firehose_ddr.elf <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image/rawprogram*.xml <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image/patch*.xml

.. _change_hex_tool_install_path:

Change the Hexagon tool install path
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``HEXAGON_ROOT`` environment variable must point to the path where the Hexagon tools are installed. By default, the ``qpm-cli`` tool installs a ``HEXAGON_ROOT`` variable in the ``$HOME`` directory. You can also choose an alternate installation directory.

Use the ``––path`` option in ``qpm-cli`` command to install Hexagon tools in a directory of your choice and export the ``HEXAGON_ROOT`` variable to the same directory.

Provide an absolute path for ``<TOOLS_DIR>`` in ``qpm-cli`` and export commands as shown in the following example:

.. container:: nohighlight
      
   ::

      # Example
      
      mkdir -p <TOOLS_DIR>
      qpm-cli --extract hexagon8.4 --version 8.4.07 --path <TOOLS_DIR>/8.4.07
      export HEXAGON_ROOT=<TOOLS_DIR>

.. _image_recipes_github_workflow:

Image recipes supported in the GitHub workflow
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

+------------------------------------+------------------------------------------+
| Image recipe                       | Description                              |
+====================================+==========================================+
| ``qcom-minimal-image``             | A minimal rootfs image that              |
|                                    | boots to shell                           |
+------------------------------------+------------------------------------------+
| ``qcom-console-image``             | Boot to shell with package               |
|                                    | group to bring in all the basic          |
|                                    | packages                                 |
+------------------------------------+------------------------------------------+
| ``qcom-multimedia-image``          | Image recipe includes recipes            |
|                                    | for multimedia software                  |
|                                    | components, such as, audio,              |
|                                    | Bluetooth\ :sup:`®`, camera, computer    |
|                                    | vision, display, and video.              |
+------------------------------------+------------------------------------------+
| ``qcom-multimedia-test-image``     | Image recipe that includes               |
|                                    | tests                                    |
+------------------------------------+------------------------------------------+

.. _download_platform_esdk:

Download the Platform eSDK
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Check the :ref:`host computer requirements <host_machine_req_github_workflow_unregistered_users>`.

2. Set up the :ref:`Ubuntu host <ubuntu_host_setup_github_unreg>`.

3. Download the Platform eSDK:

   a. Create a directory:

      .. container:: nohighlight
      
         ::

            mkdir <workspace_path>

   #. Open the directory:

      .. container:: nohighlight
      
         ::

            cd <workspace_path>

   #. Download the zipped file:

      -  For an Ubuntu x86 architecture-based host computer:

         .. container:: nohighlight
      
            ::

               wget https://artifacts.codelinaro.org/artifactory/qli-ci/flashable-binaries/qimpsdk/qcs6490-rb3gen2-core-kit/x86/qcom-6.6.65-QLI.1.4-Ver.1.0_qim-product-sdk-1.1.1.zip

      -  For an Arm architecture-based host computer:

         .. container:: nohighlight
      
            ::

               wget https://artifacts.codelinaro.org/artifactory/qli-ci/flashable-binaries/qimpsdk/qcs6490-rb3gen2-core-kit/arm/qcom-6.6.65-QLI.1.4-Ver.1.0_qim-product-sdk-1.1.1.zip

   #. Unzip the QIMP SDK to a directory of your choice:

      .. container:: nohighlight
      
         ::

            unzip qcom-6.6.65-QLI.1.4-Ver.1.0_qim-product-sdk-1.1.1.zip

      After unzipping, ensure that the eSDK installer is located at
      ``<unzip_location>/target/qcm6490/sdk/``:
      
      |imageunzipESDK|

   #. If you do not have the necessary write permissions for the
      directory where you are trying to install the eSDK, the installer
      alerts you and then terminates. If this occurs, set up the
      permissions in the directory appropriately by using the following
      command and rerun the installer:

      .. container:: nohighlight
      
         ::

            umask a+rx

4. Run the installer script to install the eSDK. For example:

   .. container:: nohighlight
      
      ::

         sh ./qcom-wayland-x86_64-qcom-multimedia-image-armv8-2a-qcm6490-toolchain-ext-1.0.sh

5. Follow the instructions on the console to install the eSDK in a convenient location on your host computer.

6. Ensure that the eSDK installation is successful.

   After installation, the QIMP SDK layers are included under ``<workspace_path>/layers``:

   |imageLayerWorkspace|

   .. note:: Advanced developers can still build their own eSDK by following the steps mentioned in `Advanced procedures <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-51/advanced-procedure.html>`__.

7. Run the following command to set the ``ESDK_ROOT`` variable:

   .. container:: nohighlight
      
      ::

         export ESDK_ROOT=<pathofinstallationdirectory>

   For example:

   .. container:: nohighlight
      
      ::

         export ESDK_ROOT=/local/mnt/workspace/Platform_eSDK_plus_QIM

The QIMP SDK installation is now complete. To develop an application for
the device, see `Develop your first application <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-51/content-develop-your-first-application.html>`__.



.. |imageunzipESDK| image:: ../../media/k2c-qli-build-ga/unzip_location.png
.. |imageLayerWorkspace| image:: ../../media/k2c-qli-build-ga/qimp_sdk_layers.png