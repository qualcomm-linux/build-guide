Set up the Ubuntu host computer
--------------------------------

Install and configure the software tools on the Ubuntu host computer.

1. Install the following packages:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install build-essential chrpath cpio debianutils diffstat file gawk gcc git iputils-ping libacl1 locales python3 python3-git python3-jinja2 python3-pexpect python3-pip python3-subunit socat texinfo unzip wget xz-utils zstd 
         sudo apt-get install lib32stdc++6 libncurses5 checkinstall libreadline-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev curl
         sudo apt install pipx

         # This command should add the kas binary location to your PATH.
         # Restart your shell session after running this command for the changes to take effect.
         pipx ensurepath

         # The kas version is expected to be 4.8 or higher
         pipx install kas

#. Optionally download the kas-container script. kas is the tool used by Qualcomm Linux to sync and build the Yocto meta layers. The kas package also provides a kas-container script for running kas in a container. If you prefer running the image builds in an isolated environment, consider using kas-container instead.

   .. container:: nohighlight
      
      ::

         # kas-container can be run on any linux distribution with docker installed.
         wget -qO kas-container https://raw.githubusercontent.com/siemens/kas/refs/tags/5.1/kas-container
         chmod +x kas-container

#. Add your Qualcomm login ID with PAT to the ``~/.netrc`` file in your home directory:

   .. container:: nohighlight
      
      ::

         # Log in to qsc-cli to generate PAT
         qsc-cli login -u <username>
         # Run the following command to generate PAT
         qsc-cli show-access-token
         # This command gives output as shown in the following note
         # The last line in this output is the token, which can be used to access
         # Qualcomm Proprietary repositories. This token expires in two weeks.

#. Use your preferred text editor to edit the ``~/.netrc`` file and add the following entries. Create the ``~/.netrc`` file if it doesn't exist.

   .. container:: nohighlight
      
      ::

         machine chipmaster2.qti.qualcomm.com
         login <your Qualcomm login id>
         password <your PAT token>

         machine qpm-git.qualcomm.com
         login <your Qualcomm login id>
         password <your PAT token>

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

         # Add the following git configurations to follow remote redirects from http-alternates files or alternates
         git config --global http.https://chipmaster2.qti.qualcomm.com.followRedirects true
         git config --global http.https://qpm-git.qualcomm.com.followRedirects true

#. Set up Python 3.10.2:

   .. note:: Skip the following instructions if you already have Python 3.10.2 or later versions.

   .. container:: nohighlight
      
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

.. note::
  The `kas <https://kas.readthedocs.io/en/latest/>`__ tool is used by Qualcomm Linux to sync the meta layers, configure the environment and execute the bitbake commands.
