Build a BSP image
-------------------
The board support package (BSP) image build has software components for the Qualcomm device support and software features applicable to Qualcomm SoCs. This build includes a reference distribution configuration for the Qualcomm development kits. For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.

1. Download Qualcomm Yocto and the supporting meta-layers. For the latest ``<meta-qcom-release-tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>
         git clone https://github.com/qualcomm-linux/meta-qcom-releases -b <meta-qcom-release-tag>
         kas checkout meta-qcom-releases/lock.yml

#. Build the software image. Build targets are defined based on machine and distro combinations. 

   .. container:: nohighlight
      
      ::

         kas build --skip repos_checkout meta-qcom/ci/<machine>:meta-qcom/ci/<distro>

   For various ``<machine>`` and ``<distro>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         cd <workspace-dir>/build/tmp/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image-rb3gen2-core-kit.rootfs.qcomflash/
         ls -al rootfs.img
