.. _intro:

Build overview
===============

Qualcomm Linux uses Yocto to build an embedded system software image. Hence, Qualcomm recommends that you read the `Qualcomm Linux Yocto Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27>`__ before starting your build. You can do any of the following:
 
- Download prebuilt images and flash the software
- Sync, build, and flash the software

Download prebuilt images and flash the software
------------------------------------------------

- In the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/topic/prebuilt_flashable_images_along_with_esdk.html>`__, go to the *Artifactory links to prebuilt flashable images and eSDK* table to download prebuilt flashable images and the Platform eSDK.

.. note:: The Platform eSDK is an installer that allows you to synchronize, modify, compile, and install applications and open-source plug-ins within a complete Yocto environment.

- To flash the prebuilt images, see :ref:`Flash software images <flash_images>`.

Sync, build, and flash the software
----------------------------------------------

- Unregistered users

  If you're an unregistered Qualcomm user, you can sync and build Qualcomm Linux. For more information about using firmware components in the form of binary files, see :doc:`Build from Source <build_from_source_github_intro>`.
  
- Registered users

  .. note:: You must `register with Qualcomm <https://www.qualcomm.com/support/working-with-qualcomm>`__ to sync and build Qualcomm Linux.

  If you're a registered user, you can use the following method to sync and build Qualcomm Linux. These methods use the Qualcomm Yocto layers and the supporting base Yocto layers. You can access the source of certain firmware components and Qualcomm tools, which lets you build and download the software.

  .. list-table:: 
     :header-rows: 1
     :align: center
     :class: longtable
  
     * - Manual build 
       - CLI
     * - Build using selective proprietary sources and binaries/libraries. 
       - QSC command-line interface (CLI).
     * - :ref:`Build from source (with firmware and extras) <build_addn_info>`
       - :ref:`Build with QSC CLI <build_frm_source_qsc_cli>`

.. only:: html
   
   .. figure:: ../../media/k2c-qli-build-ga/explore_QSC_html.svg
      :align: center

.. only:: latex
   
   .. figure:: ../../media/k2c-qli-build-ga/explore_QSC_pdf.svg
      :align: center

.. note:: 
   
   - See `Hardware SoCs <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-115/soc.html>`__ that are supported on Qualcomm Linux.
   - For IQ-615 build instructions, see `Qualcomm Dragonwing™ IQ-615 Beta Evaluation Kit quickstart <https://docs.qualcomm.com/bundle/80-70020-293/resource/80-70020-293.pdf>`__.