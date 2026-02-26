.. _setup_local_firwmare:

Setting up local firmware binaries
--------------------------------------

Once the firmware compliations are done, you can run these commands to include your local firmware binaries in the Yocto image 
compliation rather than the upstream firmware binaries. These commands provide examples for overriding firmware for QCS9100 chipset.
The appropriate paths for the other chipsets are documented in the `Release Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.

#. As a prerequisite, set the path to the firmware binaries for use in subsequent commands.

  .. container:: nohighlight
    
    ::

        # The firmware recipe is compiled when the Yocto build is initiated.
        # Example, for QCS9100, the directory path must contain QCS9100_bootbinaries.zip, QCS9100_dspso.zip, and QCS9100_fw.zip.
        # Set the environment variable to pick up the prebuilts:
        export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-2-0_ap_standard_oem_nomodem/<product>/common/build/ufs/bin"


Overriding linux-firmware binaries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the linux firmware recipe. This recipe provides the firmware binaries to meta-qcom. 

  .. container:: nohighlight
    
    ::

        # Create a kas configuration file for overriding the default revision for linux-firmware.git
        cat << EOF > meta-qcom/ci/firwmare.yml
        header:
          version: 14

        local_conf_header:
          firmware-build: |
            "SRCREV:pn-linux-firmware = "06a743fd69999590e88199bb9edba9d5b73d6ad1"
        EOF

        kas shell -c "devtool modify linux-firmware" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml:meta-qcom/ci/firmware.yml
        # Example, kas shell -c "devtool modify linux-firmware" meta-qcom/ci/qcs9100-ride-sx:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml:meta-qcom/ci/firmware.yml

  .. note::
  
      To find the correct value to be assigned to SRCREV:pn-linux-firmware
      first check the version of the linux-firmware recipe under the path
      oe-core/meta/recipes-kernel/linux-firmware/.

      The file name may be as follows linux-firmware_20260110.bb,
      In such case find the matching SRCREV from the linux-firmware git tags
      https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/refs/tags

      The SRCREV:pn-linux-firmware should be set as follows

      SRCREV:pn-linux-firmware = "06a743fd69999590e88199bb9edba9d5b73d6ad1"

#. Unzip the bootbinaries zip file if the unzipped version is not available.  

  .. container:: nohighlight
    
    ::

        unzip $FWZIP_PATH/<chipset>_fw.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_fw.zip -d $FWZIP_PATH

#. Copy the contents of the firmware zip file to the devtool workspace

  .. container:: nohighlight
    
    ::

        cp $FWZIP_PATH/<chipset>_fw/lib/firmware/<path-to-firmware>/*.{mbn,jsn,elf} build/workspace/sources/linux-firmware/<path-to-firmware>/
        # Example, cp $FWZIP_PATH/QCS9100_fw/lib/firmware/qcom/sa8775p/*.{mbn,jsn,elf} build/workspace/sources/linux-firmware/qcom/sa8775p/

.. note:: If you are overriding the firmwares in linux-firmware, you would always want to override the hexagon dsp binaries as well.

Overriding DSPSO binaries 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the hexagon-dsp-binaries recipe. This recipe provides the dspso binaries to meta-qcom. 

  .. container:: nohighlight
    
    ::

        kas shell -c "devtool modify hexagon-dsp-binaries" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml
        # Example, kas shell -c "devtool modify hexagon-dsp-binaries" meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml

#. Unzip the bootbinaries zip file if the unzipped version is not available.  

  .. container:: nohighlight
    
    ::

        unzip $FWZIP_PATH/<chipset>_fw.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_fw.zip -d $FWZIP_PATH

#. Copy the dsp so binaries from the firmware zip file to the devtool workspace

  .. container:: nohighlight
    
    ::

        # Copy the dsp so binaries for your chipset for adsp, cdsp and gdsp
        cp $FWZIP_PATH/<chipset>_fw/usr/share/<path-to-chipset-dspso>/adsp/* build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-adsp-dspso>/
        cp $FWZIP_PATH/<chipset>_fw/usr/share/<path-to-chipset-dspso>/cdsp/* build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-cdsp-dspso>/
        cp $FWZIP_PATH/<chipset>_fw/usr/share/<path-to-chipset-dspso>/gdsp/* build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-gdsp-dspso>/

        # Remove any extra files that aren't natively provided by the hexagon-dsp-binaries
        rm build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-adsp-dspso>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-cdsp-dspso>/*.txt
        rm build/workspace/sources/hexagon-dsp-binaries/<path-to-chipset-gdsp-dspso>/*.txt

        # Example,
        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/adsp/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/adsp-DSP.AT.1.0.1-00190-LEMANS-1/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/adsp-DSP.AT.1.0.1-00190-LEMANS-1/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/cdsp/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp-DSP.AT.1.0.1-00190-LEMANS-1/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp-DSP.AT.1.0.1-00190-LEMANS-1/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/cdsp1/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp1-DSP.AT.1.0.1-00190-LEMANS-1/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/cdsp1-DSP.AT.1.0.1-00190-LEMANS-1/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/gdsp0/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp0-DSP.AT.1.0.1-00190-LEMANS-1/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp0-DSP.AT.1.0.1-00190-LEMANS-1/*.txt

        # cp $FWZIP_PATH/QCS9100_fw/usr/share/qcom/sa8775p/Qualcomm/SA8775P-RIDE/dsp/gdsp1/* build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp1-DSP.AT.1.0.1-00190-LEMANS-1/
        # rm build/workspace/sources/hexagon-dsp-binaries/sa8775p/Qualcomm/SA8775P-RIDE/gdsp1-DSP.AT.1.0.1-00190-LEMANS-1/*.txt

Overriding boot firmware binaries
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Create a local devtool workspace for the boot critical firmware recipes

  .. container:: nohighlight
    
    ::

        kas shell -c "devtool modify firmware-qcom-boot-<chipset>" meta-qcom/ci/<machine.yml>:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml
        # Example, kas shell -c "devtool modify firmware-qcom-boot-qcs9100" meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro-prop-image.yml:meta-qcom/ci/linux-qcom-6.18.yml:meta-qcom/ci/lock.yml

#. Unzip the bootbinaries zip file if the unzipped version is not available.  

  .. container:: nohighlight
    
    ::

        unzip $FWZIP_PATH/<chipset>_bootbinaries.zip -d $FWZIP_PATH
        # Example, unzip $FWZIP_PATH/QCS9100_bootbinaries.zip -d $FWZIP_PATH

#. Copy the contents of the bootbinaries zip file to the devtool workspace

  .. container:: nohighlight
    
    ::

        cp -r $FWZIP_PATH/<chipset>_bootbinaries/* build/workspace/sources/firmware-qcom-boot-<chipset>
        # Example, cp -r $FWZIP_PATH/QCS9100_bootbinaries/* build/workspace/sources/firmware-qcom-boot-qcs9100

        # meta-qcom boot firmware recipes need this configuration to work correctly. 
        # pick the correct bbappend based on the recipe synced to your workspace
        echo 'ALLOW_EMPTY:${PN} = "1"' >> build/workspace/appends/firmware-qcom-boot-qcs9100_00116.0.bbappend

Once these steps are run, the next Yocto image build should pickup the local firmware binaries.
