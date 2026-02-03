.. _build_with_dockerfile:

Build with Dockerfile
-------------------------

Set up the Ubuntu host computer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Install and configure the required software tools on the Ubuntu host computer.

1. Install the following packages to prepare your host environment for the Yocto build:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install git docker

#. Add the user to the Docker group:

   .. container:: nohighlight
      
      ::

         sudo groupadd docker
         sudo usermod -aG docker $USER
         newgrp docker
         # To check if you are part of a Docker group, run the following command:
         sudo grep /etc/group -e "docker"

# Clone the ``siemens/kas`` git repository, which provides a Dockerfile for the build: 

   .. container:: nohighlight
      
      ::

         git clone https://github.com/siemens/kas


.. _build_with_docker_bsp_image:

Build a BSP image
^^^^^^^^^^^^^^^^^^^^^

Dockertag uses lowercase letters for the release folder followed by the Dockerfile OS version, to identify the release build with the Dockerfile. Docker doesn't allow uppercase letters in the Dockertag. To troubleshoot Docker issues, see :ref:`Troubleshoot Docker <troubleshoot_docker>`.

Create and build a Yocto Docker image:

1. Download Qualcomm Yocto and the supporting meta-layers. For the latest ``<meta-qcom-release-tag>``, see the section *Build-Critical Release Tags* in the `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   .. container:: nohighlight
      
      ::

         # cd to directory where you have 300 GB of free storage space to create your workspaces
         mkdir <workspace-dir>
         cd <workspace-dir>
         git clone https://github.com/qualcomm-linux/meta-qcom-releases -b <meta-qcom-release-tag>
         kas/kas-container checkout meta-qcom-releases/lock.yml

#. Build the software image. Build targets are defined based on machine and distro combinations. 

   .. container:: nohighlight
      
      ::

         kas/kas-container build --skip repos_checkout meta-qcom/ci/<machine>:meta-qcom/ci/<distro>

   For various ``<machine>`` and ``<distro>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. After a successful build, check that the ``rootfs.img`` file is in the build artifacts:

   .. container:: nohighlight

      ::

         bash docker/docker_run.sh -t qcom-6.6.116-qli.1.7-ver.1.1_22.04 -r qcom-6.6.116-QLI.1.7-Ver.1.1 -M <machine> --build-override <override> --alternate-repo true
         # Example, bash docker/docker_run.sh -t qcom-6.6.116-qli.1.7-ver.1.1_22.04 -r qcom-6.6.116-QLI.1.7-Ver.1.1 -M qcs6490-rb3gen2-vision-kit --build-override custom --alternate-repo true

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

   The build workspace is available in
   ``<qcom-download-utils download path>/<release>/build-qcom-wayland``.
   For example,
   ``qcom-download-utils/qcom-6.6.116-QLI.1.7-Ver.1.1/build-qcom-wayland``.

.. note:: 
   - **# ERROR: error.GitError: git config (‘–replace-all’,‘color.ui’, ‘auto’): error: couldn't write config file /home/$USER/.gitconfig: Device or resource busy**
     
     As Git 1.8.4 is enabled by default, you will see this error when the UI color configuration is not set in gitconfig. To enable color display in your account, run the following command: ``git config --global color.ui auto``.

   - If you see and fix a build error, run the commands in :ref:`Rebuild <build_with_docker_rebuild>`.

.. _build_with_docker_rebuild:

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
       qcom-6.6.116-qli.1.7-ver.1.1_22.04                        latest                      8fcea388d8ca   2 days ago     1.47GB

#. Run the following commands outside the Docker container:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/qcom-download-utils/qcom-6.6.116-QLI.1.7-Ver.1.1

         # Run the following commands inside the base image build location
         bash
         docker run -it -v "${HOME}/.gitconfig":"/home/${USER}/.gitconfig" -v "${HOME}/.netrc":"/home/${USER}/.netrc" -v $(pwd):$(pwd) -w $(pwd) qcom-6.6.116-qli.1.7-ver.1.1_22.04 /bin/bash

         # Example
         WORKSPACE=<workspace_path>/qcom-download-utils/qcom-6.6.116-QLI.1.7-Ver.1.1

#. Set up the build environment:

   .. container:: nohighlight
      
      ::

         # cd <release directory>
         MACHINE=<machine> DISTRO=qcom-wayland QCOM_SELECTED_BSP=<override> source setup-environment
         # Example, MACHINE=qcs6490-rb3gen2-vision-kit DISTRO=qcom-wayland QCOM_SELECTED_BSP=custom source setup-environment
         # source setup-environment: Sets the environment, creates the build directory build-qcom-wayland,
         # and enters into build-qcom-wayland directory.

   For various ``<machine>`` and ``<override>`` combinations, see `Release Notes <https://docs.qualcomm.com/doc/80-70023-300/>`__.

#. Build the software image:

   .. container:: nohighlight
      
      ::

         bitbake qcom-multimedia-image
         
#. Close Docker before you flash the images.

Flash
^^^^^^^

To flash the software images to the device, see :ref:`Flash software images <flash_images>`.

Related topics
---------------
- :ref:`Connect to UART shell <connect_uart>`
- :ref:`Connect to network <connect_to_network>`
- :ref:`Sign in using SSH <use-ssh>`
- :ref:`Troubleshoot sync, build, and flash issues <troubleshoot_sync_build_and_flash>`
