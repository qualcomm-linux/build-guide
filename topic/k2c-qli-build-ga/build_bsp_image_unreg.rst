Build a BSP image
-------------------
The board support package (BSP) image build has software components for the Qualcomm device support and software features applicable to Qualcomm SoCs. This build includes a reference distribution configuration for the Qualcomm development kits. For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.

1. Download Qualcomm Yocto and the supporting layers. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

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
