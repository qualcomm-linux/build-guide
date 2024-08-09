.. _sync_build_flash:

Use QSC CLI
===========

This section provides instructions on how to download, compile, and
recompile Qualcomm Linux using QSC CLI.

.. _section_ez3_31x_v1c:

Download
--------

**Note:**

If you are building a distribution with access level “Registered
developer from a verified organization” or “Licensed customers with
additional access”, then you must log in to ``qpm-cli`` before you
compile:

::

   # Run the following command to login
   qpm-cli --login
   # Run the following command to verify if qpm-cli login is successful
   qpm-cli --product-list

This command prompts for your username and password. After successfully
logging in, the system fetches and refreshes the product list.

-  Download a particular software release by specifying the product ID,
   build ID, distribution, and the absolute/full workspace path as shown
   in the following example:

   ::

      # qsc-cli download --workspace-path '<absolute_workspace_path>' --product '<Product_ID>' --build '<Build_ID>' --distribution '<Distro>'

   -  Select Product_ID and Build_ID values from the **QSC-CLI Input
      Parameters** table in the `Release
      Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

      Example: For QCM6490 with Linux embedded (LE) build, the input
      parameters are shown in the following table:

      \|\ **Product_ID (–product)**\ \|
      ``{#codeblock_ezc_hlg_z1c_vinayjk_04-06-24-101-3-582} QCM6490.LE.1.0``

| 

::

       |**Build\_ID \(--build\)**|        ``` {#codeblock_vhk_vpt_vbc_vinayjk_06-25-24-1219-7-466}

QCM6490.LE.1.0-00218-STD.PROD-1 \``\`

| 

::

   -   Select the appropriate distribution to download. Distribution access is controlled by access levels as listed in the following table:

       **Note:** For more details on the available distributions, see **Access Controlled Distribution** table in the [Release Notes](https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/).

       |**Access level**|**Distribution**|Yocto layers|
       |----------------|----------------|------------|
       |Unregistered/community developer|Base build: High-level operating system \(OS\) and prebuilt firmware \(GPS only\)`Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PUBLIC`

\|\ ``meta-qcom``

``meta-qcom-hwe``

| 

::

       |Base build + Qualcomm Intelligent Multimedia Product \(QIMP\) SDK`Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-qim-product-sdk``

| 

::

       |Base build + QIMP SDK + Qualcomm Intelligent Robotics Product \(QIRP\) SDK`Qualcomm_Linux.SPF.1.0|TEST|DEVICE|RoboApiLnx`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-ros``

``meta-qcom-robotics``

``meta-qcom-robotics-distro``

``meta-qcom-robotics-sdk``

``meta-qcom-qim-product-sdk``

| 

::

       |Registered developer from a verified organization|Base build: High-level OS and firmware source \(GPS only\)`Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

::

       |Base build + QIMP SDK`Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIMPSDK`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

::

       |Base build + QIMP SDK + QIRP SDK`Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NM_QIRPSDK`

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

::

       |Licensed developer with additional access|Base build: High-level OS and firmware \(GPS only\) source`Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

::

       |Base build + QIMP SDK \(GPS only\)`Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|QIMPSDK`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

::

       |Base build: High-level OS and firmware \(GPS and modem\) source`Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

| 

::

       |Base build + QIMP SDK \(GPS and modem\)`Qualcomm_Linux.SPF.1.0|AMSS|Standard|OEM|QIMPSDK`

\|\ ``meta-qcom``

``meta-qcom-hwe``

``meta-qcom-extras``

``meta-qcom-qim-product-sdk``

| 

::

       For Yocto layer descriptions, see [Table 1](qsc_launcher.rst#Yocto_layer_descriptions).

-  Start the download:

   ::

      # cd to directory where you have 300 GB of free storage space to create your workspaces
      mkdir <WORKSPACE_DIR>
      cd <WORKSPACE_DIR>
      # WORKSPACE_DIR=/local/mnt/workspace/Qworkspace_QIMPSDK

      # Example
      qsc-cli download --workspace-path '/local/mnt/workspace/Qworkspace_QIMPSDK' --product 'QCM6490.LE.1.0' --build 'QCM6490.LE.1.0-00218-STD.PROD-1' --distribution 'Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PB_QIMPSDK'

When the process is completed successfully, the software product is
available in the user-provided workspace directory.

**Note:** A new workspace is required for each distribution if you are
downloading more than one distribution.

.. _section_yhy_11w_q1c_vinayjk_03-07-24-006-28-270:

Compile
-------

Start the compilation after the download completes:

**Note:** Depending on the size of the software and host machine
configuration, compilation may take a few hours.

::

   qsc-cli compile --workspace-path '<absolute_workspace_path>'
    
   # Example
   qsc-cli compile --workspace-path '/local/mnt/workspace/Qworkspace_QIMPSDK'

This process builds the Qualcomm firmware as needed and also completes
the build for the Qualcomm Linux.

**Note:** If you see a BitBake fetcher error, retry compilation to work
around this error. If the issue persists, see `BitBake Fetcher
Error <troubleshoot_sync_build_and_flash.rst#do_fetch_error>`__ for a
solution.

On a successful build of the ``qcom-wayland`` distributions, you can see
the images at the following path:

::

   # system.img is present at the following path
   <workspace_path>/DEV/LE.QCLINUX.1.0.r1/build-qcom-wayland/tmp-glibc/deploy/images/qcm6490/qcom-multimedia-image/*

On a successful build of the ``qcom-robotics-ros2-humble`` (QIRP)
distribution, you can see the QIRP SDK build artifacts at the following
paths:

::

   QIRP SDK artifacts: <workspace_path>/DEV/LE.QCROBOTICS.1.0.r1/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/qirpsdk_artifacts/qirp-sdk_<version>.tar.gz
   # system.img is present at the following path
   Robotics image: <workspace_path>/DEV/LE.QCROBOTICS.1.0.r1/build-qcom-robotics-ros2-humble/tmp-glibc/deploy/images/qcm6490/qcom-robotics-full-image/*

.. _section_qb2_sp1_q1c_vinayjk_03-04-24-129-15-322:

Recompile
---------

Recompile your workspace if you already have a workspace built using QSC
CLI:

::

   # qsc-cli compile --image '<software_image_name>' --workspace-path '<absolute_workspace_path>'
    
   # Example
   qsc-cli compile --image LE.QCLINUX.1.0.r1 --workspace-path '/local/mnt/workspace/Qworkspace_QIMPSDK'

**Note:** For more information on software image names (``--image``),
see QSC-CLI input parameters table in the `Release
Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240626095531/>`__.

.. _section_x2k_vnf_w1c:

Flash
-----

For the steps to flash software images to the device, see `Flash images
for registered users <flash_images.rst>`__.

