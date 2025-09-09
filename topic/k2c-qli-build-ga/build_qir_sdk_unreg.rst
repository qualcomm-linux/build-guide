.. _build_qirp_sdk_image_unreg:

Build QIR SDK image
---------------------
The Qualcomm® Intelligent Robotics (QIR) SDK 2.0 is a collection of components that let you develop robotic features on Qualcomm platforms. This SDK is applicable to the Qualcomm Linux releases. For more details, see `QIR SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-265>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for QIR SDK build is the same as the BSP build. Clone the QIR SDK layers on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.97-QLI.1.6-Ver.1.0.xml
         repo sync

#. Download the QIR SDK layers into the BSP build ``<WORKSPACE DIR>`` directory:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/ros/meta-ros -b scarthgap layers/meta-ros && cd layers/meta-ros && git checkout c560699e810e60a9526f4226c2c23f8d877280c8 && cd ../../
         git clone https://github.com/quic-yocto/meta-qcom-robotics.git -b qcom-6.6.97-QLI.1.6-Ver.1.0_robotics-sdk-1.0 layers/meta-qcom-robotics
         git clone https://github.com/quic-yocto/meta-qcom-robotics-distro.git -b qcom-6.6.97-QLI.1.6-Ver.1.0_robotics-sdk-1.0 layers/meta-qcom-robotics-distro
         git clone https://github.com/quic-yocto/meta-qcom-robotics-sdk.git -b qcom-6.6.97-QLI.1.6-Ver.1.0_robotics-sdk-1.0 layers/meta-qcom-robotics-sdk
         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b qcom-6.6.97-QLI.1.6-Ver.1.0_qim-product-sdk-2.1.0 layers/meta-qcom-qim-product-sdk

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         ln -s layers/meta-qcom-robotics-distro/set_bb_env.sh ./setup-robotics-environment
         ln -s layers/meta-qcom-robotics-sdk/scripts/qirp-build ./qirp-build
         MACHINE=<machine> DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=<override> source setup-robotics-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=custom source setup-robotics-environment
         # source setup-robotics-environment: Sets the environment, creates the build directory build-qcom-robotics-ros2-humble,
         # and enters into build-qcom-robotics-ros2-humble directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

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