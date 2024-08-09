*************
Introduction
*************

This guide describes the methods to configure, download, compile, and
flash Qualcomm® Linux® and the associated firmware on supported devices.
**This information is also available in** `Simplified
Chinese <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-254Y>`__.

Qualcomm recommends that you read the `Qualcomm Linux Yocto
Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70014-27>`__
before starting your build.

.. _section_rtp_vbg_qbc_vinayjk_06-05-24-1835-16-723:

Unregistered users
------------------

Unregistered users can sync and build Qualcomm Linux using the `GitHub
workflow for unregistered
users <github_workflow_unregistered_users.rst>`__.

.. _section_x3d_nqy_v1c:

Registered users
----------------

Registered users can use any one of the following three methods to sync
and build Qualcomm Linux. These methods use the Qualcomm Yocto layers
and the supporting base Yocto layers.

+--------------------------------+-----------+------------------------+
| Launcher                       | CLI       | GitHub                 |
+================================+===========+========================+
| Easy-to-use, GUI-based,        | Simple    | Instructions to use    |
| Qualcomm Software Center (QSC) | QSC       | GitHub based workflow. |
| Launcher.                      | com       |                        |
|                                | mand-line |                        |
|                                | interface |                        |
|                                | (CLI).    |                        |
+--------------------------------+-----------+------------------------+
| `Build with QSC                | `Build    | `GitHub workflow for   |
| Launcher <build_fr             | with QSC  | registered             |
| om_source_qsc_gui_intro.rst>`__ | C         | users <build_from_sour |
|                                | LI <build | ce_github_intro.rst>`__ |
|                                | _from_sou |                        |
|                                | rce_QSC_C |                        |
|                                | LI.rst>`__ |                        |
+--------------------------------+-----------+------------------------+

.. note::

    Prebuilt binaries along with Platform eSDK links are hosted in the `Release
    Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/ReleaseNote.html#prebuilt-flashable-images-along-with-esdk>`__.

    The Platform eSDK is an installer generated from the Qualcomm Linux image. It provides a complete Yocto environment that allows you to
    synchronize, modify, compile, and install applications and open-source plugins. For more details, see `How to download the PlatformeSDK? <howto_build.rst#section_imr_xc4_1cc_vinayjk_07-12-24-1513-38-780>`__.

.. note::
     Qualcomm Linux allows you to build images for QCS6490 and QCS5430.
     QCM6490 and QCS6490 are used interchangeably in this guide.
