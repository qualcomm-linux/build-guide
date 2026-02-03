.. _ubuntu_host_setup_github_unreg:

Set up the Ubuntu host computer
--------------------------------

Install and configure the required software tools on the Ubuntu host computer.

1. Install the following packages to prepare your host environment for the Yocto build:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install kas repo gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect python3-yaml xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool locales tar python-is-python3 file libxml-opml-simplegen-perl vim whiptail g++ libacl1

#. Set up the locales (if not set up already):

   .. container:: nohighlight
      
      ::

         sudo locale-gen en_US.UTF-8
         sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
         export LC_ALL=en_US.UTF-8
         export LANG=en_US.UTF-8

#. Update git configurations:

   .. container:: nohighlight
      
      ::

         # Check if your identity is configured in .gitconfig
         git config --get user.email
         git config --get user.name

         # Run the following commands if you don't have your account identity set in .gitconfig
         git config --global user.email <Your email ID>
         git config --global user.name <"Your Name">

         # Add the following UI color option for output of console (optional)
         git config --global color.ui auto

         # Add the following git configurations to fetch large size repositories and to avoid unreliable connections
         git config --global http.postBuffer 1048576000
         git config --global http.maxRequestBuffer 1048576000
         git config --global http.lowSpeedLimit 0
         git config --global http.lowSpeedTime 999999
