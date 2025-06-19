.. _build_with_standalone_commands_unreg:

Build with standalone commands
-----------------------------------

.. _ubuntu_host_setup_github_unreg:

Set up the Ubuntu host computer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install and configure the required software tools on the Ubuntu host computer.

1. Install the following packages to prepare your host environment for the Yocto build:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install repo gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool locales tar python-is-python3 file libxml-opml-simplegen-perl vim whiptail g++ libacl1

2. Set up the locales (if not set up already):

   .. container:: nohighlight
      
      ::

         sudo locale-gen en_US.UTF-8
         sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
         export LC_ALL=en_US.UTF-8
         export LANG=en_US.UTF-8

3. Update git configurations:

   .. container:: nohighlight
      
      ::

         # Check if your identity is configured in .gitconfig
         git config --get user.email
         git config --get user.name

         # Run the following commands if you don't have your account identity set in .gitconfig
         git config --global user.email <Your email ID>
         git config --global user.name <"Your Name">

         # Add the following UI color option for output of console (optional)
         git config --global color.ui auto

         # Add the following git configurations to fetch large size repositories and to avoid unreliable connections
         git config --global http.postBuffer 1048576000
         git config --global http.maxRequestBuffer 1048576000
         git config --global http.lowSpeedLimit 0
         git config --global http.lowSpeedTime 999999

Sync
^^^^^^^

Use the Repo tool that was installed during the :ref:`Ubuntu host setup <ubuntu_host_setup_github_unreg>` to download git repositories and other attributes from the `Qualcomm manifest <https://github.com/quic-yocto/qcom-manifest>`__. The Repo tool downloads the manifests using the ``repo init`` command.

The following table shows an example mapping of the Yocto layers to the manifest release tags. Use this mapping to download and build Qualcomm Linux.

.. list-table:: Mapping Yocto layers to manifest release tags
   :header-rows: 1
   :class: longtable


   * - Yocto layers
     - Manifest release tag
     - Reference distribution (``DISTRO``)

   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
     - BSP build: High-level OS and prebuilt firmware (GPS only)
       
       ``qcom-6.6.90-QLI.1.5-Ver.1.0.xml``
     - ``qcom-wayland``

   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-qcom-qim-product-sdk``
     - BSP build + Qualcomm IM SDK build:
       
       ``qcom-6.6.90-QLI.1.5-Ver.1.0_qim-product-sdk-2.0.0.xml``
     - ``qcom-wayland``
   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-qcom-realtime``
     - BSP build + Real-time kernel build:
       
       ``qcom-6.6.90-QLI.1.5-Ver.1.0_realtime-linux-1.1.xml``
     - ``qcom-wayland``
   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-ros``
        - ``meta-qcom-robotics``
        - ``meta-qcom-robotics-distro``
        - ``meta-qcom-robotics-sdk``
        - ``meta-qcom-qim-product-sdk``
     - BSP build + QIR SDK build:
       
       ``qcom-6.6.90-QLI.1.5-Ver.1.0_robotics-product-sdk-1.0.xml``
     - ``qcom-robotics-ros2-humble``

.. note::
   - Syntax for the manifest release tag:
     
     ``qcom-<Linux LTS Kernel Version>-QLI.<version>-Ver.<release>.xml``
    
     For example, the manifest release tag ``qcom-6.6.90-QLI.1.5-Ver.1.0.xml`` denotes the following:
     
     - 6.6.90: Qualcomm Linux kernel
     - QLI.1.5: Qualcomm Linux v1.5
     - 1.0: Milestone release

   - Syntax for the additional productization manifest release tag:
   
     ``qcom-<Linux LTS Kernel version>-QLI.<version>-Ver.<milestone release>_<product/customization>-<patch release>.xml``

     For example, the additional productization manifest release tag ``qcom-6.6.90-QLI.1.5-Ver.1.0_qim-product-sdk-2.0.0.xml`` denotes the following:
     
     - 6.6.90: Qualcomm Linux kernel
     - QLI.1.5: Qualcomm Linux v1.5
     - qim-product-sdk-2.0.0: Qualcomm IM SDK release on top of QLI.1.5

       Other product/customization examples:

       - *realtime-linux-1.0*
       - *robotics-product-sdk-1.1*
     - 1.0: Milestone release
     - 2.0.0: Patch release associated with the milestone release

   - For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27/qualcomm_linux_metadata_layers_overview.html#qualcomm-linux-metadata-layers>`__.

Build a BSP image
^^^^^^^^^^^^^^^^^^
The board support package (BSP) image build has software components for the Qualcomm device support and software features applicable to Qualcomm SoCs. This build includes a reference distribution configuration for the Qualcomm development kits. For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27/qualcomm_linux_metadata_layers_overview.html#qualcomm-linux-metadata-layers>`__.

1. Download Qualcomm Yocto and the supporting layers. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0.xml
         repo sync

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

#. Build the software image. For supported image recipes, see :ref:`Image recipes supported in the GitHub workflow <image_recipes_github_workflow>`.

   .. container:: nohighlight
      
      ::

         bitbake <image recipe>
         # Example, bitbake qcom-multimedia-image

#. After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

.. _build_qimp_sdk_image_unreg:

Build Qualcomm IM SDK image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The Qualcomm® Intelligent Multimedia SDK (IM SDK) is a collection of four standalone function SDKs: Qualcomm IM SDK, Qualcomm® Neural Processing SDK, Qualcomm® AI Engine direct SDK, and Lite Runtime (LiteRT). This SDK also includes reference applications that you can use to develop use cases. For more details, see `Qualcomm IM SDK Quick Start Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-51>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for the Qualcomm IM SDK build is the same as the BSP build. Clone the Qualcomm IM SDK layer on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0.xml
         repo sync

#. Clone the Qualcomm IM SDK layer into the workspace:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
         # Example, <meta-qcom-qim-product-sdk release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0_qim-product-sdk-2.0.0

#. Export the Qualcomm IM SDK layer:

   .. container:: nohighlight
      
      ::

         export EXTRALAYERS="meta-qcom-qim-product-sdk"

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image
         # Build SDK image
         bitbake qcom-qim-product-sdk

#. After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

.. _build_qirp_sdk_image_unreg:

Build QIR SDK image
^^^^^^^^^^^^^^^^^^^^^
The Qualcomm® Intelligent Robotics (QIR) SDK 2.0 is a collection of components that let you develop robotic features on Qualcomm platforms. This SDK is applicable to the Qualcomm Linux releases. For more details, see `QIR SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-265>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for QIR SDK build is the same as the BSP build. Clone the QIR SDK layers on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0.xml
         repo sync

#. Download the QIR SDK layers into the BSP build ``<WORKSPACE DIR>`` directory:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/ros/meta-ros -b scarthgap layers/meta-ros && cd layers/meta-ros && git checkout c560699e810e60a9526f4226c2c23f8d877280c8 && cd ../../
         git clone https://github.com/quic-yocto/meta-qcom-robotics.git -b qcom-6.6.90-QLI.1.5-Ver.1.0_robotics-product-sdk-1.0 layers/meta-qcom-robotics
         git clone https://github.com/quic-yocto/meta-qcom-robotics-distro.git -b qcom-6.6.90-QLI.1.5-Ver.1.0_robotics-product-sdk-1.0 layers/meta-qcom-robotics-distro
         git clone https://github.com/quic-yocto/meta-qcom-robotics-sdk.git -b qcom-6.6.90-QLI.1.5-Ver.1.0_robotics-product-sdk-1.0 layers/meta-qcom-robotics-sdk
         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b qcom-6.6.90-QLI.1.5-Ver.1.0_qim-product-sdk-2.0.0 layers/meta-qcom-qim-product-sdk

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         ln -s layers/meta-qcom-robotics-distro/set_bb_env.sh ./setup-robotics-environment
         ln -s layers/meta-qcom-robotics-sdk/scripts/qirp-build ./qirp-build
         MACHINE=<machine> DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=<override> source setup-robotics-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=custom source setup-robotics-environment
         # source setup-robotics-environment: Sets the environment, creates the build directory build-qcom-robotics-ros2-humble,
         # and enters into build-qcom-robotics-ros2-humble directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

#. Build the robotics image and the QIR SDK artifacts:

   .. container:: nohighlight
      
      ::

         ../qirp-build qcom-robotics-full-image

#. After a successful build, check that the QIR SDK build artifacts are at the following paths:

   .. container:: nohighlight
      
      ::

         QIR SDK artifacts: <WORKSPACE DIR>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/qirpsdk_artifacts/qirp-sdk_<version>.tar.gz
         # system.img is present in the following path
         Robotics image: <WORKSPACE DIR>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-robotics-full-image

.. _build_real_time_linux_image_unreg:

Build real-time Linux image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The real-time layer provides recipes and configurations required to run the Qualcomm Linux kernel as a real-time kernel. The real-time kernel runs with preemption fully enabled through a configuration, ``CONFIG_PREEMPT_RT=y``. This layer supports ``linux-kernel-qcom-rt`` recipe that fetches and builds the Qualcomm Linux kernel for the supported machine. This layer appends to the kernel and the upstream ``PREEMPT_RT`` patches based on the kernel version, and enables real-time configurations. For more details, see `Real-time kernel <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-3/real_time_kernel_overview.html>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for real-time Linux image is the same as the BSP build. Clone the real-time Linux on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0.xml
         repo sync

#. Clone the real-time Linux layer into the workspace:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/quic-yocto/meta-qcom-realtime -b <meta-qcom-realtime release tag> layers/meta-qcom-realtime
         # Example, <meta-qcom-realtime release tag> is qcom-6.6.90-QLI.1.5-Ver.1.0_realtime-linux-1.1

#. Build the real-time layer:

   .. container:: nohighlight
      
      ::

         export EXTRALAYERS="meta-qcom-realtime"

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image

#. After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

Flash
^^^^^^^

To flash the software images to the device, see :ref:`Flash software images <flash_images>`.

Related topics
---------------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`