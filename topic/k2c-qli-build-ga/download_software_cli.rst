.. _qsc_cli_software_download:

Download the software
----------------------

Download a software release by specifying the absolute workspace path, product ID, distribution, and release ID:

.. note:: If you are downloading more than one distribution, create a new workspace for each distribution that you download.

.. container:: nohighlight

   ::

      qsc-cli chip-software download --workspace-path '<Base_Workspace_Path>' --product '<Product_ID>' --distribution '<Distribution>' --release '<Release_ID>'
      # Example, qsc-cli chip-software download --workspace-path '/local/mnt/workspace/sample_workspace' --product 'QCS9100.LE.2.0' --distribution 'Qualcomm_Linux.SPF.2.0|TEST|DEVICE|PUBLIC' --release 'r00003.1'

.. note::
   - For the Product_ID, Distribution, and Release_ID values, see the table *QSC-CLI Input Parameters* in the `Release Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.
   - For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-80020-27/qualcomm_linux_metadata_layers.html>`__.
