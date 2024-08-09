.. _howto_sync:

Sync
====

.. _section_znd_jcg_v1c_vinayjk_03-22-24-1638-43-232:

Repo installation alternate methods
-----------------------------------

The latest Repo works with python3 and if your default Python is pointed
to python2, then install ``python-is-python3`` to make python3 as the
default Python.

::

   mkdir -p ~/bin
   cd ~/bin
   # If you already have a previous directory of repo_tool, you can delete it
   rm -rf ~/bin/repo_tool
   git clone https://android.googlesource.com/tools/repo.git -b v2.41 repo_tool
   cd repo_tool
   git checkout -b v2.41
   export PATH=~/bin/repo_tool:$PATH

If the previous steps do not work, you can install Repo using the
following commands:

::

   # Install curl (if it is not installed)
   sudo apt install curl bc
    
   # Latest Repo version works with python3
    
   mkdir -p ~/bin
   curl https://raw.githubusercontent.com/GerritCodeReview/git-repo/v2.41/repo -o ~/bin/repo && chmod +x ~/bin/repo
   export PATH=~/bin:$PATH

.. _section_em1_xng_q1c_vinayjk_03-04-24-2103-35-76:

How does QSC-CLI work?
----------------------

1. **Setup**

   QSC-CLI installs Docker and sets up the necessary git configuration.

2. **Sync**

   QSC-CLI downloads the firmware sources as needed based on the input
   parameters and completes the Qualcomm Yocto layers build.

3. **Build**

   QSC-CLI builds the Qualcomm firmware as needed and also completes the
   build for the Qualcomm Yocto layers.

4. Internally, QSC-CLI implements the standalone commands covered in the
   `Github workflow (firmware and extras) <build_addn_info.rst>`__ and
   leverages the prebuilt Docker images for the respective Qualcomm
   style software images. For example, ``LE.QCLINUX.1.0.r1``.

.. _section_gcp_5hh_q1c_vinayjk_03-04-24-2335-11-750:

How to see more information on commands and options supported by QSC-CLI?
-------------------------------------------------------------------------

To see all the commands provided by QSC-CLI, run the following commands:

::

   qsc-cli -h
   qsc-cli download –h

To see more details about a particular command, you can append ``-h`` to
the command. For example:

::

   qsc-cli compile -h

.. _section_hxv_5hh_q1c_vinayjk_03-04-24-2335-16-353:

How to manage workspaces using QSC-CLI?
---------------------------------------

List the workspaces using the following command:

::

   qsc-cli workspace info --list

To delete a workspace, run the following command:

::

   qsc-cli workspace delete --workspace-path <workspace_path>

   # Example
   qsc-cli workspace delete --workspace-path '/local/mnt/worskspace/Qworkspace_QIMPSDK'

.. _section_kw3_vhh_q1c_vinayjk_03-04-24-2335-25-119:

How do I find my Yocto workspace with QSC-CLI?
----------------------------------------------

You can install the ``tree`` command and run it on your workspace. The
Yocto workspace is under the ``LE.QCLINUX.1.0.r1`` directory as shown in
the following views. These directories stay the same for future
releases.

-  **QSC-CLI workspace structure after
   ``Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK`` distribution
   build**

   The following is a sample view, in which:

   -  ``LE.QCLINUX.1.0.r1`` contains the Yocto workspace.
   -  Remaining directories can be ignored as all the necessary sources
      and binaries are encoded in the Yocto layer recipes synced under
      ``LE.QCLINUX.1.0.r1/layers``. |image1|

-  **QSC-CLI workspace structure after
   ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build
   with firmware and extras**

   The following is a sample view, in which:

   -  ``LE.QCLINUX.1.0.r1`` contains the Yocto workspace.
   -  Few additional directories are for the Qualcomm firmware. While
      building with extras:

      -  These additional firmware are built.
      -  The output binaries from these are picked up from firmware
         recipes in the Qualcomm Yocto layers.
      -  For detailed sync and build instructions, see `Github workflow
         (firmware and extras) <build_addn_info.rst>`__. |image2|

.. _section_whj_vhh_q1c_vinayjk_03-04-24-2335-25-416:

How to refresh the workspace with a new download using QSC CLI?
---------------------------------------------------------------

This option is supported only for the ``LE.QCLINUX.1.0.r1`` image, which
syncs the Yocto layers and prepares the Yocto workspace to be built.
This includes the following steps:

**Note:** Get to a Docker shell as mentioned in `How to generate
eSDK <howto_build.rst#section_bcj_vhh_q1c_vinayjk_03-04-24-2335-25-265>`__.

1. Download a new release:

   ::

      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <release tag>
      repo sync

   **Note:** For the ``<manifest release tag>`` information, see the
   *Build-critical release tags* section in the `Release
   Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.
   An example ``<manifest release tag>`` is
   ``qcom-6.6.28-QLI.1.1-Ver.1.1.xml``.

2. Set up the build environment:

   ::

      MACHINE=qcm6490 DISTRO=qcom-wayland source setup-environment

3. Build the software image:

   ::

      bitbake qcom-multimedia-image



.. |image1| image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_2.png
.. |image2| image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_4.png
