.. _build_qim_sdk_image_unreg:

Build Qualcomm IM SDK image
-----------------------------
The Qualcomm® Intelligent Multimedia SDK (IM SDK) is a collection of four standalone function SDKs: Qualcomm IM SDK, Qualcomm® Neural Processing SDK, Qualcomm® AI Engine direct SDK, and Lite Runtime (LiteRT). This SDK also includes reference applications that you can use to develop use cases. For more details, see `Qualcomm IM SDK quickstart <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-51>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for the Qualcomm IM SDK build is the same as the BSP build. Clone the Qualcomm IM SDK layer on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.97-QLI.1.6-Ver.1.0.xml
         repo sync

#. Clone the Qualcomm IM SDK layer into the workspace:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
         # Example, <meta-qcom-qim-product-sdk release tag> is qcom-6.6.97-QLI.1.6-Ver.1.0_qim-product-sdk-2.1.0

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

   To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

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