
Introduction
========================================

This guide describes how to configure, download, compile, and flash Qualcomm\ :sup:`®` Linux\ :sup:`®` and the associated firmware on supported devices.

Qualcomm recommends that you read the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27>`__ before starting your build.

Users can download prebuilt flashable images or sync and build as described in the following sections.

Download images
----------------

- Download prebuilt flashable images (includes Platform eSDK) from the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/ReleaseNote.html#prebuilt-flashable-images-along-with-esdk>`__.

- The Platform eSDK is an installer generated from the Qualcomm Linux software. It provides a complete Yocto environment that allows you to sync, modify, compile, and install applications and open-source plug-ins. For more information, see :ref:`Download the Platform eSDK <section_imr_xc4_1cc_vinayjk_07-12-24-1513-38-780>`.

- To flash the prebuilt flashable images, see :doc:`Flash images <flash_images>`.

Sync and build
---------------

.. _section_rtp_vbg_qbc_vinayjk_06-05-24-1835-16-723:

Unregistered users
^^^^^^^^^^^^^^^^^^^

Unregistered users can sync and build Qualcomm Linux using the :doc:`GitHub workflow for unregistered users <github_workflow_unregistered_users>`. Qualcomm exclusively offers firmware components in the form of binary files to users who are not registered.

.. _section_x3d_nqy_v1c:

Registered users
^^^^^^^^^^^^^^^^^

Registered users can use any one of the following three methods to sync and build Qualcomm Linux. These methods use the Qualcomm Yocto layers and the supporting base Yocto layers. Registered users have the ability to access the source of certain firmware components and Qualcomm tools, which allows them to build and download the software.


.. list-table:: 
   :header-rows: 1
   :align: center
   :class: longtable
 
   * - Launcher
     - CLI
     - GitHub
   * - Easy-to-use, GUI-based, Qualcomm\ :sup:`®` Software Center (QSC) Launcher.
     - Simple QSC command-line interface (CLI).
     - Instructions to use GitHub based workflow.
   * - .. centered:: :doc:`Build with QSC Launcher<build_from_source_qsc_gui_intro>` 
     - .. centered:: :doc:`Build with QSC CLI <build_from_source_QSC_CLI>`
     - .. centered:: :doc:`GitHub workflow for registered users <build_from_source_github_intro>`

.. figure:: ../../media/k2c-qli-build-ga/explore_QSC.svg
   :align: center

.. note:: See `hardware SoCs <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-115/soc.html>`__ that are supported on Qualcomm Linux.