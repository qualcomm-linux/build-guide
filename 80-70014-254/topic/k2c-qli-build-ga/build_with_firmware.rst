.. _build_private_distribution_with_extras:

Build with firmware sources
===========================

.. _section_q4d_4gq_p1c_vinayjk_03-02-24-1519-18-136:

Sync firmware
-------------

The following table describes the Qualcomm Yocto layers and release
tags:

+--------------------+------------------+------------------+-----------+
| Access level       | Yocto layer      | Release tag      | Example   |
+====================+==================+==================+===========+
| Registered         | `                | manifest release | qcom      |
| developer with any | `meta-qcom-hwe`` | tag              | -6.6.28-Q |
| email address      |                  |                  | LI.1.1-Ve |
|                    |                  |                  | r.1.1.xml |
+--------------------+------------------+------------------+-----------+
| ``meta-qcom        | manifest release | qcom-            |           |
| -qim-product-sdk`` | tag              | 6.6.28-QLI.1.1-V |           |
|                    |                  | er.1.1_qim-produ |           |
|                    |                  | ct-sdk-1.1.3.xml |           |
+--------------------+------------------+------------------+-----------+
| ``meta-q           | manifest release | qcom-6.6         |           |
| com-robotics-sdk`` | tag              | .28-QLI.1.1-Ver. |           |
|                    |                  | 1.1_robotics-pro |           |
|                    |                  | duct-sdk-1.1.xml |           |
+--------------------+------------------+------------------+-----------+
| Registered         | ``me             | meta-qcom-extras | r1.       |
| developer from a   | ta-qcom-extras`` | release tag      | 0_00041.0 |
| verified           |                  |                  |           |
| organization       |                  |                  |           |
+--------------------+------------------+------------------+-----------+
| See `Table         | NA               | firmware release | r1.       |
| 2 <#mapping_       |                  | tag              | 0_00039.2 |
| access_levels>`__. |                  |                  |           |
+--------------------+------------------+------------------+-----------+

The following tables describe the available distributions for firmware
that can be downloaded according to the need and entitlements:

+-------------------------+-------------------------+------------------+
| **Access level**        | **Distribution**        | Yocto layers     |
+=========================+=========================+==================+
| Registered developer    | Base build: High-level  |                  |
| from a verified         | OS and firmware source  |                  |
| organization            | (GPS                    |                  |
|                         | only)\ ``Qu             |                  |
|                         | alcomm_Linux.SPF.1.0|AP |                  |
|                         | |Standard|OEM|NoModem`` |                  |
+-------------------------+-------------------------+------------------+

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

\|Base build + QIMP
SDK\ ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIMPSDK``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

\|Base build + QIMP SDK + QIRP
SDK\ ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIRPSDK``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-robotics-extras``

``meta-ros``

``meta-qcom-robotics``

``meta-qcom-robotics-distro``

``meta-qcom-robotics-sdk``

``meta-qcom-qim-product-sdk``

| 

\|Licensed developer with additional access|Base build: High-level OS
and firmware (GPS only)
source\ ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

\|Base build + QIMP SDK (GPS
only)\ ``Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|QIMPSDK``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

\|Base build: High-level OS and firmware (GPS and modem)
source\ ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

\|Base build + QIMP SDK (GPS and
modem)\ ``Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|QIMPSDK``

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

For Yocto layer descriptions, see `Table
1 <qsc_launcher.rst#Yocto_layer_descriptions>`__.

+----------------+--------+--------------------------------------------+
| Firmware       | Git    | Directory into which firmware gets synced  |
| distribution   | c      | on git clone                               |
|                | ommand |                                            |
+================+========+============================================+
| Qual           | ``gi   | ``qualco                                   |
| comm_Linux.SPF | t clon | mm-linux-spf-1-0_ap_standard_oem_nomodem`` |
| .1.0|AP|Standa | e -b < |                                            |
| rd|OEM|NoModem | firmwa |                                            |
|                | re rel |                                            |
|                | ease t |                                            |
|                | ag> -- |                                            |
|                | depth  |                                            |
|                | 1 http |                                            |
|                | s://qp |                                            |
|                | m-git. |                                            |
|                | qualco |                                            |
|                | mm.com |                                            |
|                | /home2 |                                            |
|                | /git/q |                                            |
|                | ualcom |                                            |
|                | m/qual |                                            |
|                | comm-l |                                            |
|                | inux-s |                                            |
|                | pf-1-0 |                                            |
|                | _ap_st |                                            |
|                | andard |                                            |
|                | _oem_n |                                            |
|                | omodem |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qualcom        | `      | ``qualcomm-                                |
| m_Linux.SPF.1. | `git c | linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` |
| 0|AP|Standard| | lone - |                                            |
| OEM|NM_QIMPSDK | b <fir |                                            |
|                | mware  |                                            |
|                | releas |                                            |
|                | e tag> |                                            |
|                |  --dep |                                            |
|                | th 1 h |                                            |
|                | ttps:/ |                                            |
|                | /qpm-g |                                            |
|                | it.qua |                                            |
|                | lcomm. |                                            |
|                | com/ho |                                            |
|                | me2/gi |                                            |
|                | t/qual |                                            |
|                | comm/q |                                            |
|                | ualcom |                                            |
|                | m-linu |                                            |
|                | x-spf- |                                            |
|                | 1-0_ap |                                            |
|                | _stand |                                            |
|                | ard_oe |                                            |
|                | m_nm-q |                                            |
|                | impsdk |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qualcom        | `      | ``qualcomm-                                |
| m_Linux.SPF.1. | `git c | linux-spf-1-0_ap_standard_oem_nm-qirpsdk`` |
| 0|AP|Standard| | lone - |                                            |
| OEM|NM_QIRPSDK | b <fir |                                            |
|                | mware  |                                            |
|                | releas |                                            |
|                | e tag> |                                            |
|                |  --dep |                                            |
|                | th 1 h |                                            |
|                | ttps:/ |                                            |
|                | /qpm-g |                                            |
|                | it.qua |                                            |
|                | lcomm. |                                            |
|                | com/ho |                                            |
|                | me2/gi |                                            |
|                | t/qual |                                            |
|                | comm/q |                                            |
|                | ualcom |                                            |
|                | m-linu |                                            |
|                | x-spf- |                                            |
|                | 1-0_ap |                                            |
|                | _stand |                                            |
|                | ard_oe |                                            |
|                | m_nm-q |                                            |
|                | irpsdk |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qualcomm_Lin   | ``     | ``qualcomm-linux-spf-1-0_ap_standard_oem`` |
| ux.SPF.1.0|AP| | git cl |                                            |
| Standard|OEM\| | one -b |                                            |
|                |  <firm |                                            |
|                | ware r |                                            |
|                | elease |                                            |
|                |  tag>  |                                            |
|                | --dept |                                            |
|                | h 1 ht |                                            |
|                | tps:// |                                            |
|                | qpm-gi |                                            |
|                | t.qual |                                            |
|                | comm.c |                                            |
|                | om/hom |                                            |
|                | e2/git |                                            |
|                | /qualc |                                            |
|                | omm/qu |                                            |
|                | alcomm |                                            |
|                | -linux |                                            |
|                | -spf-1 |                                            |
|                | -0_ap_ |                                            |
|                | standa |                                            |
|                | rd_oem |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qual           | ``gi   | ``qualco                                   |
| comm_Linux.SPF | t clon | mm-linux-spf-1-0_ap_standard_oem_qimpsdk`` |
| .1.0|AP|Standa | e -b < |                                            |
| rd|OEM|QIMPSDK | firmwa |                                            |
|                | re rel |                                            |
|                | ease t |                                            |
|                | ag> -- |                                            |
|                | depth  |                                            |
|                | 1 http |                                            |
|                | s://qp |                                            |
|                | m-git. |                                            |
|                | qualco |                                            |
|                | mm.com |                                            |
|                | /home2 |                                            |
|                | /git/q |                                            |
|                | ualcom |                                            |
|                | m/qual |                                            |
|                | comm-l |                                            |
|                | inux-s |                                            |
|                | pf-1-0 |                                            |
|                | _ap_st |                                            |
|                | andard |                                            |
|                | _oem_q |                                            |
|                | impsdk |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qualcomm_Linux | ``gi   | ``                                         |
| .SPF.1.0|AMSS| | t clon | qualcomm-linux-spf-1-0_amss_standard_oem`` |
| Standard|OEM\| | e -b < |                                            |
|                | firmwa |                                            |
|                | re rel |                                            |
|                | ease t |                                            |
|                | ag> -- |                                            |
|                | depth  |                                            |
|                | 1 http |                                            |
|                | s://qp |                                            |
|                | m-git. |                                            |
|                | qualco |                                            |
|                | mm.com |                                            |
|                | /home2 |                                            |
|                | /git/q |                                            |
|                | ualcom |                                            |
|                | m/qual |                                            |
|                | comm-l |                                            |
|                | inux-s |                                            |
|                | pf-1-0 |                                            |
|                | _amss_ |                                            |
|                | standa |                                            |
|                | rd_oem |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+
| Qualco         | ``git  | ``qualcomm                                 |
| mm_Linux.SPF.1 | clone  | -linux-spf-1-0_amss_standard_oem_qimpsdk`` |
| .0|AMSS|Standa | -b <fi |                                            |
| rd|OEM|QIMPSDK | rmware |                                            |
|                |  relea |                                            |
|                | se tag |                                            |
|                | > --de |                                            |
|                | pth 1  |                                            |
|                | https: |                                            |
|                | //qpm- |                                            |
|                | git.qu |                                            |
|                | alcomm |                                            |
|                | .com/h |                                            |
|                | ome2/g |                                            |
|                | it/qua |                                            |
|                | lcomm/ |                                            |
|                | qualco |                                            |
|                | mm-lin |                                            |
|                | ux-spf |                                            |
|                | -1-0_a |                                            |
|                | mss_st |                                            |
|                | andard |                                            |
|                | _oem_q |                                            |
|                | impsdk |                                            |
|                | .git`` |                                            |
+----------------+--------+--------------------------------------------+

**Note:** Commands in the following sections are based on binary and
source for firmware images without modem and GPS (see the command in
`distributions for firmware <#distributions_firmware>`__). Hence,
``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk`` is used. If you
use any other distribution, then update the directory accordingly.

The **Git command** column (see `distributions for
firmware <#distributions_firmware>`__) provides the git repository,
which contains the firmware sources. These repositories are hosted on
Qualcomm servers. Clone the appropriate repositories based on your
access profile and use case. The following git clone command downloads
the selected firmware components in source, except the modem:

::

   mkdir -p <FIRMWARE_ROOT>
   cd <FIRMWARE_ROOT>
   git clone -b <firmware release tag> --depth 1 https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk.git
   # Example, <firmware release tag> is r1.0_00039.2

**Note:** The ``git clone`` command clones the content into the
``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk``
directory.

For the latest ``<firmware release tag>``, see the *Build-critical
release tags* section in the `Release
Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

.. _section_v5m_4gq_p1c_vinayjk_03-02-24-1519-24-381:

Build firmware
--------------

-  **Prerequisites**

   -  Ensure that the working shell is ``bash``. Check the output:

      ::

         echo $0

      The expected output of the command should be ``bash``. If not,
      enter the bash shell:

      ::

         bash

   -  Install libffi6 using the following commands. This is required for
      the QAIC compiler, which generates header and source files from
      IDL files:

      ::

         curl -LO http://archive.ubuntu.com/ubuntu/pool/main/libf/libffi/libffi6_3.2.1-8_amd64.deb
         sudo dpkg -i libffi6_3.2.1-8_amd64.deb

   -  Install LLVM for AOP, TZ, and BOOT compilation:

      ::

         cd <FIRMWARE_ROOT>
         mkdir llvm

         # Log in to qpm-cli and activate the license
         qpm-cli --login
         qpm-cli --license-activate sdllvm_arm

         # LLVM requirement for BOOT compilation is 10.0.3
         qpm-cli --install sdllvm_arm --version 10.0.3 --path <FIRMWARE_ROOT>/llvm/10.0.3
         chmod -R 777 <FIRMWARE_ROOT>/llvm/10.0.3

         # LLVM requirement for AOP is 14.0.4 
         qpm-cli --install sdllvm_arm --version 14.0.4 --path <FIRMWARE_ROOT>/llvm/14.0.4
         chmod -R 777 <FIRMWARE_ROOT>/llvm/14.0.4

         # LLVM requirement for TZ compilation is 16.0.7
         qpm-cli --install sdllvm_arm --version 16.0.7 --path <FIRMWARE_ROOT>/llvm/16.0.7
         chmod -R 777 <FIRMWARE_ROOT>/llvm/16.0.7

   -  Export the ``SECTOOLS`` variable and compile the firmware builds
      (``<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk``
      is the top-level directory):

      ::

         export SECTOOLS=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/sectoolsv2/ext/Linux/sectools
         export SECTOOLS_DIR=<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/sectoolsv2/ext/Linux
         # An example <product> is QCM6490.LE.1.0, see the latest [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/).

   -  Install and set up Qualcomm® Hexagon™:

      ::

         qpm-cli --extract hexagon8.4 --version 8.4.07
         export HEXAGON_ROOT=$HOME/Qualcomm/HEXAGON_Tools
         echo $HEXAGON_ROOT

      **Note:** Set the environment variable HEXAGON_ROOT to the path
      where the Hexagon SDK is installed. To change the install path
      when using ``qpm-cli``, see `How can I change the Hexagon tool
      install
      path? <howto_build.rst#section_nqg_cj3_v1c_vinayjk_03-23-24-006-3-877>`__.

-  **Build cDSP**

   **Tools required**

   -  Compiler version – Hexagon 8.4.07
   -  Python version – Python 3.10.2
   -  Install libffi6 **Build steps**

   1. Navigate to the following directory:

      ::

         cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/CDSP.HT.2.5.c3/cdsp_proc/build/ms

   2. Clean the build:

      ::

         python ./build_variant.py kodiak.cdsp.prod --clean

   3. Build the image:

      ::

         python ./build_variant.py kodiak.cdsp.prod

-  **Build aDSP**

   **Tools required**

   -  Compiler version – Hexagon 8.4.07
   -  Python version – Python 3.10.2
   -  Install libffi6 **Nanopb integration (only one-time setup)**

   ::

      cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/qsh_api
      curl https://jpa.kapsi.fi/nanopb/download/nanopb-0.3.9.5-linux-x86.tar.gz -o nanopb-0.3.9.5-linux-x86.tar.gz
      cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/
      python qsh_api/build/config_nanopb_dependency.py -f nanopb-0.3.9.5-linux-x86

   **Build steps**

   1. Navigate to the following directory:

      ::

         cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/ADSP.HT.5.5.c8/adsp_proc/build/ms

   2. Clean the build:

      ::

         python ./build_variant.py kodiak.adsp.prod --clean

   3. Build the image:

      ::

         python ./build_variant.py kodiak.adsp.prod

-  **Build Boot**

   **Tools required**

   -  Compiler version – LLVM version must be updated to 10.0.3

      **Note:** To avoid build errors, ensure that there is a ``/`` at
      the end of the command.

      ::

         export LLVM=<FIRMWARE_ROOT>/llvm/10.0.3/

   -  Python version – Python 3.10

   -  Install libffi6 **Build steps**

   1. For compiling Boot, you need the device tree compiler in the
      /pkg/qct/software/boottools directory. Install the package:

      ::

         sudo apt-get install device-tree-compiler

   2. Copy the /usr/bin/dtc file to the /pkg/qct/software/boottools
      directory:

      ::

         sudo mkdir -p /pkg/qct/software/boottools
         sudo cp -r /usr/bin/dtc /pkg/qct/software/boottools

   3. Navigate to the following directory:

      ::

         cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/BOOT.MXF.1.0.c1/

   4. Install the dependencies:

      ::

         python -m pip install -r boot_images/boot_tools/dtschema_tools/oss/requirements.txt

   5. Clean the build:

      ::

         python -u boot_images/boot_tools/buildex.py -t kodiak,QcomToolsPkg -v LAA -r RELEASE --build_flags=cleanall

   6. Build the image:

      ::

         python -u boot_images/boot_tools/buildex.py -t kodiak,QcomToolsPkg -v LAA -r RELEASE

      **Note:** For debug variant builds, replace ``RELEASE`` with
      ``DEBUG``.

-  **TZ firmware**

   **Tools required**

   -  Compiler version – LLVM 16.0.7
   -  Python version – Python 3.10 **Build steps**

   1. Install LLVM:

      ::

         cd <FIRMWARE_ROOT>/TZ.XF.5.0/trustzone_images/build/ms/
         vi build_config_deploy_kodiak.xml
         # Edit all the occurrences of /pkg/qct/software/llvm/release/arm/16.0.7/ to <FIRMWARE_ROOT>/llvm/16.0.7/

   2. Build the image:

      ::

         cd <FIRMWARE_ROOT>/TZ.XF.5.0/trustzone_images/build/ms/
         python build_all.py -b TZ.XF.5.0 CHIPSET=kodiak --cfg=build_config_deploy_kodiak.xml

-  **AOP firmware**

   **Tools required**

   -  Compiler version – LLVM 14.0.4
   -  Python version – Python 3.10 **Build steps**

   1. Navigate to the following directory:

      ::

         cd <FIRMWARE_ROOT>/AOP.HO.3.6/aop_proc/build/

   2. Build the image:

      ::

         ./build_kodiak.sh -l <FIRMWARE_ROOT>/llvm/14.0.4/

-  **CPUCP firmware**

   The CPUCP firmware is released as a binary and build compilation is
   not required.

-  **CPUSYS.VM firmware**

   The CPUSYS.VM firmware is released as a binary and build compilation
   is not required.

-  **BTFM firmware**

   The BTFM firmware is released as a binary and build compilation is
   not required.

-  **WLAN firmware**

   The WLAN firmware is released as a binary and build compilation is
   not required.

-  **Generate firmware prebuilds (boot-critical and split-firmware
   binaries)**

   Create an integrated firmware binary from the individual components
   that you compiled:

   ::

      # cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/build
      # An example <product> is QCM6490.LE.1.0, see the latest [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/)
      cd <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/build
      python build.py --imf

   **Note:** Firmware prebuild is successful if the following zip files
   are generated in the
   <FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/QCM6490.LE.1.0/common/build/ufs/bin
   directory:

   -  QCM6490_bootbinaries.zip
   -  QCM6490_dspso.zip
   -  QCM6490_fw.zip

.. _section_unn_4gq_p1c_vinayjk_03-02-24-1519-24-874:

Build base image with extras
----------------------------

1. Download Qualcomm Yocto and supporting layers with extras:

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.28-QLI.1.1-Ver.1.1.xml
      repo sync
      git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
      # Example, <meta-qcom-extras release tag> is r1.0_00041.0
      mkdir -p layers/meta-qcom-extras
      cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/QCM6490.LE.1.0/common/config/meta-qcom-extras/* layers/meta-qcom-extras/

   **Note:** For the ``<manifest release tag>`` and
   ``<meta-qcom-extras release tag>`` information, see the
   *Build-critical release tags* section in the `Release
   Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

2. Set up the Yocto build:

   ::

      # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
      export EXTRALAYERS="meta-qcom-extras"

      # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
      # It enables source compilation for the corresponding binaries present in meta-qcom-hwe.
      # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
      # CUST_ID must be initialized to <PARTY_ID> for "Licensed customers with additional access".
      # For example, for distros like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
      # <PARTY_ID> is provided while signing up for distros mapping to "Licensed customers with additional access".
      # To find <PARTY_ID>, sign in to your account at qualcomm.com.
      # Click the profile icon, select Account Settings, and then scroll to Company Information section.
      # Use the number specified for Export ID as <PARTY_ID>.
      export CUST_ID="213195"

      # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
      # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
      # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
      # QCM6490_dspso.zip, and QCM6490_fw.zip. 
      # Set the environment variable to pick up the prebuilts:
      export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/build/ufs/bin"
      # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/).

      MACHINE=qcm6490 DISTRO=qcom-wayland source setup-environment
      # source setup-environment: Sets the environment settings, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory.

3. Compile the Yocto build:

   ::

      bitbake qcom-multimedia-image

   **Note:** Clean the Yocto build:

   ::

      bitbake -fc cleansstate qcom-multimedia-image
      bitbake -fc cleanall qcom-multimedia-image

   On successful build, you can check if ``system.img`` is present in
   the
   ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image``
   directory:

   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image
      ls -al system.img

4. Flash the generated build using `Flash images for registered
   users <flash_images.rst>`__.

.. _section_cx2_dqf_s1c_vinayjk_03-11-24-2139-47-648:

Build QIMP SDK image with extras
--------------------------------

1. Download QIMP SDK layers, Qualcomm Yocto, and supporting layers with
   extras:

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.28-QLI.1.1-Ver.1.1.xml
      repo sync
      git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
      # Example, <meta-qcom-extras release tag> is r1.0_00041.0
      mkdir -p layers/meta-qcom-extras
      cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/QCM6490.LE.1.0/common/config/meta-qcom-extras/* layers/meta-qcom-extras/
      git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b qcom-6.6.28-QLI.1.1-Ver.1.1_qim-product-sdk-1.1.3 layers/meta-qcom-qim-product-sdk

   **Note:** For the ``<manifest release tag>`` and
   ``<meta-qcom-extras release tag>`` information, see the
   *Build-critical release tags* section in the `Release
   Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

2. Set up the Yocto build:

   ::

      # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
      export EXTRALAYERS="meta-qcom-extras meta-qcom-qim-product-sdk"

      # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
      # It enables source compilation for the corresponding binaries present in meta-qcom-hwe.
      # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
      # CUST_ID must be initialized to <PARTY_ID> for "Licensed customers with additional access".
      # For example, for distros like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
      # <PARTY_ID> is provided while signing up for distros mapping to "Licensed customers with additional access".
      # To find <PARTY_ID>, sign in to your account at qualcomm.com.
      # Click the profile icon, select Account Settings, and then scroll to Company Information section.
      # Use the number specified for Export ID as <PARTY_ID>.
      export CUST_ID="213195"

      # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
      # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
      # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
      # QCM6490_dspso.zip, and QCM6490_fw.zip. 
      # Set the environment variable to pick up the prebuilts:
      export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qimpsdk/<product>/common/build/ufs/bin"
      # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/).

      MACHINE=qcm6490 DISTRO=qcom-wayland source setup-environment
      # source setup-environment: Sets the environment settings, creates the build directory build-qcom-wayland,
      # and enters into build-qcom-wayland directory.

3. Compile the QIMP SDK build:

   ::

      bitbake qcom-multimedia-image
      bitbake qim-product-sdk

   **Note:** Clean the QIMP SDK build:

   ::

      bitbake -fc cleansstate qcom-multimedia-image
      bitbake -fc cleanall qcom-multimedia-image

      bitbake -fc cleansstate qim-product-sdk
      bitbake -fc cleanall qim-product-sdk

   On successful build, you can check if ``system.img`` is present in
   the
   ``<workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image``
   directory:

   ::

      cd <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image
      ls -al system.img

4. Flash the generated build using `Flash images for registered
   users <flash_images.rst>`__.

.. _section_kjz_d1j_5bc_vinayjk_06-20-24-1130-57-104:

Build QIRP SDK image with extras
--------------------------------

**Note:** Ensure that you have cloned the respective firmware for QIRP
SDK. For example, ``qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk``.

1. Download QIRP SDK layers, Qualcomm Yocto, and supporting layers with
   extras:

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m <manifest release tag>
      # Example, <manifest release tag> is qcom-6.6.28-QLI.1.1-Ver.1.1.xml
      repo sync
      git clone https://qpm-git.qualcomm.com/home2/git/qualcomm/qualcomm-linux-spf-1-0_hlos_oem_metadata.git -b <meta-qcom-extras release tag> --depth 1
      # Example, <meta-qcom-extras release tag> is r1.0_00041.0
      mkdir -p layers/meta-qcom-extras
      mkdir -p layers/meta-qcom-robotics-extras
      cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/QCM6490.LE.1.0/common/config/meta-qcom-extras/* layers/meta-qcom-extras/
      cp -rf qualcomm-linux-spf-1-0_hlos_oem_metadata/QCM6490.LE.1.0/common/config/meta-qcom-robotics-extras/* layers/meta-qcom-robotics-extras/

      git clone https://git.codelinaro.org/clo/le/meta-ros.git -b ros.qclinux.1.0.r1-rel layers/meta-ros
      git clone https://github.com/quic-yocto/meta-qcom-robotics.git layers/meta-qcom-robotics
      git clone https://github.com/quic-yocto/meta-qcom-robotics-distro.git layers/meta-qcom-robotics-distro
      git clone https://github.com/quic-yocto/meta-qcom-robotics-sdk.git layers/meta-qcom-robotics-sdk
      git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
      # Example, <qim-product-sdk release tag> is qcom-6.6.28-QLI.1.1-Ver.1.1_qim-product-sdk-1.1.3

   **Note:** For the ``<manifest release tag>``,
   ``<meta-qcom-extras release tag>``, and
   ``<qim-product-sdk release tag>`` information, see the
   *Build-critical release tags* section in the `Release
   Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

2. Set up the Yocto build:

   ::

      # Export additional meta layers to EXTRALAYERS. Location is assumed under <WORKSPACE DIR>/layers.
      export EXTRALAYERS="meta-qcom-extras meta-qcom-robotics-extras"

      # CUST_ID is used to clone the proprietary source repositories downloaded by meta-qcom-extras.
      # It enables source compilation for the corresponding binaries present in meta-qcom-hwe.
      # This ID is constant for the firmware repository qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk.git.
      # CUST_ID must be initialized to <PARTY_ID> for "Licensed customers with additional access".
      # For example, for distros like "Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|" and "Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|",
      # <PARTY_ID> is provided while signing up for distros mapping to "Licensed customers with additional access".
      # To find <PARTY_ID>, sign in to your account at qualcomm.com.
      # Click the profile icon, select Account Settings, and then scroll to Company Information section.
      # Use the number specified for Export ID as <PARTY_ID>.
      export CUST_ID="213195"

      # The firmware recipe is compiled when the Yocto build is initiated. Firmware recipe expects the
      # path of firmware. You have generated firmware prebuilts (boot-critical and split-firmware binaries)
      # using the steps described in the previous section. The directory path must contain QCM6490_bootbinaries.zip,
      # QCM6490_dspso.zip, and QCM6490_fw.zip.
      # Set the environment variable to pick up the prebuilts:
      export FWZIP_PATH="<FIRMWARE_ROOT>/qualcomm-linux-spf-1-0_ap_standard_oem_nm-qirpsdk/<product>/common/build/ufs/bin"
      # An example <product> is QCM6490.LE.1.0. For more information on <product>, see the latest [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/).

3. Compile the QIRP SDK build:

   ::

      ln -s layers/meta-qcom-robotics-distro/set_bb_env.sh ./setup-robotics-environment
      ln -s layers/meta-qcom-robotics-sdk/scripts/qirp-build ./qirp-build
      MACHINE=qcm6490 DISTRO=qcom-robotics-ros2-humble source setup-robotics-environment
      # source setup-robotics-environment: Sets the environment settings, creates the build directory build-qcom-robotics-ros2-humble,
      # and enters into build-qcom-robotics-ros2-humble directory.
      ../qirp-build qcom-robotics-full-image

   On a successful build, you can see the QIRP SDK build artifacts at
   the following paths:

   ::

      QIRP SDK artifacts: <workspace_path>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/qirpsdk_artifacts/qirp-sdk_<version>.tar.gz
      # system.img is present in the following path
      Robotics image: <workspace_path>/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/images/qcm6490/qcom-robotics-full-image

4. Flash the generated build using `Flash images for registered
   users <flash_images.rst>`__.

