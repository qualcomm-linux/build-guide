
Introduction
========================================

This guide describes the methods to configure, download, compile, and
flash Qualcomm\ :sup:`®` Linux\ :sup:`®` and the associated firmware on supported devices.

..
  **This information is also available in** `Simplified Chinese <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-254Y>`__.

Qualcomm recommends that you read the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-27>`__
before starting your build.

.. _section_rtp_vbg_qbc_vinayjk_06-05-24-1835-16-723:

Unregistered users
------------------

Unregistered users can sync and build Qualcomm Linux using the :doc:`GitHub workflow for unregistered users <github_workflow_unregistered_users>`. For unregistered users, Qualcomm delivers firmware components only as binary.

.. _section_x3d_nqy_v1c:

Registered users
----------------

Registered users can use any one of the following three methods to sync and build Qualcomm Linux. These methods use the Qualcomm Yocto layers and the supporting base Yocto layers. Registered users have source access to some of the firmware components and they can also avail some of the Qualcomm tools (software building and downloading tools).


.. list-table:: 
   :header-rows: 1
   :align: center
   :class: longtable
 
   * - Launcher
     - CLI
     - GitHub
   * - Easy-to-use, GUI-based, Qualcomm Software Center (QSC) Launcher.
     - Simple QSC command-line interface (CLI).
     - Instructions to use GitHub based workflow.
   * - .. centered:: :doc:`Build with QSC Launcher<build_from_source_qsc_gui_intro>` 
     - .. centered:: :doc:`Build with QSC CLI <build_from_source_QSC_CLI>`
     - .. centered:: :doc:`GitHub workflow for registered users <build_from_source_github_intro>`

.. only:: html

      .. figure:: ../../media/k2c-qli-build-ga/explore_QSC.svg
         :align: center


.. note::

   - Prebuilt binaries along with Platform eSDK links are hosted in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240911224732/ReleaseNote.html#prebuilt-flashable-images-along-with-esdk>`__.

     The Platform eSDK is an installer generated from the Qualcomm Linux software. It provides a complete Yocto environment that allows you to synchronize, modify, compile, and install applications and open-source plugins. For more details, see :ref:`How to download the Platform eSDK? <section_imr_xc4_1cc_vinayjk_07-12-24-1513-38-780>`.
    
   - For the list of chipsets supported on Qualcomm Linux, see `chip products <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-115/soc.html>`__.