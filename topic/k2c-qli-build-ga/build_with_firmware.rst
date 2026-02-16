Build with firmware sources
------------------------------------

Sync firmware
^^^^^^^^^^^^^^

Commands in the following sections are based on the binary and source for firmware images without modem and GPS (see the command in :ref:`Mapping firmware distributions to git repositories <Mapping_firmware_table>`). Hence, ``qualcomm-linux-spf-1-0_ap_standard_oem_nm`` is used. If you use any other distribution, then update the directory accordingly.

The following table describes the Qualcomm Yocto layers and release tags:

.. tabularcolumns:: |p{3cm}|p{4cm}|p{3cm}|p{4cm}|

.. flat-table:: Qualcomm Yocto layers and manifest tags
   :header-rows: 1
   :class: longtable

   * - Access level
     - Yocto layer
     - Release tag
     - Example
	 
   * - Public developers (unregistered)
     - ``meta-qcom``
     - meta-qcom-releases tag
     - qcom-6.18-QLI.2.0-Ver.1.0
   * - Licensed developers with authorized access
     - ``meta-qcom-extras``
     - meta-qcom-extras-release tag
     - qcom-6.18-QLI.2.0-Ver.1.0
   * - See :ref:`Mapping access levels to firmware distributions <build_mapping_access_levels>`
     - NA
     - firmware release tag
     - r1.0_00114.0

The following tables describe the firmware distributions that you can download. For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.

.. _build_mapping_access_levels:

.. flat-table:: Mapping access levels to firmware distributions
   :widths: 24 24 24
   :header-rows: 1
   :class: longtable table-wrap

   * - **Access level**
     - **Distribution**
     - Yocto layers
   * - Licensed developers with authorized access
     - BSP build: High-level OS and firmware source (GPS only)
       
       ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem``
     - 
       ``meta-qcom``

       ``meta-qcom-distro``
       
       ``meta-qcom-extras``
   * - :rspan:`2` Licensed developers (contact Qualcomm for access)
     - BSP build: High-level OS and firmware (GPS only) source
       
       ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|``
     - 
       ``meta-qcom``

       ``meta-qcom-distro``

       ``meta-qcom-extras``
   *  
     - BSP build: High-level OS and firmware (GPS and modem) source
      
        ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|``
     - ``meta-qcom``

       ``meta-qcom-distro``

       ``meta-qcom-extras``

The following table maps the firmware distributions to git repositories: 

.. _Mapping_firmware_table:

.. flat-table:: Mapping firmware distributions to git repositories
   :widths: 24 24 24
   :header-rows: 1
   :class: longtable

   * - Firmware distribution
     - Git command
     - Directory into which firmware gets synced on git clone

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nomodem.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem_nomodem``

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM\|
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem``

   * - Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM\|
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_amss_standard_oem.git``
     - ``qualcomm-linux-spf-1-0_amss_standard_oem``

The **Git command** column (in the :ref:`Mapping firmware distributions to git repositories <Mapping_firmware_table>` table) provides information about the git repositories that contain the firmware sources. Qualcomm servers host these repositories. Clone the appropriate repositories based on your access profile and use case.

The following ``git clone`` command downloads the selected firmware components in source, except the modem:

.. container:: nohighlight
      
   ::

      mkdir -p <FIRMWARE_ROOT>
      cd <FIRMWARE_ROOT>
      git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nm.git
      # Example, <firmware release tag> is r1.0_00114.0

The ``git clone`` command clones the content into the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm`` directory. For the latest ``<firmware release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

Build firmware
^^^^^^^^^^^^^^^^^^^^^

.. container:: persistenttab-soc

   .. tabs::

      .. group-tab:: QCS6490/QCS5430

         .. rubric:: Prerequisites

         -  Ensure that the working shell is ``bash``.

            .. container:: nohighlight
      
               ::

                  echo $0

            The expected output of the command should be ``bash``. If not, enter the bash shell:

            .. container:: nohighlight
      
               ::

                  bash

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates the header and the source files from the IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm\ :sup:`®` Trusted Execution Environment (TEE), and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qsc-cli and activate the license
                  qsc-cli login
                  qsc-cli tool activate-license --name sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qsc-cli tool install --name sdllvm_arm --required-version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for the Qualcomm TEE compilation is 16.0.7
                  qsc-cli tool install --name sdllvm_arm --required-version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCM6490.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCM6490.LE.1.0/common/sectoolsv2/ext/Linux

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor. Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qsc-cli``, see :ref:`Change the Hexagon tool install path <change_hex_tool_install_path>`.

            .. container:: nohighlight
      
               ::

                  qsc-cli tool extract --name hexagon8.4 --required-version 8.4.07
                  qsc-cli tool extract --name hexagon8.4 --required-version 8.4.10
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

         .. rubric:: Build cDSP 

         **Tools required**

         -  Compiler version: Hexagon 8.4.07
         -  Python version: Python 3.10.2
         -  libffi6 package 
         
         **Build steps**

         1. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/CDSP.HT.2.5.c3/cdsp_proc/build/ms

         2. Clean the build:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.cdsp.prod --clean

         3. Build the image:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.cdsp.prod

         .. rubric:: Build aDSP

         **Tools required**

         -  Compiler version: Hexagon 8.4.07
         -  Python version: Python 3.10.2
         -  libffi6 package 
         
         **Build steps**

         1. Nanopb integration (one-time setup):

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/ADSP.HT.5.5.c8/adsp_proc/qsh_api
                  curl https://jpa.kapsi.fi/nanopb/download/nanopb-0.3.9.5-linux-x86.tar.gz -o nanopb-0.3.9.5-linux-x86.tar.gz
                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/ADSP.HT.5.5.c8/adsp_proc/
                  python qsh_api/build/config_nanopb_dependency.py -f nanopb-0.3.9.5-linux-x86
         
         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/ADSP.HT.5.5.c8/adsp_proc/build/ms

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.adsp.prod --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.adsp.prod

         .. rubric:: Build Boot

         **Tools required**

         -  Compiler version: LLVM version must be updated to 14.0.4

            .. container:: nohighlight
      
               ::

                  export LLVM=<FIRMWARE_ROOT>/llvm/14.0.4/

         -  Python version: Python 3.10

         -  libffi6 package  
         
         **Build steps**

         1. Install the device tree compiler:

            .. container:: nohighlight
      
               ::

                  sudo apt-get install device-tree-compiler
                  export DTC=/usr/bin

         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt
                  pip install json-schema-for-humans

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t kodiak,QcomToolsPkg -v LAA -r RELEASE --build_flags=cleanall

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t kodiak,QcomToolsPkg -v LAA -r RELEASE

            .. note:: 
               For debug variant builds, replace ``RELEASE`` with ``DEBUG``.

         .. rubric:: Build Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  vi build_config_deploy_kodiak.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=kodiak --cfg=build_config_deploy_kodiak.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=kodiak --cfg=build_config_deploy_kodiak.xml

         .. rubric:: Build AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/AOP.HO.3.6/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_kodiak.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_kodiak.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: Build MPSS

         .. note:: This build is applicable only for ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|``.

         **Tools required**

         -  Compiler version: Hexagon 8.4.10
         -  Python version: Python 3.8.2
         
         **Build steps**

         1. Nanopb integration (one-time setup):

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_amss_standard_oem/MPSS.HI.4.3.3.c6.2/modem_proc/ssc_api
		            curl https://jpa.kapsi.fi/nanopb/download/nanopb-0.3.9.5-linux-x86.tar.gz -o nanopb-0.3.9.5-linux-x86.tar.gz
		            cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_amss_standard_oem/MPSS.HI.4.3.3.c6.2/modem_proc
		            python ssc_api/build/config_nanopb_dependency.py -f  nanopb-0.3.9.5-linux-x86
         
         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_amss_standard_oem/MPSS.HI.4.3.3.c6.2/modem_proc/build/ms

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_variant.py kodiak.gen.prod --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python build_variant.py kodiak.gen.prod bparams=-k

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation isn't required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation isn't required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation isn't required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation isn't required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         - Create an integrated firmware binary from the individual components that you compiled:

           .. container:: nohighlight
      
              ::

                cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCM6490.LE.1.0/common/build
                python build.py --imf

         - Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCM6490.LE.1.0/common/build/ufs/bin`` directory:

            -  ``QCM6490_bootbinaries.zip``
            -  ``QCM6490_dspso.zip``
            -  ``QCM6490_fw.zip``

      .. group-tab:: IQ-9075

         .. rubric:: Prerequisites

         -  Ensure that the working shell is ``bash``.

            .. container:: nohighlight
      
               ::

                  echo $0

            The expected output of the command should be ``bash``. If not, enter the bash shell:

            .. container:: nohighlight
      
               ::

                  bash

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates the header and the source files from the IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm TEE, and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qsc-cli and activate the license
                  qsc-cli login
                  qsc-cli tool activate-license --name sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qsc-cli tool install --name sdllvm_arm --required-version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for the Qualcomm TEE compilation is 16.0.7
                  qsc-cli tool install --name sdllvm_arm --required-version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS9100.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS9100.LE.1.0/common/sectoolsv2/ext/Linux

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor. Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qsc-cli``, see :ref:`Change the Hexagon tool install path <change_hex_tool_install_path>`.

            .. container:: nohighlight
      
               ::

                  qsc-cli tool extract --name hexagon8.6 --required-version 8.6.05.2
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

         .. rubric:: Build DSP      

         **Tools required**

         -  Compiler version: Hexagon 8.6.05.2
         -  Python version: Python 3.8.2
         
         **Build steps**

         1. Install the device tree compiler:

            .. container:: nohighlight
      
               ::

                  sudo apt-get install device-tree-compiler
                  export DTC_PATH=/usr/bin

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  pip install ruamel.yaml==0.17.17
                  pip install dtschema==2021.10
                  pip install jsonschema==4.0.0

         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/DSP.AT.1.0.1/dsp_proc/build/ms

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py lemans.adsp.prod --clean
                  python ./build_variant.py lemans.cdsp0.prod --clean
                  python ./build_variant.py lemans.cdsp1.prod --clean
                  python ./build_variant.py lemans.gpdsp0.prod --clean
                  python ./build_variant.py lemans.gpdsp1.prod --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py lemans.adsp.prod && python ./build_variant.py lemans.cdsp0.prod && python ./build_variant.py lemans.cdsp1.prod && python ./build_variant.py lemans.gpdsp0.prod && python ./build_variant.py lemans.gpdsp1.prod

         .. rubric:: Build Boot

         **Tools required**

         -  Compiler version: LLVM version must be updated to 14.0.4

            .. container:: nohighlight
      
               ::

                  export LLVM=<FIRMWARE_ROOT>/llvm/14.0.4/

         -  Python version: Python 3.10

         -  libffi6 package 
         
         **Build steps**

         1. Install the device tree compiler:

            .. container:: nohighlight
      
               ::

                  sudo apt-get install device-tree-compiler
                  export DTC=/usr/bin

         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt
                  pip install json-schema-for-humans

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t lemans,QcomToolsPkg -v LAA -r RELEASE --build_flags=cleanall

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t lemans,QcomToolsPkg -v LAA -r RELEASE

            .. note:: 
               For debug variant builds, replace ``RELEASE`` with ``DEBUG``.

         .. rubric:: Build Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  vi build_config_deploy_lemans.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=lemans --cfg=build_config_deploy_lemans.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=lemans --cfg=build_config_deploy_lemans.xml

         .. rubric:: Build AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/AOP.HO.3.6.1/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_lemansau.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_lemansau.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation isn't required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation isn't required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation isn't required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation isn't required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         - Create an integrated firmware binary from the individual components that you compiled:

           .. note:: Apply all the changes from the section *Additional information* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/topic/additional_information.html>`__.

           .. container:: nohighlight
      
              ::

                cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS9100.LE.1.0/common/build
                python build.py --imf

         - Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCM6490.LE.1.0/common/build/ufs/bin`` directory:

            -  ``QCS9100_bootbinaries.zip``
            -  ``QCS9100_dspso.zip``
            -  ``QCS9100_fw.zip``

      .. group-tab:: IQ-8275

         .. rubric:: Prerequisites

         -  Ensure that the working shell is ``bash``.

            .. container:: nohighlight
      
               ::

                  echo $0

            The expected output of the command should be ``bash``. If not, enter the bash shell:

            .. container:: nohighlight
      
               ::

                  bash

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates the header and the source files from the IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm TEE, and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qsc-cli and activate the license
                  qsc-cli login
                  qsc-cli tool activate-license --name sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qsc-cli tool install --name sdllvm_arm --required-version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for the Qualcomm TEE compilation is 16.0.7
                  qsc-cli tool install --name sdllvm_arm --required-version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS8300.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS8300.LE.1.0/common/sectoolsv2/ext/Linux               

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor. Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qsc-cli``, see :ref:`Change the Hexagon tool install path <change_hex_tool_install_path>`.
            .. container:: nohighlight
      
               ::

                  qsc-cli tool extract --name hexagon8.6 --required-version 8.6.05.2
                  qsc-cli tool extract --name hexagon8.7 --required-version 8.7.02.1
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

         .. rubric:: Build DSP      

         **Tools required**

         -  Compiler version: Hexagon 8.6.05.2 and 8.7.02.1
         -  Python version: Python 3.8.2
         
         **Build steps**

         1. Install the device tree compiler:

            .. container:: nohighlight
      
               ::

                  sudo apt-get install device-tree-compiler
                  export DTC_PATH=/usr/bin

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  pip install ruamel.yaml==0.17.17
                  pip install dtschema==2021.10
                  pip install jsonschema==4.0.0

         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/DSP.AT.1.0.1/dsp_proc/build/ms

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py lemans.adsp.prod --clean
                  python ./build_variant.py monaco.cdsp0.prod --clean
                  python ./build_variant.py lemans.gpdsp0.prod --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py lemans.adsp.prod && python ./build_variant.py monaco.cdsp0.prod && python ./build_variant.py lemans.gpdsp0.prod

         .. rubric:: Build Boot

         **Tools required**

         -  Compiler version: LLVM version must be updated to 14.0.4

            .. container:: nohighlight
      
               ::

                  export LLVM=<FIRMWARE_ROOT>/llvm/14.0.4/

         -  Python version: Python 3.10

         -  libffi6 package 
         
         **Build steps**

         1. Install the device tree compiler:

            .. container:: nohighlight
      
               ::

                  sudo apt-get install device-tree-compiler
                  export DTC=/usr/bin

         #. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt
                  pip install json-schema-for-humans

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t monaco,QcomToolsPkg -v LAA -r RELEASE --build_flags=cleanall

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t monaco,QcomToolsPkg -v LAA -r RELEASE

            .. note:: 
               For debug variant builds, replace ``RELEASE`` with ``DEBUG``.

         .. rubric:: Build Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  vi build_config_deploy_monaco.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=monaco --cfg=build_config_deploy_monaco.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/TZ.XF.5.29.1/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=monaco --cfg=build_config_deploy_monaco.xml

         .. rubric:: Build AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Go to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/AOP.HO.3.6.1/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_monaco.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_monaco.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation isn't required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation isn't required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation isn't required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation isn't required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         - Create an integrated firmware binary from the individual components that you compiled:

           .. container:: nohighlight
         
              ::

                 cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS8300.LE.1.0/common/build
                 python build.py --imf

         - Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/QCS8300.LE.1.0/common/build/ufs/bin`` directory:
                        
           -  ``QCS8300_bootbinaries.zip``
           -  ``QCS8300_dspso.zip``
           -  ``QCS8300_fw.zip``


Build a BSP image with extras
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The BSP image build has software components for the Qualcomm device support and software features applicable to the Qualcomm SoCs. This build includes a reference distribution configuration for the Qualcomm development kits. The ``meta-qcom-extras`` layer enables source compilation of select components, which are otherwise present as binary. For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70023-27/qualcomm_linux_metadata_layers.html>`__.

1. Download Qualcomm Yocto and the supporting layers with extras. For the ``<meta-qcom-extras-release-tag>`` information, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. container:: nohighlight
      
      ::

         # NETRC_FILE env variable needs to be set for kas to pickup netrc credentials
         export NETRC_FILE="~/.netrc"
         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>
         git clone https://github.com/qualcomm-linux/meta-qcom-extras-releases -b <meta-qcom-extras-release-tag>
         kas checkout meta-qcom-extras-releases/lock.yml

#. Set up the Yocto build:

   .. container:: nohighlight
      
      ::

         # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
         # It allows source compilation for the corresponding binaries present in meta-qcom.         
         # CUST_ID must be set to "213195" for no-modem based distributions "qualcomm-linux-spf-1-0_ap_standard_oem_nm".         
         # For other modem based distributions, CUST_ID must be set based on the "Customer ID".
         # To find "Customer ID", sign in to your account at qualcomm.com.
         # Click the Profile icon, select Account Settings, and then scroll down to the Company Information section.
         # export CUST_ID using the following command.
         export CUST_ID=<Customer ID>

         # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
         # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
         # using the steps described in the previous section.
         # Example, for QCM6490, the directory path must contain QCM6490_bootbinaries.zip, QCM6490_dspso.zip, and QCM6490_fw.zip. 
         # Set the environment variable to pick up the prebuilts:
         export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm/<product>/common/build/ufs/bin"
         # An example <product> is QCM6490.LE.1.0. For more information about <product>, see the latest Release Notes (https://docs.qualcomm.com/doc/80-70023-300/).

         # Populate meta-qcom-extras kas fragment
         meta-qcom-extras/setup_extras_config.sh 

         # Copy generated meta-qcom-extras kas fragment to meta-qcom
         cp meta-qcom-extras/ci/extras.yml meta-qcom/ci/extras.yml

         # kas configuration files need to be part of the same repository
         # copy the kas lock file to meta-qcom repository
         cp meta-qcom-releases/ci/lock.yml meta-qcom/ci/lock.yml

#. Build the software image. Build targets are defined based on machine and distro combinations:

   .. container:: nohighlight
      
      ::

         kas build meta-qcom/ci/<machine>:meta-qcom/ci/<distro>:meta-qcom/ci/extras.yml:meta-qcom/ci/lock.yml

         # eg. kas build meta-qcom/ci/qcs9100-ride-sx.yml:meta-qcom/ci/qcom-distro.yml:meta-qcom/ci/extras.yml:meta-qcom/ci/lock.yml

   For various ``<machine>`` and ``<distro>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. note::
      To build the images in a fully isolated environment, you can try out `kas-container <https://kas.readthedocs.io/en/latest/userguide/kas-container.html>`__ instead.

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         # meta-qcom uses qcomflash IMAGE_FSTYPE to create a single tarball
         # containing all the relevant files to perform a full clean flash,
         # including partition metadata, boot firmware, ESP # partition and
         # the rootfs.
         cd <workspace-dir>/build/tmp/deploy/images/<MACHINE>/<IMAGE>-<MACHINE>.rootfs-<DATE>.qcomflash/
         ls -al rootfs.img

#. Flash the generated build using :doc:`Flash software images <flash_images>`.

.. note::
   For repo manifest based builds, refer to :ref:`Alternate Build Instructions via Manifest <howto_build>`

Next steps
-----------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`
