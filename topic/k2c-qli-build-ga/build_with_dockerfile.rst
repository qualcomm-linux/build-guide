.. _build_private_distribution:

Build with Dockerfile
-------------------------

.. _section_d5g_qcq_p1c_vinayjk_03-02-24-1449-27-555:

Ubuntu host setup
^^^^^^^^^^^^^^^^^^^^^
The Ubuntu host machine must be setup to ensure that the required software tools are installed and configured for use.

-  Install git:

   .. container:: nohighlight
      
      ::

         # Install git if you have not already installed
         sudo apt install git

-  Clone the ``qcom-download-utils`` git repository, which provides a Dockerfile for Qualcomm public Yocto layers and a few helper scripts:

   .. container:: nohighlight
      
      ::

         mkdir <workspace_path>
         cd <workspace_path>
         git clone https://git.codelinaro.org/clo/le/qcom-download-utils.git
         cd qcom-download-utils

-  Add the user to the Docker group:

   .. container:: nohighlight
      
      ::

         sudo usermod -aG docker $USER
         newgrp docker
         # To check if user is part of a Docker group, run the following command:
         sudo grep /etc/group -e "docker"

.. _section_anw_sv3_v1c_vinayjk_03-23-24-142-19-530:

Check the host machine configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

-  Check if your host machine is configured accurately:

   .. container:: nohighlight
      
      ::

         bash utils/check_config.sh
         # Resolve the errors and run this command until no errors show up

-  Install Docker:

   .. container:: nohighlight
      
      ::

         bash docker/docker_setup.sh

.. note:: 
   The following figure shows the directory structure of ``qcom-download-utils`` in relation to the Docker setup:

   .. image:: ../../media/k2c-qli-build-ga/qcom-download-utils-folder.png

.. _section_opk_sh4_w1c:

Build BSP image
^^^^^^^^^^^^^^^^^^^^^

Create a Yocto Docker image and build:

1. Run ``docker_build.sh`` to create the Docker image with Dockerfile
   (**Dockerfile_22.04**) and Dockertag
   (**qcom-6.6.52-qli.1.3-ver.1.1_22.04**). This Docker image is used to
   create the container environment to run the Yocto build.

   **Dockertag**: Use lowercase letters for the release folder followed by the Dockerfile OS version to identify the release build with the Dockerfile since Docker does not permit uppercase letters in the Dockertag.

   .. note:: 
      To troubleshoot Docker issues, see :ref:`Troubleshoot Docker <section_hkm_2dc_p1c_vinayjk_02-29-24-1641-18-155>`.

   .. container:: nohighlight
      
      ::

         bash docker/docker_build.sh -f ./docker/dockerfiles/Dockerfile_22.04 -t qcom-6.6.52-qli.1.3-ver.1.1_22.04

   If you face any issues while running ``docker_build.sh``, see the
   following solution:

   .. container:: nohighlight
      
      ::

         # Error 1: Cache-related issue.
            # If you are facing issues with the docker build command, try using --no-cache option. This option forces rebuilding of the layers that are already available
            bash docker/docker_build.sh -n --no-cache -f ./docker/dockerfiles/Dockerfile_22.04 -t qcom-6.6.52-qli.1.3-ver.1.1_22.04

         # Error2: Response from daemon: Get "https://registry-1.docker.io/v2/": http: server gave HTTP response to HTTPS client
            # Check with your IT administrator to acquire ``registry-mirrors`` URL and replace <docker-mirror-host>`` in the following solution 
            # Using a tab instead of space and other invisible white-space characters may break the proper work of json configuration files
            # and later may lead to the Docker service failing to start

            # Solution:
            sudo vim /etc/docker/daemon.json
            # Add an entry similar to the following in /etc/docker/daemon.json:
            {
               "registry-mirrors": ["https://<docker-mirror-host>"]
            }
            # Restart the Docker service to take the new settings
            sudo systemctl restart docker

#. Sync and build the Yocto image in a Docker container with the Docker
   tag and release parameters:

   .. container:: nohighlight
      
      ::

         bash docker/docker_run.sh -t qcom-6.6.52-qli.1.3-ver.1.1_22.04 -r qcom-6.6.52-QLI.1.3-Ver.1.1 -M <machine> --build-override <override> --alternate-repo true
         # Example, bash docker/docker_run.sh -t qcom-6.6.52-qli.1.3-ver.1.1_22.04 -r qcom-6.6.52-QLI.1.3-Ver.1.1 -M qcs6490-rb3gen2-vision-kit --build-override custom --alternate-repo true 

   .. note::
      For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

   The build workspace is available in
   ``<qcom-download-utils download path>/<release>/build-qcom-wayland``.
   For example,
   ``qcom-download-utils/qcom-6.6.52-QLI.1.3-Ver.1.1/build-qcom-wayland``.

.. note:: 
   - **# ERROR: error.GitError: git config (‘–replace-all’,‘color.ui’, ‘auto’): error: could not write config file /home/$USER/.gitconfig: Device or resource busy**
     
     This error is triggered when your gitconfig does not set the UI color configuration as Git 1.8.4 is enabled by default. To enable color display in your account, run the following command: ``git config --global color.ui auto``.

   - If a build error is triggered and fixed, run the commands in :ref:`Rebuild <section_p1h_tv3_v1c_vinayjk_03-23-24-142-26-643>`.

.. _section_mp2_1n4_w1c:

Build QIMP SDK image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. :ref:`Build BSP image <section_opk_sh4_w1c>` using Docker.
#. Build QIMP SDK on top of the base image using Docker:

   a. Run the ``docker run`` command:

      .. container:: nohighlight
      
         ::

            # Run the following commands inside the base image build location
            cd <workspace_path>/qcom-download-utils/qcom-6.6.52-QLI.1.3-Ver.1.1
            bash
            docker run -it -v "${HOME}/.gitconfig":"/home/${USER}/.gitconfig" -v "${HOME}/.netrc":"/home/${USER}/.netrc" -v $(pwd):$(pwd) -w $(pwd) qcom-6.6.52-qli.1.3-ver.1.1_22.04 /bin/bash

   #. Clone the QIMP SDK layer into the workspace:

      .. container:: nohighlight
      
         ::

            git clone https://github.com/quic-yocto/meta-qcom-qim-product-sdk -b <meta-qcom-qim-product-sdk release tag> layers/meta-qcom-qim-product-sdk
            # Example, <meta-qcom-qim-product-sdk release tag> is qcom-6.6.52-QLI.1.3-Ver.1.1_qim-product-sdk-1.1.2

      To build a QIMP SDK layer, the following export is required:

      .. container:: nohighlight
      
         ::

            export EXTRALAYERS="meta-qcom-qim-product-sdk"

   #. Set up the build environment:

      .. container:: nohighlight
      
         ::

            MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
            # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
            # and enters into build-qcom-wayland directory.

      .. note::
          For the ``MACHINE`` parameter values, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

   #. Build the software image:

      .. container:: nohighlight
      
         ::

            bitbake qcom-multimedia-image
            # Build SDK image
            bitbake qcom-qim-product-sdk

.. _section_p1h_tv3_v1c_vinayjk_03-23-24-142-26-643:

Rebuild
^^^^^^^^^^^^^^

To rebuild after any modifications to the software release, use your existing workspace built using Docker:

1. List the Docker images:

   .. container:: nohighlight
      
      ::

         docker images

   This command generates the following output:

   .. container:: screenoutput

       REPOSITORY                                               TAG                         IMAGE ID       CREATED        SIZE
       qcom-6.6.52-qli.1.3-ver.1.1_22.04                        latest                      8fcea388d8ca   2 days ago     1.47GB

#. Attach the container:

   .. container:: nohighlight
      
      ::

         # Run the following commands outside the Docker container
         cd <workspace_path>/qcom-download-utils/qcom-6.6.52-QLI.1.3-Ver.1.1

         # Run the following commands inside the base image build location
         bash
         docker run -it -v "${HOME}/.gitconfig":"/home/${USER}/.gitconfig" -v "${HOME}/.netrc":"/home/${USER}/.netrc" -v $(pwd):$(pwd) -w $(pwd) qcom-6.6.52-qli.1.3-ver.1.1_22.04 /bin/bash

         # Example
         WORKSPACE=<workspace_path>/qcom-download-utils/qcom-6.6.52-QLI.1.3-Ver.1.1

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         # cd <release directory>
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   .. note::
      For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-241225194606/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image

.. note:: Exit from Docker before you flash the images.

.. _section_x2k_vnf_w1c:

Flash
^^^^^^^

To flash the software images to the device, see :ref:`Flash software images <flash_images>`.