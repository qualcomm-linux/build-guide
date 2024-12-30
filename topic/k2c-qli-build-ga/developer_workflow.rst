.. _developer_workflow:

Developer workflow
--------------------------

.. _section_ycs_nrf_s1c_vinayjk_03-11-24-2150-29-324:

Sync and build with real-time Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``PREEMPT_RT`` patches for the Qualcomm Linux kernel are provided in the
``meta-qcom-realtime`` layer. This layer is available in the Qualcomm
`GitHub <https://github.com/quic-yocto/meta-qcom-realtime>`__ and it is
built on top of the BSP build image. For more information on this layer,
see
`meta-qcom-realtime <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-27/platform_software_features.html#meta-qcom-realtime>`__
from the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-27>`__.

To sync and build real-time Linux, see :ref:`Build real-time Linux image <section_k51_23b_wbc_vinayjk_06-26-24-1344-54-418>`.

.. _section_qct_b5g_s1c_vinayjk_03-12-24-127-51-384:

Migrate from the previous release to the next release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The migration process depends on the development, branching, and integration workflows at your end. However, the following steps must be performed:

1. Integrate changes for the sources that you have branched:

   a. Skip this step if you have not forked any underlying source code
      used by Qualcomm Yocto layers and are applying only ``.patch``
      files.
   #. Qualcomm provides git history to all the source repositories.
      A reference list of repositories is provided in the `Release
      Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.
      For Qualcomm repositories that are branched and modified at your
      end, perform the following steps:

      i. Compare the Qualcomm Yocto layers with your Yocto layers, and
         identify the repositories you must migrate.
      #. Check the Qualcomm Yocto layer recipes and identify the SRC_URI
         updates.
      #. Clone these repositories using ``git clone`` and check out the
         appropriate branch and SHA as pointed by the SRC_URI.
      #. Use ``git merge`` or appropriate mechanism to bring the delta
         to your own fork according to your need.
      #. Update your recipes as required.

2. Integrate the Yocto layer recipes – ``git merge`` the Yocto layer to
   pick up any changes.
3. Build and validate your resulting workspace.
4. Proceed with the next steps in your internal workflows.

.. _section_rvz_b5g_s1c_vinayjk_03-12-24-127-55-935:

Build a Qualcomm Linux kernel
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `building kernel image <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-3/yocto-kernel-support.html#build-kernel-image>`__.

.. _section_lb1_c5g_s1c_vinayjk_03-12-24-127-56-85:

Customize Qualcomm Yocto layers
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `user
customizations <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-27/user_customizations.html>`__.

.. _section_l2s_5qj_ybc_vinayjk_07-04-24-2052-5-755:

Download layers for the QIMP SDK build using the manifest release tag
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: For the latest ``<manifest release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

::

   # cd to directory where you have 300 GB of free storage space to create your workspaces
   mkdir <WORKSPACE_DIR>
   cd <WORKSPACE_DIR>
   repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
   # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2.xml
   repo sync

.. note:: 
   For the steps to set up environment and build software images, see :ref:`Build QIMP SDK image <section_lrb_1nd_fbc>`.

.. _section_bgr_hfk_ybc_vinayjk_07-04-24-2242-31-273:

Download layers for the QIRP SDK build by using the manifest release tag
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: For the latest ``<manifest release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

::

   # cd to directory where you have 300 GB of free storage space to create your workspaces
   mkdir <WORKSPACE_DIR>
   cd <WORKSPACE_DIR>
   repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
   # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1_robotics-product-sdk-1.1.xml
   repo sync

.. note:: 
   For the steps to set up the environment and build software images, see :ref:`Build QIRP SDK image <section_gv3_czl_qbc_vinayjk_06-06-24-1402-32-392>`.

.. _section_jpw_mfk_ybc_vinayjk_07-04-24-2244-2-775:

Download layers for the real-time Linux build by using the manifest release tag
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. note:: For the latest ``<manifest release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

::

   # cd to directory where you have 300 GB of free storage space to create your workspaces
   mkdir <WORKSPACE_DIR>
   cd <WORKSPACE_DIR>
   repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
   # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1_realtime-linux-1.0.xml
   repo sync

.. note:: 
   For the steps to set up the environment and build software images, see :ref:`Build real-time Linux image <section_k51_23b_wbc_vinayjk_06-26-24-1344-54-418>`.

.. _section_xxm_fk3_v1c_vinayjk_03-23-24-014-37-829:

Build a meta-qcom-qim-product-sdk layer as an add-on layer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you have completed a base image build using GitHub standalone
commands and already have an existing ``<WORKSPACE DIR>``, follow these steps to build ``meta-qcom-qim-product-sdk``:

1. Download the latest ``<meta-qcom-qim-product-sdk release tag>``:

   ::

      cd <WORKSPACE DIR>
      rm -rf layers/meta-qcom-qim-product-sdk
      git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk

   .. note:: For more information on 
             ``<meta-qcom-qim-product-sdk release tag>``, see https://github.com/quic-yocto/meta-qcom-qim-product-sdk/tags. An
             example ``<meta-qcom-qim-product-sdk release tag>`` is ``qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2.xml``.

2. Set up the build environment:

   ::

      MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
      # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory

   .. note:: To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

3. Build the software image:

   ::

      bitbake qcom-multimedia-image
      # Build SDK image
      bitbake qcom-qim-product-sdk