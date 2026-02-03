.. _howto_sync:

Sync
---------------

.. _alternative_methods_install_repo:

Alternative methods to install Repo
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- The latest Repo works with python3. If your default Python is python2, then install ``python-is-python3`` to make python3 as the default Python.

   .. container:: nohighlight
      
      ::

         mkdir -p ~/bin
         cd ~/bin
         # If you already have a previous directory of repo_tool, you can delete it
         rm -rf ~/bin/repo_tool
         git clone https://android.googlesource.com/tools/repo.git -b v2.41 repo_tool
         cd repo_tool
         git checkout -b v2.41
         export PATH=~/bin/repo_tool:$PATH

- If the earlier steps didn't work, install Repo using the following commands:

   .. container:: nohighlight
      
      ::

         # Install curl (if it isn't installed)
         sudo apt install curl bc
      
         # Latest Repo version works with python3
      
         mkdir -p ~/bin
         curl https://raw.githubusercontent.com/GerritCodeReview/git-repo/v2.41/repo -o ~/bin/repo && chmod +x ~/bin/repo
         export PATH=~/bin:$PATH

How does QSC CLI work?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. **Setup**

   QSC CLI installs Docker and configures git.

2. **Sync**

   QSC CLI downloads the firmware sources and the Qualcomm Yocto layers, based on the input parameters.

3. **Build**

   QSC CLI builds the necessary Qualcomm firmware and the Qualcomm Yocto layers.

4. Internally, QSC CLI implements the standalone commands covered in the :doc:`Build from Source (with firmware and extras) <build_addn_info>` and leverages the prebuilt Docker images for the respective Qualcomm style software images. For example, ``LE.QCLINUX.1.0.r1``.

View information about QSC CLI commands
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To see all the commands provided by QSC CLI, run the following commands:

.. container:: nohighlight
      
   ::

      qsc-cli -h
      qsc-cli chip-software download –h

To see more details about a particular command, you can append ``-h`` to the command. For example:

.. container:: nohighlight
      
   ::

      qsc-cli chip-software compile -h

Manage workspaces using QSC CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

List the workspaces using the following command:

.. container:: nohighlight
      
   ::

      qsc-cli chip-software list-workspace

To delete a workspace, run the following command:

.. container:: nohighlight
      
   ::

      qsc-cli chip-software delete-workspace --workspace-path <workspace_path>

      # Example, qsc-cli chip-software delete-workspace --workspace-path '/local/mnt/workspace/Qworkspace_QIMPSDK'

Find a Yocto workspace using QSC CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can install the ``tree`` command and run it on your workspace. The Yocto workspace is under the ``LE.QCLINUX.1.0.r1`` directory. These directories stay the same for future releases.

-  **QSC CLI workspace structure after ``Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK`` distribution
   build**

   The following is a sample view, in which:

   -  ``LE.QCLINUX.1.0.r1`` has the Yocto workspace.
   -  Ignore the remaining directories as all the necessary sources
      and binaries are encoded in the Yocto layer recipes synced under
      ``LE.QCLINUX.1.0.r1/layers``.       
      |YoctoLEQCLinux|

-  **QSC CLI workspace structure after
   ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`` distribution build
   with firmware and extras**

   The following is a sample view, in which:

   -  ``LE.QCLINUX.1.0.r1`` has the Yocto workspace.
   -  Few additional directories are for the Qualcomm firmware. While
      building with extras:

      -  The additional firmware is built.
      -  The output binaries from these are taken from the firmware
         recipes in the Qualcomm Yocto layers.
      -  For detailed sync and build instructions, see :doc:`Build from Source (with firmware and extras) <build_addn_info>`.
         |ws_qsc_cli_4|

Refresh the workspace with a new download using QSC CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

This option is supported only for the ``LE.QCLINUX.1.0.r1`` image, which
syncs the Yocto layers and prepares to build the Yocto workspace.
This includes the following steps:

1. Get to a Docker shell as mentioned in :ref:`Generate an eSDK <how_to_build_generate_sdk>`.

#. Download a new release. For the ``<manifest release tag>`` information, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__. An example ``<manifest release tag>`` is ``qcom-6.6.116-QLI.1.7-Ver.1.1.xml``.

   .. container:: nohighlight
      
      ::

         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m <release tag>
         repo sync

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment

   To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image

.. |YoctoLEQCLinux| image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_2.png
.. |ws_qsc_cli_4| image:: ../../media/k2c-qli-build-ga/ws_qsc_cli_4.png
