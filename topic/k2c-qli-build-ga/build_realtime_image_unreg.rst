.. _build_real_time_linux_image_unreg:

Build real-time Linux image
-----------------------------
The real-time layer provides recipes and configurations required to run the Qualcomm Linux kernel as a real-time kernel. The real-time kernel runs with preemption fully enabled through a configuration, ``CONFIG_PREEMPT_RT=y``. This layer supports ``linux-kernel-qcom-rt`` recipe that fetches and builds the Qualcomm Linux kernel for the supported machine. This layer appends to the kernel and the upstream ``PREEMPT_RT`` patches based on the kernel version, and enables real-time configurations. For more details, see `Real-time kernel <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-3/real_time_kernel_overview.html>`__.

1. Download Qualcomm Yocto and the supporting layers. The ``<manifest release tag>`` for real-time Linux image is the same as the BSP build. Clone the real-time Linux on top of the BSP build. For the latest ``<manifest release tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.97-QLI.1.6-Ver.1.0.xml
         repo sync

#. Clone the real-time Linux layer into the workspace:

   .. container:: nohighlight
      
      ::

         git clone https://github.com/quic-yocto/meta-qcom-realtime -b <meta-qcom-realtime release tag> layers/meta-qcom-realtime
         # Example, <meta-qcom-realtime release tag> is qcom-6.6.97-QLI.1.6-Ver.1.0_realtime-linux-1.0

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

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250630224842/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image

#. After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img