.. _intro:

Introduction
==============

Qualcomm recommends that you read the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-27>`__ before starting your build.

You can do any of the following:
 
- Download prebuilt images and flash the software
- Sync, build, and flash the software

Download prebuilt images and flash the software
------------------------------------------------

- Download prebuilt flashable images (includes Platform eSDK) using the links in the table *Artifactory links to prebuilt flashable images and eSDK* of the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/ReleaseNote.html#prebuilt-flashable-images-along-with-esdk>`__.

- The Platform eSDK is an installer generated from the Qualcomm Linux software. It provides a complete Yocto environment that allows you to sync, modify, compile, and install applications and open-source plug-ins. For more information, see :ref:`Download the Platform eSDK <download_platform_esdk>`.

- To flash the prebuilt flashable images, see :ref:`Flash software images <flash_images>`.

Sync, build, and flash the software
-------------------------------------

Unregistered users
^^^^^^^^^^^^^^^^^^^

Unregistered users can sync and build Qualcomm Linux using the :doc:`Build with GitHub for unregistered users <github_workflow_unregistered_users>`. Qualcomm exclusively offers firmware components in the form of binary files to users who aren't registered.

Registered users
^^^^^^^^^^^^^^^^^

.. note:: To register, go to https://www.qualcomm.com/support/working-with-qualcomm.

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
   * - .. centered:: :doc:`Build with QSC Launcher <build_from_source_qsc_gui_intro>` 
     - .. centered:: :doc:`Build with QSC CLI <build_frm_source_qsc_cli>`
     - .. centered:: :doc:`Build with GitHub for registered users <build_from_source_github_intro>`

.. only:: html
 
   .. figure:: ../../media/k2c-qli-build-ga/explore_QSC_html.svg
      :align: center

.. only:: latex
 
   .. figure:: ../../media/k2c-qli-build-ga/explore_QSC_pdf.svg
      :align: center

.. note::
  
   - See `hardware SoCs <https://docs.qualcomm.com/bundle/publicresource/topics/80-70018-115/soc.html>`__ that are supported on Qualcomm Linux.
   - For QCS615 build instructions, see `QCS615.LE.1.0 Software User Guide <https://docs.qualcomm.com/bundle/resource/topics/80-70018-75/introduction_1.html>`__.