.. _troubleshoot_sync_build_and_flash:

***************
Troubleshoot
***************

Addresses common issues encountered during the Qualcomm Linux build and flash processes, organized into four categories: Docker, Sync, Build, and Flash.

.. _troubleshoot_docker:

Docker
--------

-  "docker: Cannot connect to the Docker daemon at
   unix:///var/run/docker.sock. Is the docker daemon running?"

   Start Docker:

   .. container:: nohighlight
      
      ::

         sudo systemctl start docker

-  "Error response from daemon: Get “https://registry-1.docker.io/v2/”: http: server gave HTTP response to HTTPS client"

   1. Add an internal Docker registry mirror (internal setting for the Qualcomm network). Don't include # comments in the JSON configuration file. Using a tab instead of space and other invisible whitespace characters may break the functionality of JSON configuration files and can also lead to ``docker.service`` failing to start.

      .. container:: nohighlight
      
         ::

            sudo vim /etc/docker/daemon.json
            # Add an entry similar to the following in /etc/docker/daemon.json:
            {
               "registry-mirrors": ["https://docker-registry.qualcomm.com"]
            }

      .. note:: As an alternative, you can add the following entry in ``/etc/docker/daemon.json``:

                ``"registry-mirrors": ["https://ccr.ccs.tencentyun.com"]``

   #. Restart the Docker service to take the new settings.

      .. container:: nohighlight
      
         ::

            sudo systemctl restart docker

-  "Failed to download from https://download.docker.com"

    If you are unable to access or download from https://download.docker.com, run the following commands to install docker:

      .. container:: nohighlight
      
         ::

            sudo apt update
            sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release
            curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add –
            sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"

-  "Docker failure due to Virtualization not enabled"

   Enable virtualization from the BIOS to resolve this error.
   Follow the specific instructions from the system provider to enable
   the virtualization. For example, the following steps can enable
   virtualization provided by a system provider:

   1. When the system is booting up, step into the BIOS. The :guilabel:`BIOS` window appears.
   2. Switch to the :guilabel:`Advanced` tab.
   3. In the :guilabel:`CPU Configuration` section, set :guilabel:`Virtualization Technology` to enabled.
   4. Save and exit.
   5. Restart the system.

-  "Permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock"

   This happens when ``qsc-cli`` is already installed on the machine and you aren't part of the Docker group.

   1. Add to the Docker group:

      .. container:: nohighlight
      
         ::

            sudo groupadd docker
            sudo usermod -aG docker $USER
            newgrp docker

   #. Confirm that you are part of the Docker group:

      .. container:: nohighlight
      
         ::

            sudo grep /etc/group -e "docker"
            # This command shows a list of users who are part of the Docker group; must include your user ID

   #. Sign out and sign in again for the access to take effect:

      .. container:: nohighlight
      
         ::

            # You can run the following command to check if you are part of the Docker group
            id -a
            # This command returns an output string which should include 'docker'

Sync
-------

-  "repo init or sync failure with except ManifestInvalidRevisionError,
   e:"

   You might see this issue after installing the Repo package:

   -  If you have redirection in your ``/etc/gitconfig`` or
      ``~/.gitconfig`` to an internal mirror.
   -  If your internal mirror has a prefix to branches while mirroring.
      For example, if ``/etc/gitconfig`` redirects and the internal
      mirror has a stable branch from upstream git mirrored as
      ``aosp/stable``, then the following error appears while performing
      ``repo init``:

   .. container:: nohighlight
      
      ::

         [url "ssh://<internal mirror>:<port>/tools/repo"]
         insteadOf = https://android.googlesource.com/tools/repo
         insteadOf = https://gerrit.googlesource.com/git-repo

   .. container:: nohighlight
      
      ::

         File "/local/mnt/workspace/<userid>/test_repo/.repo/repo/main.py", line 126
            except ManifestInvalidRevisionError, e:
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
         SyntaxError: multiple exception types must be parenthesized

   The steps to resolve this error are as follows:

   1. Remove the older ``.repo`` folder. This will be in the directory where you ran ``repo init`` command earlier.

      .. container:: nohighlight
      
         ::

            rm -rf .repo

   #. Export and run the repo commands to fix the repo issues. The REPO_REV must point to the mirrored branch from upstream 'stable' branch of https://gerrit.googlesource.com/git-repo.

      .. container:: nohighlight
      
         ::

            export REPO_REV='aosp/stable'

-  "Install repo “Server certificate verification failed”"

   - If you see a certificate error such as "Server certificate verification failed. CAfile: none CRLfile: none", configure git to disable SSL certificate verification with git configuration. Discuss with your IT administration for further guidance. You can use either of the following commands to disable SSL:

     .. container:: nohighlight
      
        ::

           export GIT_SSL_NO_VERIFY=1
           git config --global http.sslverify false

   - If your region is blocking access to ``android.googlesource``, try the following configuration to fetch Repo from CodeLinaro Mirror:

     .. container:: nohighlight
      
        ::

           git config --global url.https://git.codelinaro.org/clo/la/tools/repo.insteadOf https://android.googlesource.com/tools/repo

-  "error.GitError: git config (‘–replace-all’, ‘color.ui’, ‘auto’):
   error: could not write config file /home/$USER/.gitconfig: Device or
   resource busy"

   This error occurs when your gitconfig doesn't set the UI color
   configuration. This configuration is set by default in Git 1.8.4 and
   later versions. Run the following command to enable the color display
   in your account:

   .. container:: nohighlight
      
      ::

         git config --global color.ui auto

-  "[Error]: Failed preparing build for compilation. Error: Error
   setting docker credentials. Error: “Error saving credentials: error
   closing temp file: close
   /usr2/<userid>/.docker/config.json3322274803: disk quota
   exceeded\\n”"

   QSC CLI uses the home directory only for a few kilo bytes (kB). Clear
   a few MBs from your home directory.

-  "[Error]: The “path” argument must be of type string. Received undefined"

   **Error excerpt**

   .. container:: nohighlight
      
      ::

         qsc-cli chip-software download --workspace-path '/local/mnt/workspace/<userid>/K2L/QSC_CLI_BUILD/build' --product 'QCM6490.LE.2.0' --release 'r00004.2' --distribution 'Qualcomm_Linux.SPF.2.0|TEST|DEVICE|PUBLIC'
         [Info]: Starting qsc-cli version 0.0.0.7 
         (node:2924765) ExperimentalWarning: The Fetch API is an experimental feature. This feature could change at any time
         (Use `qsc-cli --trace-warnings ...` to show where the warning was created)
         [Info]: Checking if Workspace already exists
         [Info]: Saved updated Workspace info
         [Info]: Workspace Setup Completed
         [Error]: The "path" argument must be of type string. Received undefined

   **Solution**

   This error occurs if QSC CLI is incompatible with Qlauncher. Qlauncher will be deprecated and replaced with a new application from the QSC. If you have Qlauncher in the workspace, you can run the following commands:

   .. container:: nohighlight
      
      ::

         # Find your workspace within the Qlauncher UI
         # Take a backup of the following metadata file if you want to preserve the older workspace created with Qlauncher.
         # These will work only with Qlauncher app. You can reinstall the app at a later time again to access. If you don't 
         # need the workspaces, you can delete this file using:
         mv /var/lib/qcom/data/qualcomm_launcher/workspaces2.json /var/lib/qcom/data/qualcomm_launcher/workspaces2.json.bak
         # Uninstall Qlauncher with the following command:
         qsc-cli tool uninstall qualcomm_launcher

-  "docker: Error response from daemon: error while creating mount
   source path ‘/usr2/<userid>/.netrc’: mkdir /usr2/<userid>/.netrc:
   permission denied"

   **Error excerpt**

   .. container:: nohighlight
      
      ::

         Updating files: 100% (64/64), done.
         2024-02-29T07:58:00: Sync Command Completed
         2024-02-29T07:58:01: Finished setup.
         [Info]: Setting Docker Credential
         2024-02-29T07:58:03: Authorization successful
         2024-02-29T07:58:03: Sync Command Starting
         2024-02-29T07:58:03: Running Sync Command for SyncOpenSourceCode - DownloadOpenSource
         docker: Error response from daemon: error while creating mount source path '/usr2/ramevelp/.netrc': mkdir /usr2/ramevelp/.netrc: permission denied.
         2024-02-29T07:58:04: Sync Command Failed
         [Error]: Failed SP Download with error: 2024-02-29T07:58:04: Sp Download failed. ExitCode: 126 Signal: 0  with errorcode 4
         [Error]: 2024-02-29T07:58:04: Sp Download failed. ExitCode: 126 Signal: 0

   **Solution**

   This might happen due to the way IT has set up your home directory.
   Work with your IT administrator for any further changes to your home
   directory.

-  "fatal: couldn’t find remote ref refs/heads/qcom-linuxSTXscarthgap"

   If you see any stray characters while copying commands from the PDF,
   remove or replace the stray characters with appropriate symbols and
   rerun the command.

   **Example**

   .. container:: nohighlight
      
      ::

         # Replace the following command
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linuxSTXscarthgap -m qcom-6.6.116-QLI.1.7-Ver.1.1.xml
         # with
         repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-scarthgap -m qcom-6.6.116-QLI.1.7-Ver.1.1.xml

-  "pull access denied for 032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/ql-tool"

   This error can occur while running the ``qsc-cli`` download command.

   **Error excerpt**

   .. container:: nohighlight
      
      ::

         Unable to find image '032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/ql-tool:20.04.20231220102843864.9' locally
         docker: Error response from daemon: pull access denied for 032693710300.dkr.ecr.us-west-2.amazonaws.com/stormchaser/ql-tool, repository doesn't exist or may require 'docker login': denied: Your authorization token has expired. Reauthenticate and try again.

   **Solution**
      
   .. container:: nohighlight
      
      ::

         rm -rf ~/.docker/config.json

   Rerun the ``qsc-cli`` command.

Build
--------

-  "ERROR: linux-kernel-qcom-6.6-r0 do_menuconfig: No valid terminal
   found, unable to open devshell"

   This error can trigger while running a
   ``bitbake linux-kernel-qcom -c menuconfig`` command.

   **Error excerpt**

   .. container:: nohighlight
      
      ::

         ERROR: linux-kernel-qcom-6.6-r0 do_menuconfig: No valid terminal found, unable to open devshell.
         Tried the following commands:
               tmux split-window -c "{cwd}" "do_terminal"
               tmux new-window -c "{cwd}" -n "linux-kernel-qcom Configuration" "do_terminal"
               xfce4-terminal -T "linux-kernel-qcom Configuration" -e "do_terminal"
               terminology -T="linux-kernel-qcom Configuration" -e do_terminal

   **Solution**

   .. container:: nohighlight
      
      ::

         sudo apt install screen
         sudo apt install tmux

-  "NOTE: No reply from server in 30s"

   If you are seeing this error during the build on the rerun of
   ``qsc-cli chip-software compile`` or ``bitbake`` commands, you can try to delete
   ``bitbake.lock``, ``bitbake.sock``, and ``hashserve.lock`` from your
   partially built workspace and retry the build. For example, if you
   are building with ``qsc-cli``, then these files are found under
   ``<absoute_workspace_path>/DEV/LE.QCLINUX.2.0/build``.

.. _do_fetch_error_1:

-  "do_fetch: BitBake Fetcher Error: FetchError(‘Unable to fetch URL
   from any source’)"

   These are intermittent fetch failures. Check if there is a
   network/host issue at your end, else check if the server is creating
   this issue. You can increase ``postBuffer`` and
   ``maxRequestBuffer`` settings in your ``.gitconfig`` file if the
   errors occur while fetching the git repositories. If you are using
   ``qsc-cli``, then these configurations are already taken care of by the
   ``qsc-cli`` tool:

   .. container:: nohighlight
      
      ::

         git config --global http.postBuffer 1048576000
         git config --global http.maxRequestBuffer 1048576000

   If these configurations don't work, you can retry the compile to get
   past these intermittent errors for the first time.

   A few large git projects can generate this error. In such scenarios, manually clone the projects:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>/downloads/git2/
         git clone --bare --mirror https://<url>/<project-name>.git <workspace_path>/downloads/git2/<local-name>.git
         touch <workspace_path>/downloads/git2/<local-name>.git.done

   For example, when ``do_fetch`` fails for
   ``qualcomm_linux-spf-2-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git``, run the following command:

   .. container:: nohighlight
      
      ::

         git clone --bare --mirror https://qpm-git.qualcomm.com/home2/git/revision-history/qualcomm_linux-spf-2-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git <workspace_path>/downloads/git2/qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-2-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git
         touch <workspace_path>/downloads/git2/qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-2-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git.done

   After creating the ``.done`` file, proceed with the ``bitbake <image recipe>`` command. After completing the initial build,
   it's recommended to set up your own `download directory <https://docs.yoctoproject.org/4.0.16/singleindex.html#term-DL_DIR>`__.

-  "make[4]: /bin/sh: Argument list too long"

   This happens when the path of the workspace exceeds 90 characters. Reduce the workspace path length to avoid this failure.

-  "kernel-source/arch/arm64/boot/dts/qcom/qcm6490-idp.dts:8:10: fatal error: dt-bindings/iio/qcom,spmi-adc7-pmk8350.h: No such file or directory"

   The ``qcom,spmi-adc7-pmk8350.h`` file is part of the kernel source ``<kernel-src>/include/dt-bindings/iio/qcom,spmi-adc7-pmk8350.h``.

   1. Check the workspace for this file and initialize the environment to pick this file. While compiling dtbs, the kernel build system runs a GCC preprocessor to replace the macros in dts files by its definition. The mentioned path is one such place where several ``includes`` reside.

   #. Check if you have ``core.symlinks`` set to ``false`` in your git configuration. If yes, set it to true:

      .. container:: nohighlight
      
         ::

            git config --global core.symlinks true

-  "qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-2-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git
   –progress failed with exit code 128, no output"

   128 is a masking error and this error needs further triage as it can be a network issue at your end or a genuine issue accessing
   Qualcomm or upstream mirrors. As a workaround for this error, see :ref:`BitBake Fetcher Error <do_fetch_error_1>`. You can triage it further by following the subsequent instructions to dump verbose logs during fetch.

   By default, verbose logging isn't enabled for Yocto git fetch. Enabling git verbose logging for all recipes can significantly increase the build time. It's recommended to enable it only in required recipes on a need basis. To enable the same for all git projects, edit the ``local.conf`` file and change the ``BB_GIT_VERBOSE_FETCH`` value to **1**. You can also enable verbose logging for each recipe. For example, to enable verbose logging and debug a ``do_fetch()`` failure in a diag recipe, perform the following steps:

   1. Edit ``layers/meta-qcom-hwe/recipes-bsp/diag/daig_15.0.bb`` and
      add the line ``BB_GIT_VERBOSE_FETCH = "1"``.
   2. Clean the earlier downloaded artifacts using
      ``bitbake -fc cleanall diag``.
   3. Fetch the source again using ``bitbake -fc fetch diag``.
   4. Fetch the log with verbose logging available under the diag
      recipe’s working directory
      ``build-qcom-wayland/tmp-glibc/work/qcm6490-qcom-linux/diag/15.0-r0/temp``.
   5. Share ``log.do_fetch`` from this path with the Qualcomm support team. 
      
-  "Failed SP Download with error: <> Sp Download failed. ExitCode: 128 Signal: 0 with errorcode 4"

   **Error excerpt**

   .. container:: nohighlight
      
      ::

         warning: redirecting to https://git-na-ssl.chipcode.qti.qualcomm.com/57f0ec058e47f7a82b2de7b95111c74a/qualcomm/qualcomm-linux-spf-2-0_ap_standard_oem_nomodem.git/
         remote: Counting objects: 129803, done.
         remote: Compressing objects: 100% (114948/114948), done.
         fatal: write error: No space left on device5 GiB | 1.63 MiB/s
         fatal: fetch-pack: invalid index-pack output
         2024-03-02T14:32:18: Sync Command Failed
         [Error]: Failed SP Download with error: 2024-03-02T14:32:18: Sp Download failed. ExitCode: 128 Signal: 0 with errorcode 4
         [Error]: 2024-03-02T14:32:18: Sp Download failed. ExitCode: 128 Signal: 0

   **Solution**

   Error log indicates that there is no space on the device "fatal:
   write error: No space left on device".

   Clean up the space and retrigger.

-  "File “/usr/lib/python3.10/locale.py”, line 620, in setlocale return \_setlocale(category, locale)locale.Error: unsupported locale setting"

   To resolve this error, run the following commands and recompile:

   .. container:: nohighlight
      
      ::

         sudo locale-gen en_US.UTF-8
         sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
         export LC_ALL=en_US.UTF-8
         export LANG=en_US.UTF-8

-  "pyinotify.WatchManagerError: No space left on device (ENOSPC)"

   Compilation triggers this error.

   **Solution**

   Run the following commands:

   .. container:: nohighlight
      
      ::

         sudo su
         echo 1048576 > /proc/sys/fs/inotify/max_user_watches

Flash
-----------

No known errors.
