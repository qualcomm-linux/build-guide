.. _build_from_source_github:

Build with standalone commands
------------------------------------

.. _section_vh2_rqf_v1c_vinayjk_03-22-24-1509-40-643:

Ubuntu host setup
^^^^^^^^^^^^^^^^^^^^^

The Ubuntu host machine needs a few setup operations to ensure that the required software tools are ready.

1. Install the following packages to prepare your host environment for
   Yocto build:

   ::

      sudo apt update
      sudo apt install repo gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool locales tar python-is-python3 file libxml-opml-simplegen-perl vim whiptail

#. Add your Qualcomm login ID with Personalized Access Token (PAT) to the ``~/.netrc`` file in your home directory:

   ::

      # Log in to qsc-cli to generate PAT
      qsc-cli login -u <username>
      # Run the following command to generate PAT
      qsc-cli pat --get
      # This command gives output as shown in the following note
      # The last line in this output is the token, which can be used to access
      # Qualcomm Proprietary repositories. This token expires in two weeks.


   .. note::
        | user@hostname:/local/mnt/workspace$ qsc-cli pat --get
        | [Info]: Starting qsc-cli version 0.0.0.9
        | **5LThNlklb55mMVLB5C2KqUGU2jCF**

#. Use your preferred text editor to edit the ``~/.netrc`` file and add the following entries:

   .. note:: Create the ``~/.netrc`` file if it does not exist.

   ::

        machine chipmaster2.qti.qualcomm.com
        login <your Qualcomm login id>
        password <your PAT token>

        machine qpm-git.qualcomm.com
        login <your Qualcomm login id>
        password <your PAT token>

#. Set up the locales (if not set up already):

   ::

      sudo locale-gen en_US.UTF-8
      sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
      export LC_ALL=en_US.UTF-8
      export LANG=en_US.UTF-8

#. Update git configurations:

   ::

      # Check if your identity is configured in .gitconfig
      git config --get user.email
      git config --get user.name

      # Run the following commands if you do not have your account identity set in .gitconfig
      git config --global user.email <Your email ID>
      git config --global user.name <"Your Name">

      # Add the following UI color option for output of console (optional)
      git config --global color.ui auto

      # Add the following git configurations to fetch large size repositories and to avoid unreliable connections
      git config --global http.postBuffer 1048576000
      git config --global http.maxRequestBuffer 1048576000
      git config --global http.lowSpeedLimit 0
      git config --global http.lowSpeedTime 999999

      # Add the following git configurations to follow remote redirects from http-alternates files or alternates
      git config --global http.https://chipmaster2.qti.qualcomm.com.followRedirects true
      git config --global http.https://qpm-git.qualcomm.com.followRedirects true

.. _section_y32_1zy_v1c:

Sync
^^^^^^^

This section uses the Repo tool installed in :ref:`Ubuntu host setup <section_vh2_rqf_v1c_vinayjk_03-22-24-1509-40-643>` to download a list of git repositories and additional attributes from the `Qualcomm manifest <https://github.com/quic-yocto/qcom-manifest>`__. The Repo tool downloads the manifests using the ``repo init`` command.

The following table shows an example mapping of the Yocto layers to the manifest release tags, and this mapping is used to download and build Qualcomm Linux:

  .. list-table::
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
         
         ``qcom-6.6.38-QLI.1.2-Ver.1.0.xml``
       - ``qcom-wayland``
     * - 
         - ``meta-qcom``
         - ``meta-qcom-hwe``
         - ``meta-qcom-distro``
         - ``meta-qcom-qim-product-sdk``
       - BSP build + QIMP SDK build:
         
         ``qcom-6.6.38-QLI.1.2-Ver.1.0_qim-product-sdk-1.1.1.xml``
       - ``qcom-wayland``
     * - 
         - ``meta-qcom``
         - ``meta-qcom-hwe``
         - ``meta-qcom-distro``
         - ``meta-qcom-realtime``
       - BSP build + Real-time kernel build:
         
         ``qcom-6.6.38-QLI.1.2-Ver.1.0_realtime-linux-1.0.xml``
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
       - BSP build + QIRP SDK build:
         
         ``qcom-6.6.38-QLI.1.2-Ver.1.0_robotics-product-sdk-1.0.xml``
       - ``qcom-robotics-ros2-humble``

.. note::
   
   - For more information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#id7>`__.
   
   - For more information on building the ``meta-qcom-extras`` add-on layer and select firmware sources, see :doc:`GitHub workflow (firmware and extras) <build_addn_info>`.

.. _section_sk2_xk2_fbc:

Build BSP image
^^^^^^^^^^^^^^^^^^^^^

1. Download Qualcomm Yocto and supporting layers:

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0.xml
      repo sync

   .. note:: For the latest ``<manifest release tag>``, see the
             *Build-critical release tags* section in the `Release
             Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Set up the build environment:

   ::

      MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
      # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory.

   .. note::
      For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Build the software image:

   .. note:: 
      For supported image recipes, see :ref:`What are the image recipes supported in the GitHub workflow? <section_x3c_n5l_zbc_vinayjk_07-08-24-1744-58-455>`.

   ::

      bitbake <image recipe>
      # Example, bitbake qcom-multimedia-image

   After a successful build, you can verify if ``system.img`` is present in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      ls -al system.img

.. _section_lrb_1nd_fbc:

Build QIMP SDK image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Download Qualcomm Yocto and supporting layers:

   .. note:: The ``<manifest release tag>`` for QIMP SDK build is the
             same as the BSP build. Clone the QIMP SDK layer on top of the BSP build.

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0.xml
      repo sync

   .. note:: For the latest ``<manifest release tag>``, see the
             *Build-critical release tags* section in the `Release
             Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Clone the QIMP SDK layer into the workspace:

   ::

      git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
      # Example, <meta-qcom-qim-product-sdk release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0_qim-product-sdk-1.1.1

   To build a QIMP SDK layer, the following export is required:

   ::

      export EXTRALAYERS="meta-qcom-qim-product-sdk"

#. Set up the build environment:

   ::

      MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory.

   .. note::
      To know the ``<machine>`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Build the software image:

   ::

      bitbake qcom-multimedia-image
      # Build SDK image
      bitbake qcom-qim-product-sdk

   After a successful build, you can verify if ``system.img`` is present in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      ls -al system.img

.. _section_gv3_czl_qbc_vinayjk_06-06-24-1402-32-392:

Build QIRP SDK image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Download Qualcomm Yocto and supporting layers:

   .. note:: The ``<manifest release tag>`` for QIRP SDK build is the
             same as the BSP build. Clone the QIRP SDK layer  on top of the BSP build.

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0.xml
      repo sync

   .. note::  For the latest ``<manifest release tag>``, see the
              *Build-critical release tags* section in the `Release
              Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Download the QIRP SDK layers into the BSP build ``<WORKSPACE DIR>``
   directory:

   ::

      git clone https://github.com/ros/meta-ros -b kirkstone layers/meta-ros
      git clone https://github.com/quic-yocto/meta-qcom-robotics.git layers/meta-qcom-robotics
      git clone https://github.com/quic-yocto/meta-qcom-robotics-distro.git layers/meta-qcom-robotics-distro
      git clone https://github.com/quic-yocto/meta-qcom-robotics-sdk.git layers/meta-qcom-robotics-sdk
      git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk layers/meta-qcom-qim-product-sdk

#. Set up the build environment:

   ::

      ln -s layers/meta-qcom-robotics-distro/set_bb_env.sh ./setup-robotics-environment
      ln -s layers/meta-qcom-robotics-sdk/scripts/qirp-build ./qirp-build
      MACHINE=<machine> DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=<override> source setup-robotics-environment
      # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=custom source setup-robotics-environment
      # source setup-robotics-environment: Sets the environment, creates the build directory build-qcom-robotics-ros2-humble,
      # and enters into build-qcom-robotics-ros2-humble directory.

   .. note::
      For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Build the robotics image and QIRP SDK artifacts:

   ::

      ../qirp-build qcom-robotics-full-image

   After a successful build, you can see the QIRP SDK build artifacts at the following paths:

   ::

      QIRP SDK artifacts: <WORKSPACE DIR>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/qirpsdk_artifacts/qirp-sdk_<version>.tar.gz
      # system.img is present in the following path
      Robotics image: <WORKSPACE DIR>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-robotics-full-image

.. _section_k51_23b_wbc_vinayjk_06-26-24-1344-54-418:

Build real-time Linux image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Download Qualcomm Yocto and supporting layers:

   .. note:: The ``<manifest release tag>`` for real-time Linux image is the same as the BSP build. Clone the Real-time Linux on top of the BSP build.

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0.xml
      repo sync

   .. note::  For the latest ``<manifest release tag>``, see the
              *Build-critical release tags* section in the `Release
              Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Clone the real-time Linux layer into the workspace:

   ::

      git clone https://github.com/quic-yocto/meta-qcom-realtime -b <meta-qcom-realtime release tag> layers/meta-qcom-realtime
      # Example, <meta-qcom-realtime release tag> is qcom-6.6.38-QLI.1.2-Ver.1.0_realtime-linux-1.0

   To build a real-time layer, the following export is required:

   ::

      export EXTRALAYERS="meta-qcom-realtime"

#. Set up the build environment:

   ::

      MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
      # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory.

   .. note::
      For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

#. Build the software image:

   ::

      bitbake qcom-multimedia-image

   After a successful build, you can verify if ``system.img`` is present in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
      ls -al system.img

.. _section_x2k_vnf_w1c:

Flash
^^^^^^^

To flash the software images to the device, see :doc:`Flash images <flash_images>`.