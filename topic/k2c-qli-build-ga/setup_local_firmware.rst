.. _setup_local_firmware:

Set up local firmware binaries
--------------------------------------

Once the firmware compilations are complete, you can run these commands to include your local firmware binaries in the Yocto image compilation rather than the upstream firmware binaries. These commands provide examples to override firmware for the QCS9100 chipset. The appropriate paths for other chipsets are documented in the `Release Notes <https://docs.qualcomm.com/doc/80-80022-300/>`__.

.. rubric:: Prerequisite
  
   Set the path to the firmware binaries for use in subsequent commands

   .. container:: nohighlight
    
      ::

        # The firmware recipe is compiled when the Yocto build is initiated.
        # Example, for QCS9100, the directory path must contain QCS9100_bootbinaries.zip, QCS9100_dspso.zip, and QCS9100_fw.zip.
        # Set the environment variable to pick up the prebuilts
        export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-2-0_ap_standard_oem_nomodem/<fwzip-path-suffix>"
        # Example, export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-2-0_ap_standard_oem_nomodem/QCS9100.LE.2.0/common/build/ufs/bin"

Override Linux-firmware binaries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the Linux firmware recipe. This recipe provides the firmware binaries to ``meta-qcom``.

   .. container:: nohighlight
    
      ::

         # Create a kas configuration file for overriding the default revision for linux-firmware.git
         cat << EOF > meta-qcom/ci/firmware.yml
         header:
          version: 14

         local_conf_header:
          firmware-build: |
            SRCREV:pn-linux-firmware = "c822cbbb14ce5b8ee1f27346220640ac350bbf34"
         EOF

         kas shell -c "devtool modify linux-firmware" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml:meta-qcom/ci/firmware.yml
         # Example, kas shell -c "devtool modify linux-firmware" meta-qcom/ci/iq-9075-evk.yml:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml:meta-qcom/ci/firmware.yml

    .. note::
  
        To find the correct value to be assigned to ``SRCREV:pn-linux-firmware``, first check the version of the ``linux-firmware`` recipe under the path ``oe-core/meta/recipes-kernel/linux-firmware/``. If the file name is ``linux-firmware_20260221.bb``, find the matching SRCREV from the `linux-firmware git tags <https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/refs/tags>`__.

        The ``SRCREV:pn-linux-firmware`` must be set as follows:

        .. container:: nohighlight

           ::

              SRCREV:pn-linux-firmware = "c822cbbb14ce5b8ee1f27346220640ac350bbf34"

#. Unzip the frmware zip file if you do not have the unzipped version.

   .. container:: nohighlight
    
      ::

        unzip $FWZIP_PATH/<soc-firmware-zip-name>.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_fw.zip -d $FWZIP_PATH

#. Copy the contents of the firmware zip file into the devtool workspace

   .. container:: nohighlight
    
      ::

        cp $FWZIP_PATH/<soc-firmware-zip-name>/lib/firmware/qcom/<chipset-name>/*.{mbn,jsn} build/workspace/sources/linux-firmware/qcom/<chipset-name>/
        # Example, cp $FWZIP_PATH/QCS9100_fw/lib/firmware/qcom/sa8775p/*.{mbn,jsn} build/workspace/sources/linux-firmware/qcom/sa8775p/

.. note:: When you override the firmwares in ``linux-firmware``, you must also override the Hexagon DSP binaries.

Override DSPSO binaries 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the hexagon-dsp-binaries recipe. This recipe provides the DSPSO binaries to meta-qcom. 

   .. container:: nohighlight
    
      ::

         kas shell -c "devtool modify hexagon-dsp-binaries" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml
         # Example, kas shell -c "devtool modify hexagon-dsp-binaries" meta-qcom/ci/iq-9075-evk.yml:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml

#. Unzip the firmware zip file if the unzipped version is not available

   .. container:: nohighlight
    
      ::

        unzip $FWZIP_PATH/<soc-firmware-zip-name>.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_fw.zip -d $FWZIP_PATH

#. Copy the DSPSO binaries from the firmware zip file to the devtool workspace

  .. container:: nohighlight
    
    ::

        # Copy the dsp so binaries for your chipset for ADSP, CDSP, CDSP1, GDSP0 and GDSP1 as applicable
        cp $FWZIP_PATH/<soc-firmware-zip-name>/usr/share/qcom/<dspso-path-subdir>/dsp/adsp/* build/workspace/sources/hexagon-dsp-binaries/<dspso-path-subdir>/<hexagon-adsp-bins-subdir>/
        cp $FWZIP_PATH/<soc-firmware-zip-name>/usr/share/qcom/<dspso-path-subdir>/dsp/cdsp/* build/workspace/sources/hexagon-dsp-binaries/<dspso-path-subdir>/<hexagon-cdsp0-bins-subdir>/
        cp $FWZIP_PATH/<soc-firmware-zip-name>/usr/share/qcom/<dspso-path-subdir>/dsp/cdsp1/* build/workspace/sources/hexagon-dsp-binaries/<dspso-path-subdir>/<hexagon-cdsp1-bins-subdir>/
        cp $FWZIP_PATH/<soc-firmware-zip-name>/usr/share/qcom/<dspso-path-subdir>/dsp/gdsp0/* build/workspace/sources/hexagon-dsp-binaries/<dspso-path-subdir>/<hexagon-gdsp0-bins-subdir>/
        cp $FWZIP_PATH/<soc-firmware-zip-name>/usr/share/qcom/<dspso-path-subdir>/dsp/gdsp1/* build/workspace/sources/hexagon-dsp-binaries/<dspso-path-subdir>/<hexagon-gdsp1-bins-subdir>/

        # Remove any extra files that aren't natively provided by the hexagon-dsp-binaries
        rm build/workspace/sources/hexagon-dsp-binaries/<hexagon-dsp-bins-subdir>/<hexagon-adsp-bins-subdir>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<hexagon-dsp-bins-subdir>/<hexagon-cdsp-bins-subdir>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<hexagon-dsp-bins-subdir>/<hexagon-cdsp1-bins-subdir>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<hexagon-dsp-bins-subdir>/<hexagon-gdsp0-bins-subdir>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<hexagon-dsp-bins-subdir>/<hexagon-gdsp1-bins-subdir>/*.txt

        # Example,
        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/adsp/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/adsp-DSP.AT.1.0.1-00196-LEMANS-2/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/adsp-DSP.AT.1.0.1-00196-LEMANS-2/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/cdsp/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp-DSP.AT.1.0.1-00196-LEMANS-2/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp-DSP.AT.1.0.1-00196-LEMANS-2/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/cdsp1/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp1-DSP.AT.1.0.1-00196-LEMANS-2/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp1-DSP.AT.1.0.1-00196-LEMANS-2/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/gdsp0/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp0-DSP.AT.1.0.1-00196-LEMANS-2/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp0-DSP.AT.1.0.1-00196-LEMANS-2/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/gdsp1/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp1-DSP.AT.1.0.1-00196-LEMANS-2/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp1-DSP.AT.1.0.1-00196-LEMANS-2/*.txt

.. note::
    Copy the dsp so binaries for your chipset for ADSP, CDSP, CDSP1, GDSP0 and GDSP1 as applicable. You can refer to the documentation in the `Release Notes<https://docs.qualcomm.com/doc/80-80022-300/topic/build_critical_release_tags.html>`__.

Override boot firmware binaries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the boot critical firmware recipes

   .. container:: nohighlight
    
      ::

        kas shell -c "devtool modify <firmware-bootbinaries-recipe>" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml
        # Example, kas shell -c "devtool modify firmware-qcom-boot-qcs9100" meta-qcom/ci/iq-9075-evk.yml:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml

#. Unzip the bootbinaries zip file if the unzipped version is not available

   .. container:: nohighlight
    
      ::

        unzip $FWZIP_PATH/<soc-bootbinaries-zip-name>.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_bootbinaries.zip -d $FWZIP_PATH

#. Copy the contents of the bootbinaries zip file to the devtool workspace

   .. container:: nohighlight
    
      ::

        cp -r $FWZIP_PATH/<soc-bootbinaries-zip-name>/* build/workspace/sources/<firmware-bootbinaries-recipe>
        # Example, cp -r $FWZIP_PATH/QCS9100_bootbinaries/* build/workspace/sources/firmware-qcom-boot-qcs9100

        # meta-qcom boot firmware recipes need this configuration to work correctly. 
        # pick the correct bbappend based on the recipe synced to your workspace
        echo 'ALLOW_EMPTY:${PN} = "1"' >> build/workspace/appends/<firmware-bootbinaries-bbappend-file>
        # Example, echo 'ALLOW_EMPTY:${PN} = "1"' >> build/workspace/appends/firmware-qcom-boot-qcs9100_00123.bbappend

After you run these steps, the next :ref:`Yocto image build <step4_build_software_image>` will pick up the local firmware binaries.
