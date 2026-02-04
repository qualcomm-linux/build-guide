.. _build_with_dockerfile:

Build with Dockerfile
-------------------------

Set up the Ubuntu host computer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Install and configure the required software tools on the Ubuntu host computer.

1. Install git:

   .. container:: nohighlight
      
      ::

         # Install git if you haven't already installed
         sudo apt install git

#. Clone the ``qcom-download-utils`` git repository, which provides a Dockerfile for the Qualcomm public Yocto layers and a few helper scripts:

   .. container:: nohighlight
      
      ::

         mkdir <workspace_path>
         cd <workspace_path>
         git clone https://git.codelinaro.org/clo/le/qcom-download-utils.git
         cd qcom-download-utils

#. Add the user to the Docker group:

   .. container:: nohighlight
      
      ::

         sudo groupadd docker
         sudo usermod -aG docker $USER
         newgrp docker
         # To check if you are part of a Docker group, run the following command:
         sudo grep /etc/group -e "docker"

#. Configure your host computer:

   .. container:: nohighlight
      
      ::

         bash utils/check_config.sh
         # Resolve the errors and run this command until no errors show up

#. Install Docker:

   .. container:: nohighlight
      
      ::

         bash docker/docker_setup.sh

.. note:: 
   The following sample figure shows the directory structure of ``qcom-download-utils`` in relation to the Docker setup:

   .. image:: ../../media/k2c-qli-build-ga/qcom-download-utils-folder.png

.. _build_with_docker_bsp_image:

Build a BSP image
^^^^^^^^^^^^^^^^^^^^^

Dockertag uses lowercase letters for the release folder followed by the Dockerfile OS version, to identify the release build with the Dockerfile. Docker doesn't allow uppercase letters in the Dockertag. To troubleshoot Docker issues, see :ref:`Troubleshoot Docker <troubleshoot_docker>`.

Create and build a Yocto Docker image:

1. Run ``docker_build.sh`` to create the Docker image with Dockerfile (``Dockerfile_22.04``) and Dockertag (``qcom-6.6.116-qli.1.7-ver.1.1_22.04``). Use this Docker image to create the container environment and run the Yocto build.

   

   .. container:: nohighlight
      
      ::

         bash docker/docker_build.sh -f ./docker/dockerfiles/Dockerfile_22.04 -t qcom-6.6.116-qli.1.7-ver.1.1_22.04

   If you face any issues while running ``docker_build.sh``, see the
   following solution:

   .. container:: nohighlight
      
      ::

         # Error 1: Cache-related issue.
            # If you are facing issues with the docker build command, try using --no-cache option. This option forces rebuilding of the layers that are already available
            bash docker/docker_build.sh -n --no-cache -f ./docker/dockerfiles/Dockerfile_22.04 -t qcom-6.6.116-qli.1.7-ver.1.1_22.04

         # Error2: Response from daemon: Get "https://registry-1.docker.io/v2/": http: server gave HTTP response to HTTPS client
            # Check with your IT administrator to acquire ``registry-mirrors`` URL and replace <docker-mirror-host>`` in the following solution 
            # Using a tab instead of space and other invisible white-space characters may break the proper functioning of the JSON configuration files
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
