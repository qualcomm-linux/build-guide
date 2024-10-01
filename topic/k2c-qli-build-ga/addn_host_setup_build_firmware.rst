.. _host_setup_to_build_sdks:

Ubuntu host setup
-------------------------

The Ubuntu host machine needs a few setup operations to ensure that the required software tools are ready.

1. Install the following packages:

   ::

      sudo apt update
      sudo apt install repo gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint xterm python3-subunit mesa-common-dev zstd liblz4-tool locales tar python-is-python3 file libxml-opml-simplegen-perl vim whiptail
      sudo apt-get install lib32stdc++6 libncurses5 checkinstall libreadline-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev curl

#. Add your Qualcomm login ID with PAT to the ``~/.netrc`` file in your home directory:

   ::

      # Log in to qsc-cli to generate PAT
      qsc-cli login -u <username>
      # Run the following command to generate PAT
      qsc-cli pat --get
      # This command gives output as shown in the following note
      # The last line in this output is the token, which can be used to access
      # Qualcomm Proprietary repositories. This token expires in two weeks.

   .. note::
      | user@hostname:/local/mnt/workspace$ qsc-cli pat --get
      | [Info]: Starting qsc-cli version 0.0.0.9
      | **5LThNlklb55mMVLB5C2KqUGU2jCF**

#. Use your preferred text editor to edit the ``~/.netrc`` file and add the following entries:

   .. note:: Create the ``~/.netrc`` file if it does not exist.

   ::

      machine chipmaster2.qti.qualcomm.com
      login <your Qualcomm login id>
      password <your PAT token>

      machine qpm-git.qualcomm.com
      login <your Qualcomm login id>
      password <your PAT token>

#. Set up the locales (if not set up already):

   ::

      sudo locale-gen en_US.UTF-8
      sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
      export LC_ALL=en_US.UTF-8
      export LANG=en_US.UTF-8

#. Update git configurations:

   ::

      # Check if your identity is configured in .gitconfig
      git config --get user.email
      git config --get user.name

      # Run the following commands if you do not have your account identity set in .gitconfig
      git config --global user.email <Your email ID>
      git config --global user.name <"Your Name">

      # Add the following UI color option for output of console (optional)
      git config --global color.ui auto

      # Add the following git configurations to fetch large size repositories and to avoid unreliable connections
      git config --global http.postBuffer 1048576000
      git config --global http.maxRequestBuffer 1048576000
      git config --global http.lowSpeedLimit 0
      git config --global http.lowSpeedTime 999999

      # Add the following git configurations to follow remote redirects from http-alternates files or alternates
      git config --global http.https://chipmaster2.qti.qualcomm.com.followRedirects true
      git config --global http.https://qpm-git.qualcomm.com.followRedirects true

#. Set up Python 3.10.2:

   .. note:: Skip the following instructions if you already have Python 3.10.2 or later versions.

   ::

      python --version
      # Download it in a directory of your choice 
      wget https://www.python.org/ftp/python/3.10.2/Python-3.10.2.tgz
      tar -xvf Python-3.10.2.tgz
      cd Python-3.10.2
      # Use sudo if you need to access /opt
      ./configure --prefix=/opt/python3 --enable-optimizations
      make
      make install
      ln -s /opt/python3/bin/python3  /opt/python3/bin/python
      export PATH=/opt/python3/bin:$PATH
      export PYTHONPATH=/opt/python3/lib:$PYTHONPATH
