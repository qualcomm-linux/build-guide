.. _build_from_source_github:

Build with standalone commands
------------------------------------

.. _ubuntu_host_setup:

Set up the Ubuntu host computer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Install and configure the required software tools on the Ubuntu host computer.

1. Install the following packages to prepare your host environment for the Yocto build:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install repo gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool locales tar python-is-python3 file libxml-opml-simplegen-perl vim whiptail g++ libacl1

#. Add your Qualcomm Log in ID with Personalized Access Token (PAT) to the ``~/.netrc`` file in your home directory:

   .. container:: nohighlight
      
      ::

         # Log in to qsc-cli to generate PAT
         qsc-cli login -u <username>
         # Run the following command to generate PAT
         qsc-cli show-access-token
         # This command gives output as shown in the following note
         # The last line in this output is the token, which can be used to access
         # Qualcomm Proprietary repositories. This token expires in two weeks.


   .. note::
        
      .. container:: screenoutput

         | user\@hostname:/local/mnt/workspace$ qsc-cli show-access-token
         | [Info]: Starting qsc-cli version 0.0.0.9
         | **5LThNlklb55mMVLB5C2KqUGU2jCF**

#. Use your preferred text editor to edit the ``~/.netrc`` file and add the following entries. Create the ``~/.netrc`` file if it doesn't exist.

   .. container:: nohighlight
      
      ::

        machine chipmaster2.qti.qualcomm.com
        login <your Qualcomm login id>
        password <your PAT token>

        machine qpm-git.qualcomm.com
        login <your Qualcomm login id>
        password <your PAT token>

#. Set up the locales (if not set up already):

   .. container:: nohighlight
      
      ::

         sudo locale-gen en_US.UTF-8
         sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
         export LC_ALL=en_US.UTF-8
         export LANG=en_US.UTF-8

#. Update git configurations:

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

         # Add the following git configurations to follow remote redirects from http-alternates files or alternates
         git config --global http.https://chipmaster2.qti.qualcomm.com.followRedirects true
         git config --global http.https://qpm-git.qualcomm.com.followRedirects true

Sync
^^^^^^^

Use the Repo tool that was installed during the :ref:`Ubuntu host setup <ubuntu_host_setup>` to download a list of git repositories and additional attributes from the `Qualcomm manifest <https://github.com/quic-yocto/qcom-manifest>`__. The Repo tool downloads the manifests using the ``repo init`` command.

The following table shows an example mapping of the Yocto layers to the manifest release tags. Use this mapping to download and build Qualcomm Linux:

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
         
         ``qcom-6.6.116-QLI.1.7-Ver.1.1.xml``
       - ``qcom-wayland``
     * - 
         - ``meta-qcom``
         - ``meta-qcom-hwe``
         - ``meta-qcom-distro``
         - ``meta-qcom-qim-product-sdk``
       - BSP build + Qualcomm IM SDK build:
         
         ``qcom-6.6.116-QLI.1.7-Ver.1.1_qim-product-sdk-2.2.1.xml``
       - ``qcom-wayland``
     * - 
         - ``meta-qcom``
         - ``meta-qcom-hwe``
         - ``meta-qcom-distro``
         - ``meta-qcom-realtime``
       - BSP build + Real-time kernel build:
         
         ``qcom-6.6.116-QLI.1.7-Ver.1.1_realtime-linux-1.1.xml``
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
         
         ``qcom-6.6.116-QLI.1.7-Ver.1.1_robotics-sdk-1.1.xml``
       - ``qcom-robotics-ros2-humble``

For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.
   
For information on building the ``meta-qcom-extras`` add-on layer and select firmware sources, see :doc:`Build with GitHub using firmware and extras <build_addn_info>`.

Build a BSP image
^^^^^^^^^^^^^^^^^^^^^
The BSP image build has software components for the Qualcomm device support and software features applicable to the Qualcomm SoCs. This build includes a reference distribution configuration for the Qualcomm development kits. For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.

1. Download Qualcomm Yocto and the supporting layers. For the latest ``<manifest release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.116-QLI.1.7-Ver.1.1.xml
         repo sync

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. Build the software image. For supported image recipes, see :ref:`Image recipes supported in the GitHub workflow <image_recipes_github_workflow>`.

   .. container:: nohighlight
      
      ::

         bitbake <image recipe>
         # Example, bitbake qcom-multimedia-image

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         # meta-qcom uses qcomflash IMAGE_FSTYPE to create a single tarball
         # containing all the relevant files to perform a full clean flash,
         # including partition metadata, boot firmware, ESP # partition and
         # the rootfs.
         cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs-<DATE>.qcomflash/
         ls -al rootfs.img

.. note::
   For repo manifest based builds, refer to :ref:`Alternate Build Instructions via Manifest <howto_build>`

Flash
^^^^^^^

To flash the software images to the device, see :ref:`Flash software images <flash_images>`.
