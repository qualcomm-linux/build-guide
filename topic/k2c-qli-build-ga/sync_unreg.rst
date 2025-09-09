Sync
-----

Use the Repo tool that was installed during the :ref:`Ubuntu host setup <ubuntu_host_setup_github_unreg>` to download git repositories and other attributes from the `Qualcomm manifest <https://github.com/quic-yocto/qcom-manifest>`__. The Repo tool downloads the manifests using the ``repo init`` command.

The following table shows an example mapping of the Yocto layers to the manifest release tags. Use this mapping to download and build Qualcomm Linux.

.. list-table:: Mapping Yocto layers to manifest release tags
   :header-rows: 1
   :class: longtable


   * - Yocto layers
     - Manifest release tag
     - Reference distribution (``DISTRO``)

   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
     - BSP build: High-level OS and prebuilt firmware (GPS only)
       
       ``qcom-6.6.97-QLI.1.6-Ver.1.0.xml``
     - ``qcom-wayland``

   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-qcom-qim-product-sdk``
     - BSP build + Qualcomm IM SDK build:
       
       ``qcom-6.6.97-QLI.1.6-Ver.1.0_qim-product-sdk-2.1.0.xml``
     - ``qcom-wayland``
   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-qcom-realtime``
     - BSP build + Real-time kernel build:
       
       ``qcom-6.6.97-QLI.1.6-Ver.1.0_realtime-linux-1.0.xml``
     - ``qcom-wayland``
   * - 
        - ``meta-qcom``
        - ``meta-qcom-hwe``
        - ``meta-qcom-distro``
        - ``meta-ros``
        - ``meta-qcom-robotics``
        - ``meta-qcom-robotics-distro``
        - ``meta-qcom-robotics-sdk``
        - ``meta-qcom-qim-product-sdk``
     - BSP build + QIR SDK build:
       
       ``qcom-6.6.97-QLI.1.6-Ver.1.0_robotics-sdk-1.0.xml``
     - ``qcom-robotics-ros2-humble``

The release tag syntax is as follows:

- Manifest release tag:
     
  ``qcom-<Linux LTS Kernel Version>-QLI.<version>-Ver.<release>.xml``
    
  For example, the manifest release tag ``qcom-6.6.97-QLI.1.6-Ver.1.0.xml`` denotes the following:
     
  - 6.6.90: Qualcomm Linux kernel
  - QLI.1.5: Qualcomm Linux v1.5
  - 1.1: Milestone release

- Additional productization manifest release tag:
   
  ``qcom-<Linux LTS Kernel version>-QLI.<version>-Ver.<milestone release>_<product/customization>-<patch release>.xml``

  For example, the additional productization manifest release tag ``qcom-6.6.97-QLI.1.6-Ver.1.0_qim-product-sdk-2.1.0.xml`` denotes the following:
     
  - 6.6.90: Qualcomm Linux kernel
  - QLI.1.5: Qualcomm Linux v1.5
  - qim-product-sdk-2.0.1: Qualcomm IM SDK release on top of QLI.1.5

    Other product/customization examples:

    - *realtime-linux-1.0*
    - *robotics-product-sdk-1.1*
  - 1.1: Milestone release
  - 2.0.1: Patch release associated with the milestone release

For more information about the Yocto layers, see `Qualcomm Linux metadata layers <https://docs.qualcomm.com/bundle/publicresource/topics/80-70020-27/qualcomm_bsp_metadata_layers.html>`__.