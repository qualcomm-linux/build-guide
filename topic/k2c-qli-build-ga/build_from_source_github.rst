.. _build_from_source_github:

.. _ubuntu_host_setup:

Set up the Ubuntu host computer
-------------------------------

Install and configure the required software tools on the Ubuntu host computer.

1. Install the following packages to prepare your host environment for the Yocto build:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install build-essential chrpath cpio debianutils diffstat file gawk gcc git iputils-ping libacl1 locales python3 python3-git python3-jinja2 python3-pexpect python3-pip python3-subunit socat texinfo unzip wget xz-utils zstd
         sudo apt install pipx
         pipx ensurepath
         pipx install kas

#. Optionally download the kas-container script. The kas package also provides a kas-container script for running kas in a container. If you prefer running the image builds in an isolated environment, consider using kas-container instead.

   .. container:: nohighlight
      
      ::

         # kas-container can be run on any linux distribution with docker installed.
         wget -qO kas-container https://raw.githubusercontent.com/siemens/kas/refs/tags/5.1/kas-container
         chmod +x kas-container

.. note::
  The `kas <https://kas.readthedocs.io/en/latest/>`__ tool is used by Qualcomm Linux to sync the meta layers, configure the environment and execute the bitbake commands.

Sync
-----

QLI uses the kas tool to sync and build the Yocto meta layers. Kas lock files recording the meta layer repository information are stored in `meta-qcom-releases <https://github.com/qualcomm-linux/meta-qcom-releases>`__ for each critical release. 

You can checkout the lock files for each release by the `meta-qcom-release-tag`. The meta-qcom release tag syntax is as follows:

``qcom-<Linux LTS Kernel Version>-QLI.<version>-Ver.<release>``

For example, the meta-qcom release tag ``qcom-6.18-QLI.2.0-Ver.1.0`` denotes the following:

- 6.18: Qualcomm Linux kernel
- QLI.2.0: Qualcomm Linux v2.0
- 1.0: Milestone release

Build a BSP image
-----------------

Create and build a Yocto Docker image:

1. Download Qualcomm Yocto and the supporting meta-layers. For the latest ``<meta-qcom-release-tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.
      
   .. container:: nohighlight

      ::

         git clone https://github.com/qualcomm-linux/meta-qcom-releases -b <meta-qcom-release-tag>
         kas checkout meta-qcom-releases/lock.yml

#. Copy the kas lock file from meta-qcom-releases to meta-qcom. Make sure to run this step, otherwise the checked out meta layers may get updated to a newer commit. 

   .. container:: nohighlight
      
      ::

         # kas configuration files need to be part of same repository
         # copy kas lock file to meta-qcom repository
         cp meta-qcom-releases/lock.yml meta-qcom/ci/lock.yml

#. Build the software image. Build targets are defined based on machine and distro combinations. 

   .. container:: nohighlight
      
      ::

         kas build meta-qcom/ci/<machine.yml>:meta-qcom/ci/<distro.yml>:meta-qcom/ci/lock.yml

         # eg. kas build meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/lock.yml

   For various ``<machine>`` and ``<distro>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. note::
      To build the images in a fully isolated environment, you can try out `kas-container <https://kas.readthedocs.io/en/latest/userguide/kas-container.html>`__ instead. 

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
-----

To flash the software images to the device, see :ref:`Flash software images <flash_images>`.
