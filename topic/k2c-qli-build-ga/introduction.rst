.. _intro:

Introduction
==============

Qualcomm recommends that you read the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27>`__ before starting your build. You can do any of the following:
 
- Download prebuilt images and flash the software
- Sync, build, and flash the software

Download prebuilt images and flash the software
------------------------------------------------

- In the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-250617225208/ReleaseNote.html#prebuilt-flashable-images-along-with-esdk>`__, go to the *Artifactory links to prebuilt flashable images and eSDK* table to download prebuilt flashable images and the Platform eSDK.

- The Platform eSDK is an installer generated from the Qualcomm Linux software. It provides a complete Yocto environment that allows you to sync, change, compile, and install applications and open-source plug-ins. For more information, see :ref:`Download the Platform eSDK <download_platform_esdk>`.

- To flash the prebuilt flashable images, see :ref:`Flash software images <flash_images>`.

Sync, build, and flash the software
-------------------------------------

Unregistered users
^^^^^^^^^^^^^^^^^^^

If you're an unregistered Qualcomm user, you can sync and build Qualcomm Linux. For more information about using firmware components in the form of binary files, see :doc:`Build with GitHub for unregistered users <github_workflow_unregistered_users>`.

Registered users
^^^^^^^^^^^^^^^^^

.. note:: To register with Qualcomm, go to https://www.qualcomm.com/support/working-with-qualcomm.

If you're a registered user, you can use any one of the following three methods to sync and build Qualcomm Linux. These methods use the Qualcomm Yocto layers and the supporting base Yocto layers. You can access the source of certain firmware components and Qualcomm tools, which lets you build and download the software.

.. list-table:: 
   :header-rows: 1
   :align: center
   :class: longtable
 
   * - Launcher
     - CLI
     - GitHub
   * - GUI-based Qualcomm\ :sup:`®` Software Center (QSC) Launcher.
     - QSC command-line interface (CLI).
     - A GitHub-based workflow.
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
  
   - See `hardware SoCs <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-115/soc.html>`__ that are supported on Qualcomm Linux.
   - For IQ-615 build instructions, see `Qualcomm IQ-6 Quick Start Guide` [DOC NOT PUBLISHED YET].