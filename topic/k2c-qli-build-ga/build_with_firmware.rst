.. _build_private_distribution_with_extras:

Build with firmware sources
------------------------------------

.. _section_q4d_4gq_p1c_vinayjk_03-02-24-1519-18-136:

Sync firmware
^^^^^^^^^^^^^^

The following table describes the Qualcomm Yocto layers and release tags:

.. tabularcolumns:: |p{3cm}|p{4cm}|p{3cm}|p{4cm}|

.. flat-table:: Qualcomm Yocto layers and manifest tags
   :header-rows: 1
   :class: longtable

   * - Access level
     - Yocto layer
     - Release tag
     - Example
	 
   * - :rspan:`2` Public developers (unregistered)
     - ``meta-qcom-hwe``
     - manifest release tag
     - qcom-6.6.52-QLI.1.3-Ver.1.1.xml
   *  
     - ``meta-qcom-qim-product-sdk``
     - manifest release tag
     - qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2.xml
   *  
     - ``meta-qcom-robotics-sdk``
     - manifest release tag
     - qcom-6.6.52-QLI.1.3-Ver.1.1_robotics-product-sdk-1.1.xml
   * - Licensed developers with authorized access
     - ``meta-qcom-extras``
     - meta-qcom-extras release tag
     - r1.0_00055.0 
   * - See :ref:`Mapping access levels to firmware distributions <build_mapping_access_levels>`
     - NA
     - firmware release tag
     - r1.0_00054.0

The following tables describe the firmware distributions that can be downloaded according to the need and entitlements:

.. _build_mapping_access_levels:

.. flat-table:: Mapping access levels to firmware distributions
   :widths: 24 24 24
   :header-rows: 1
   :class: longtable table-wrap

   * - **Access level**
     - **Distribution**
     - Yocto layers
   * - :rspan:`2` Licensed developers with authorized access
     - BSP build: High-level OS and firmware source (GPS only)
       
       ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem``
     - 
       ``meta-qcom``
       
       ``meta-qcom-hwe``

       ``meta-qcom-distro``
       
       ``meta-qcom-extras``
   *  
     - BSP build + QIMP SDK
      
        ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIMPSDK``
     - ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``

       ``meta-qcom-qim-product-sdk``  
   *  
     - BSP build + QIMP SDK + QIRP SDK
      
        ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIRPSDK``
     - ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``

       ``meta-qcom-robotics-extras``

       ``meta-ros``   

       ``meta-qcom-robotics``

       ``meta-qcom-robotics-distro``

       ``meta-qcom-robotics-sdk``

       ``meta-qcom-qim-product-sdk``
   * - :rspan:`3` Licensed developers (contact Qualcomm for access)
     - BSP build: High-level OS and firmware (GPS only) source
       
       ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|``
     - 
       ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``
   *  
     - BSP build + QIMP SDK (GPS only)
      
        ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|QIMPSDK``
     - ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``

       ``meta-qcom-robotics-extras``

       ``meta-qcom-qim-product-sdk``
   *  
     - BSP build: High-level OS and firmware (GPS and modem) source
      
        ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|``
     - ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``
   *  
     - BSP build + QIMP SDK (GPS and modem)
      
        ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|QIMPSDK``
     - ``meta-qcom``

       ``meta-qcom-hwe``

       ``meta-qcom-distro``

       ``meta-qcom-extras``

       ``meta-qcom-qim-product-sdk``

.. note:: For more information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-27/platform_software_features.html#qualcomm-linux-metadata-layers-overview>`__.

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

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIMPSDK
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk``

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIRPSDK
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk``

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM\|
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem``

   * - Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|QIMPSDK
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_qimpsdk.git``
     - ``qualcomm-linux-spf-1-0_ap_standard_oem_qimpsdk``

   * - Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM\|
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_amss_standard_oem.git``
     - ``qualcomm-linux-spf-1-0_amss_standard_oem``

   * - Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|QIMPSDK
     - ``git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_amss_standard_oem_qimpsdk.git``
     - ``qualcomm-linux-spf-1-0_amss_standard_oem_qimpsdk``

.. note:: Commands in the following sections are based on the binary and source for firmware images without modem and GPS (see the command in :ref:`Mapping firmware distributions to git repositories <Mapping_firmware_table>`). Hence, ``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` is used. If you use any other distribution, then update the directory accordingly.

The **Git command** column (see :ref:`Mapping firmware distributions to git repositories <Mapping_firmware_table>`) provides information about the git repositories that contain the firmware sources. These repositories are hosted on Qualcomm servers. Clone the appropriate repositories based on your access profile and use case.

The following ``git clone`` command downloads the selected firmware components in source, except the modem:

.. container:: nohighlight
      
   ::

      mkdir -p <FIRMWARE_ROOT>
      cd <FIRMWARE_ROOT>
      git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk.git
      # Example, <firmware release tag> is r1.0_00054.0

.. note:: 
   - The ``git clone`` command clones the content into the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` directory.
   - For the latest ``<firmware release tag>``, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

.. _section_v5m_4gq_p1c_vinayjk_03-02-24-1519-24-381:

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

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates header and source files from IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm\ :sup:`®` Trusted Execution Environment (TEE), and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qpm-cli and activate the license
                  qpm-cli --login
                  qpm-cli --license-activate sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qpm-cli --install sdllvm_arm --version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for Qualcomm TEE compilation is 16.0.7
                  qpm-cli --install sdllvm_arm --version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/sectoolsv2/ext/Linux

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor:

            .. container:: nohighlight
      
               ::

                  qpm-cli --extract hexagon8.4 --version 8.4.07
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

            .. note:: Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qpm-cli``, see :ref:`Change the Hexagon tool install path <section_nqg_cj3_v1c_vinayjk_03-23-24-006-3-877>`.

         .. rubric:: Build cDSP 

         **Tools required**

         -  Compiler version: Hexagon 8.4.07
         -  Python version: Python 3.10.2
         -  libffi6 package 
         
         **Build steps**

         1. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/CDSP.HT.2.5.c3/cdsp_proc/build/ms

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
         
         **Nanopb integration (one-time setup)**

         .. container:: nohighlight
      
            ::

               cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/qsh_api
               curl https://jpa.kapsi.fi/nanopb/download/nanopb-0.3.9.5-linux-x86.tar.gz -o nanopb-0.3.9.5-linux-x86.tar.gz
               cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/
               python qsh_api/build/config_nanopb_dependency.py -f nanopb-0.3.9.5-linux-x86

         **Build steps**

         1. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/build/ms

         2. Clean the build:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.adsp.prod --clean

         3. Build the image:

            .. container:: nohighlight
      
               ::

                  python ./build_variant.py kodiak.adsp.prod

         .. rubric:: Build Boot

         **Tools required**

         -  Compiler version: LLVM version must be updated to 14.0.4

            .. note:: 
               To avoid build errors, ensure that there is a ``/`` at the end of the command.

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

         #. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt

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

         .. rubric:: Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  vi build_config_deploy_kodiak.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=kodiak --cfg=build_config_deploy_kodiak.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=kodiak --cfg=build_config_deploy_kodiak.xml

         .. rubric:: AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/AOP.HO.3.6/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_kodiak.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_kodiak.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation is not required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation is not required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation is not required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation is not required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         Create an integrated firmware binary from the individual components that you compiled:

         .. container:: nohighlight
      
            ::

               cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/build
               python build.py --imf

         .. note:: 
            Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/build/ufs/bin`` directory:

            -  ``QCM6490_bootbinaries.zip``
            -  ``QCM6490_dspso.zip``
            -  ``QCM6490_fw.zip``

      .. group-tab:: QCS9075

         .. rubric:: Prerequisites

         -  Ensure that the working shell is ``bash``.

            .. container:: nohighlight
      
               ::

                  echo $0

            The expected output of the command should be ``bash``. If not, enter the bash shell:

            .. container:: nohighlight
      
               ::

                  bash

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates header and source files from IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm TEE, and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qpm-cli and activate the license
                  qpm-cli --login
                  qpm-cli --license-activate sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qpm-cli --install sdllvm_arm --version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for Qualcomm TEE compilation is 16.0.7
                  qpm-cli --install sdllvm_arm --version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS9100.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS9100.LE.1.0/common/sectoolsv2/ext/Linux

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor:

            .. container:: nohighlight
      
               ::

                  qpm-cli --extract hexagon8.6 --version 8.6.05.2
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

            .. note:: Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qpm-cli``, see :ref:`Change the Hexagon tool install path? <section_nqg_cj3_v1c_vinayjk_03-23-24-006-3-877>`.

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

         #. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/DSP.AT.1.0/dsp_proc/build/ms

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

         #. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t lemans,QcomToolsPkg - v LAA -r RELEASE --build_flags=cleanall

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t lemans,QcomToolsPkg - v LAA -r RELEASE

            .. note:: 
               For debug variant builds, replace ``RELEASE`` with ``DEBUG``.

         .. rubric:: Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  vi build_config_deploy_lemans.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=lemans --cfg=build_config_deploy_lemans.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=lemans --cfg=build_config_deploy_lemans.xml

         .. rubric:: AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/AOP.HO.3.6.1/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_lemansau.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_lemansau.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation is not required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation is not required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation is not required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation is not required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         Create an integrated firmware binary from the individual components that you compiled:

         .. container:: nohighlight
      
            ::

               cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS9100.LE.1.0/common/build
               python build.py --imf

         .. note:: 
            Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS9100.LE.1.0/common/build/ufs/bin`` directory:
                     
            -  ``QCS9100_bootbinaries.zip``
            -  ``QCS9100_dspso.zip``
            -  ``QCS9100_fw.zip``

      .. group-tab:: QCS8275

         .. rubric:: Prerequisites

         -  Ensure that the working shell is ``bash``.

            .. container:: nohighlight
      
               ::

                  echo $0

            The expected output of the command should be ``bash``. If not, enter the bash shell:

            .. container:: nohighlight
      
               ::

                  bash

         -  Install the libffi6 package using the following commands. This is required for the QAIC compiler, which generates header and source files from IDL files:

            .. container:: nohighlight
      
               ::

                  curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
                  sudo dpkg -i libffi6_3.2.1-8_amd64.deb

         -  Install LLVM for AOP, Qualcomm TEE, and boot compilation:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>
                  mkdir llvm

                  # Log in to qpm-cli and activate the license
                  qpm-cli --login
                  qpm-cli --license-activate sdllvm_arm

                  # LLVM requirement for boot compilation is 14.0.4
                  qpm-cli --install sdllvm_arm --version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

                  # LLVM requirement for Qualcomm TEE compilation is 16.0.7
                  qpm-cli --install sdllvm_arm --version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
                  chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

         -  Export the ``SECTOOLS`` variable and compile the firmware builds (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` is the top-level directory):

            .. container:: nohighlight
      
               ::

                  export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS8300.LE.1.0/common/sectoolsv2/ext/Linux/sectools
                  export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS8300.LE.1.0/common/sectoolsv2/ext/Linux               

         -  Install and set up Qualcomm\ :sup:`®` Hexagon\ :sup:`™` Processor:

            .. container:: nohighlight
      
               ::

                  qpm-cli --extract hexagon8.6 --version 8.6.05.2
                  qpm-cli --extract hexagon8.7 --version 8.7.02.1
                  export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
                  echo $HEXAGON_ROOT

            .. note:: Set the environment variable HEXAGON_ROOT to the path where the Hexagon SDK is installed. To change the install path when using ``qpm-cli``, see :ref:`Change the Hexagon tool install path? <section_nqg_cj3_v1c_vinayjk_03-23-24-006-3-877>`.

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

         #. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/DSP.AT.1.0/dsp_proc/build/ms

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

         #. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/BOOT.MXF.1.0.c1/

         #. Install the dependencies:

            .. container:: nohighlight
      
               ::

                  python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t monaco,QcomToolsPkg - v LAA -r RELEASE --build_flags=cleanall

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  python -u boot_images/boot_tools/buildex.py -t monaco,QcomToolsPkg - v LAA -r RELEASE

            .. note:: 
               For debug variant builds, replace ``RELEASE`` with ``DEBUG``.

         .. rubric:: Qualcomm TEE firmware

         **Tools required**

         -  Compiler version: LLVM 16.0.7
         -  Python version: Python 3.10 
         
         **Build steps**

         1. Install LLVM:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  vi build_config_deploy_monaco.xml
                  # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  python build_all.py -b TZ.XF.5.0 CHIPSET=monaco --cfg=build_config_deploy_monaco.xml --clean

         #. Build the image:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/TZ.XF.5.29/trustzone_images/build/ms/
                  python build_all.py -b TZ.XF.5.0 CHIPSET=monaco --cfg=build_config_deploy_monaco.xml

         .. rubric:: AOP firmware

         **Tools required**

         -  Compiler version: LLVM 14.0.4
         -  Python version: Python 3.10 
            
         **Build steps**

         1. Navigate to the following directory:

            .. container:: nohighlight
      
               ::

                  cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/AOP.HO.3.6.1/aop_proc/build/

         #. Clean the build:

            .. container:: nohighlight
      
               ::

                  ./build_monaco.sh -c -l <FIRMWARE_ROOT>/llvm/14.0.4/
         
         #. Build the image:

            .. container:: nohighlight
      
               ::

                  ./build_monaco.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

         .. rubric:: CPUCP firmware

         The CPUCP firmware is released as a binary and build compilation is not required.

         .. rubric:: CPUSYS.VM firmware

         The CPUSYS.VM firmware is released as a binary and build compilation is not required.

         .. rubric:: BTFM firmware

         The BTFM firmware is released as a binary and build compilation is not required.

         .. rubric:: WLAN firmware

         The WLAN firmware is released as a binary and build compilation is not required.

         .. rubric:: Generate firmware prebuilds (boot-critical and split-firmware binaries)

         Create an integrated firmware binary from the individual components that you compiled:

         .. container:: nohighlight
      
            ::

               cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS8300.LE.1.0/common/build
               python build.py --imf

         .. note:: 
            Firmware prebuild is successful if the following zip files are generated in the ``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCS8300.LE.1.0/common/build/ufs/bin`` directory:
                     
            -  ``QCS8300_bootbinaries.zip``
            -  ``QCS8300_dspso.zip``
            -  ``QCS8300_fw.zip``

.. _section_unn_4gq_p1c_vinayjk_03-02-24-1519-24-874:

Build BSP image with extras
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
BSP image build contains the software components for Qualcomm device support and value-added software features applicable to Qualcomm SoCs. It includes a reference distribution configuration for Qualcomm development kits. The ``meta-qcom-extras`` layer enables source compilation of select components, which are otherwise present as binary.

For more details, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-27/platform_software_features.html#qualcomm-linux-metadata-layers-overview>`__.

1. Download Qualcomm Yocto and the supporting layers with extras:

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1.xml
         repo sync
         git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
         # Example, <meta-qcom-extras release tag> is r1.0_00055.0
         mkdir -p layers/meta-qcom-extras
         cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/<product>/common/config/meta-qcom-extras/* layers/meta-qcom-extras/
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).

   .. note:: For the ``<manifest release tag>`` and ``<meta-qcom-extras release tag>`` information, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

#. Set up the Yocto build:

   .. container:: nohighlight
      
      ::

         # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
         export EXTRALAYERS="meta-qcom-extras"

         # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
         # It allows source compilation for the corresponding binaries present in meta-qcom-hwe.
         # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
         # CUST_ID must be initialized to <PARTY_ID> for "Licensed developers (contact Qualcomm for access)".
         # For example, for distributions like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
         # <PARTY_ID> is provided while signing up for distributions mapping to "Licensed developers (contact Qualcomm for access)".
         # To find <PARTY_ID>, sign in to your account at qualcomm.com.
         # Click the profile icon, select Account Settings, and then scroll down to the Company Information section.
         # Use the number specified for Export ID as <PARTY_ID>.
         export CUST_ID="213195"

         # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
         # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
         # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
         # QCM6490_dspso.zip, and QCM6490_fw.zip. 
         # Set the environment variable to pick up the prebuilts:
         export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/build/ufs/bin"
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   .. note::
      To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

#. Compile the Yocto build:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image

   .. note::
      Clean the Yocto build:

      .. container:: nohighlight
      
         ::

            bitbake -fc cleansstate qcom-multimedia-image
            bitbake -fc cleanall qcom-multimedia-image

   After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

#. Flash the generated build using :doc:`Flash images <flash_images>`.

.. _section_cx2_dqf_s1c_vinayjk_03-11-24-2139-47-648:

Build QIMP SDK image with extras
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The QIMP SDK is a collection of four standalone function SDKs, namely, IM SDK, Qualcomm® Neural Processing SDK, Qualcomm® AI Engine direct SDK, and the LiteRT. It also includes reference applications that you can use to develop use cases. The ``meta-qcom-extras`` layer enables source compilation of select components, which are otherwise present as binary.

For more details, see `QIMP SDK Quick Start Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-51>`__.

1. Download the QIMP SDK layers, Qualcomm Yocto layer, and the supporting layers with
   extras:

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1.xml
         repo sync
         git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
         # Example, <meta-qcom-extras release tag> is r1.0_0005.0
         mkdir -p layers/meta-qcom-extras
         cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/<product>/common/config/meta-qcom-extras/* layers/meta-qcom-extras/
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).
         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2 layers/meta-qcom-qim-product-sdk

   .. note:: For the ``<manifest release tag>`` and
            ``<meta-qcom-extras release tag>`` information, see the section *Build-critical release tags* in the `Release
            Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

#. Set up the Yocto build:

   .. container:: nohighlight
      
      ::

         # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
         export EXTRALAYERS="meta-qcom-extras meta-qcom-qim-product-sdk"

         # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
         # It allows source compilation for the corresponding binaries present in meta-qcom-hwe.
         # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
         # CUST_ID must be initialized to <PARTY_ID> for "Licensed developers (contact Qualcomm for access)".
         # For example, for distributions like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
         # <PARTY_ID> is provided while signing up for distributions mapping to "Licensed developers (contact Qualcomm for access)".
         # To find <PARTY_ID>, sign in to your account at qualcomm.com.
         # Click the profile icon, select Account Settings, and then scroll down to the Company Information section.
         # Use the number specified for Export ID as <PARTY_ID>.
         export CUST_ID="213195"

         # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
         # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
         # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
         # QCM6490_dspso.zip, and QCM6490_fw.zip. 
         # Set the environment variable to pick up the prebuilts:
         export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/build/ufs/bin"
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   .. note::
      To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

#. Compile the QIMP SDK build:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image
         # Build SDK image
         bitbake qcom-qim-product-sdk

   .. note:: 
      Clean the QIMP SDK build:
      
      .. container:: nohighlight
      
         ::

            bitbake -fc cleansstate qcom-multimedia-image
            bitbake -fc cleanall qcom-multimedia-image

            bitbake -fc cleansstate qcom-qim-product-sdk
            bitbake -fc cleanall qcom-qim-product-sdk

   After a successful build, check that the ``system.img`` file is in the ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image`` directory:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image
         ls -al system.img

#. Flash the generated build using :doc:`Flash images <flash_images>`.

.. _section_kjz_d1j_5bc_vinayjk_06-20-24-1130-57-104:

Build QIRP SDK image with extras
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The QIRP SDK 2.0 is a collection of components that enable you to develop robotic features on Qualcomm Linux releases. The ``meta-qcom-extras`` layer enables source compilation of select components, which are otherwise available as binaries.

For more details, see `QIRP SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70017-265>`__.

.. note:: Before you begin, clone the respective firmware for QIRP SDK, for example, ``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk``.

1. Download QIRP SDK layers, Qualcomm Yocto, and supporting layers with
   extras:

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <WORKSPACE_DIR>
         cd <WORKSPACE_DIR>
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
         # Example, <manifest release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1.xml
         repo sync
         git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
         # Example, <meta-qcom-extras release tag> is r1.0_00055.0
         mkdir -p layers/meta-qcom-extras
         mkdir -p layers/meta-qcom-robotics-extras
         cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/<product>/common/config/meta-qcom-extras/* layers/meta-qcom-extras/
         cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/<product>/common/config/meta-qcom-robotics-extras/* layers/meta-qcom-robotics-extras/
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).

         git clone https://github.com/ros/meta-ros -b kirkstone layers/meta-ros
         git clone https://github.com/quic-yocto/meta-qcom-robotics.git layers/meta-qcom-robotics
         git clone https://github.com/quic-yocto/meta-qcom-robotics-distro.git layers/meta-qcom-robotics-distro
         git clone https://github.com/quic-yocto/meta-qcom-robotics-sdk.git layers/meta-qcom-robotics-sdk
         git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
         # Example, <meta-qcom-qim-product-sdk release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2

   .. note:: 
       For the ``<manifest release tag>``, ``<meta-qcom-extras release tag>``, and ``<meta-qcom-qim-product-sdk release tag>`` information, see the section *Build-critical release tags* in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

#. Set up the Yocto build:

   .. container:: nohighlight
      
      ::

         # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
         export EXTRALAYERS="meta-qcom-extras meta-qcom-robotics-extras"

         # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
         # It allows source compilation for the corresponding binaries present in meta-qcom-hwe.
         # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
         # CUST_ID must be initialized to <PARTY_ID> for "Licensed developers (contact Qualcomm for access)".
         # For example, for distributions like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
         # <PARTY_ID> is provided while signing up for distributions mapping to "Licensed developers (contact Qualcomm for access)".
         # To find <PARTY_ID>, sign in to your account at qualcomm.com.
         # Click the profile icon, select Account Settings, and then scroll down to the Company Information section.
         # Use the number specified for Export ID as <PARTY_ID>.
         export CUST_ID="213195"

         # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
         # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
         # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
         # QCM6490_dspso.zip, and QCM6490_fw.zip.
         # Set the environment variable to pick up the prebuilts:
         export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk/<product>/common/build/ufs/bin"
         # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest Release Notes (https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/).

#. Compile the QIRP SDK build:

   .. container:: nohighlight
      
      ::

         ln -s layers/meta-qcom-robotics-distro/set_bb_env.sh ./setup-robotics-environment
         ln -s layers/meta-qcom-robotics-sdk/scripts/qirp-build ./qirp-build
         MACHINE=<machine> DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=custom source setup-robotics-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-robotics-ros2-humble QCOM_SELECTED_BSP=custom source setup-robotics-environment
         # source setup-robotics-environment: Sets the environment, creates the build directory build-qcom-robotics-ros2-humble,
         # and enters into build-qcom-robotics-ros2-humble directory.
         ../qirp-build qcom-robotics-full-image

   .. note::
      To know the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241208101813/>`__.

   After a successful build, check that the QIRP SDK build artifacts are at the following paths:

   .. container:: nohighlight
      
      ::

         QIRP SDK artifacts: <workspace_path>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/qirpsdk_artifacts/qirp-sdk_<version>.tar.gz
         # system.img is present in the following path
         Robotics image: <workspace_path>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-robotics-full-image

#. Flash the generated build using :doc:`Flash images <flash_images>`.
