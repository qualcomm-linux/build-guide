.. _launcher_download_sw:

Download the software
----------------------

1. To open the QSC Launcher desktop application, launch :guilabel:`Qualcomm Software Center` from the :guilabel:`Applications` menu.

   .. note:: 
      For the Launcher workflow to detect connected devices and flash software builds, install the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) tools on the host computer. Select :guilabel:`PCAT` to install PCAT and :guilabel:`QUD` to install QUD as shown in the following image:

      .. image:: ../../media/k2c-qli-build-ga/QSC_has_PCAT_QUD_install_info.png

      **Or**

      Install PCAT and QUD using ``qsc-cli``:

      .. container:: nohighlight
         
         ::

            qsc-cli login
            qsc-cli tool install --name quts --activate-default-license
            qsc-cli tool install --name qud --activate-default-license
            qsc-cli tool install --name pcat --activate-default-license

      The ``qsc-cli --help`` command lists the help options.

      For Ubuntu 24.04, you might see an issue while installing QUD where you must enroll the public key on your Linux host for a successful QUD installation. For more details, follow the steps provided in the ``signReadme.txt`` file available at the ``/opt/QTI/sign/`` directory.

#. Use your registered email ID to sign in to the QSC desktop application. To register your Qualcomm email ID, go to `Qualcomm support page <https://www.qualcomm.com/support/working-with-qualcomm>`__.

   .. image:: ../../media/k2c-qli-build-ga/start_launcher_ab.png

   -  If you don't have a connected device, click :guilabel:`Start Launcher` (A).
   -  If you have a connected device, select :guilabel:`Start Launcher` (B) for the appropriate device in the :guilabel:`Connected devices` panel.

#. On the :guilabel:`Specify Environment` page, select the appropriate values for :guilabel:`Category`, :guilabel:`Chipset`, :guilabel:`Host Operating System`, :guilabel:`Target Operating System`, and then select :guilabel:`Next`.

   .. note:: See `chipsets (hardware SoCs) <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-115/soc.html>`__ that are supported on Qualcomm Linux.

   .. image:: ../../media/k2c-qli-build-ga/specify_env.png

#. In the :guilabel:`Select Resources` window, do the following:
   
   .. image:: ../../media/k2c-qli-build-ga/select_resource_page.png

   a. In the :guilabel:`Base Workspace Path` text box, specify a directory where you want to download the software.

   b. Select the :guilabel:`Software Product`.

   c. Select the :guilabel:`Distribution` and the :guilabel:`Release Tag`.

      .. note::
         
         - For information about the supported distributions for your hardware SoCs, see the table *Access Controlled Distribution* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.
         - For information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.        
         - For information about the Qualcomm IM and QIR SDKs, see the following guides:

           - `Qualcomm IM SDK quickstart <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-51>`__
           - `QIR SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-265>`__

#. To download the selected compilable distribution or flashable binary, select :guilabel:`Download`. After the software download is successful, a *Download complete* status appears.

   .. image:: ../../media/k2c-qli-build-ga/software_download_complete.png
   
#. You don't have to compile flashable binaries. If you have selected a flashable binary, follow the on-screen instructions to flash to a connected device.

   .. image:: ../../media/k2c-qli-build-ga/prebuilt_compile.png
