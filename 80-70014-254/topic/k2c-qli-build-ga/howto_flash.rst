.. _howto_flash:

Flash
=====

.. _section_tfh_hsh_p1c_vinayjk_03-01-24-1106-28-905:

How to flash Qualcomm configuration data table (CDT) with QDL?
--------------------------------------------------------------

For the Qualcomm hardware, the source of platform information on a
device is the CDT, which is a blob that contains information about the
current platform and its subvariants. The approach to update CDT is by
flashing a CDT binary.

Following are the steps to flash CDT:

1. Download the CDT binary.

   -  For the Core Kit, download the following file:

      ::

         wget https://artifacts.codelinaro.org/artifactory/codelinaro-le/qcom-device-config/CORE_KIT_CDT.bin

   -  For the Vision Kit, download the following file:

      ::

         wget https://artifacts.codelinaro.org/artifactory/codelinaro-le/qcom-device-config/VISION_KIT_CDT.bin

2. Use the CDT binary.

   Copy the downloaded CDT binary to a compiled build location. The
   following is a sample command:

   ::

      cp <the downloaded CDT bin file> <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image/

   **Note:** ``<workspace_path>`` is the directory where you made your
   Yocto build.

3. Update the CDT section in ``rawprogram3.xml`` with the intended CDT
   filename. The following is a sample command:

   ::

      <program SECTOR_SIZE_IN_BYTES="4096" file_sector_offset="0" filename="CORE_KIT_CDT.bin" label="cdt" num_partition_sectors="32" partofsingleimage="false" physical_partition_number="3" readbackverify="false" size_in_KB="128.0" sparse="false" start_byte_hex="0x20000" start_sector="32"/>

Continue with the flashing instructions.


