.. _troubleshoot_sync_build_and_flash:

*********************
Troubleshooting
*********************

.. _section_hkm_2dc_p1c_vinayjk_02-29-24-1641-18-155:

Docker
------

-  **docker: Cannot connect to the Docker daemon at
   unix:///var/run/docker.sock. Is the docker daemon running?**

   Run the following command to start Docker:

   ::

      sudo systemctl start docker

-  **Error response from daemon: Get “https://registry-1.docker.io/v2/”:
   http: server gave HTTP response to HTTPS client**

   Add an internal Docker registry mirror (internal setting for Qualcomm
   network).

   **Note:**

   Do not include # comments in the JSON configuration file.

   Using a tab instead of space and other invisible whitespace
   characters may break the proper work of JSON configuration files and
   may lead to ``docker.service`` failing to start.

   ::

      sudo vim /etc/docker/daemon.json
      # Add an entry similar to the following in /etc/docker/daemon.json:
      {
              "registry-mirrors": ["https://docker-registry.qualcomm.com"]
      }

   Restart the Docker service to take the new settings.

   ::

      sudo systemctl restart docker

-  **Docker failure due to Virtualization not enabled**

   This error can be resolved by enabling virtualization from the BIOS.
   Follow the specific instructions from the system provider to enable
   the virtualization. For example, the following steps can enable
   virtualization provided by a system provider:

   1. When the system is booting up, step into the BIOS. The **BIOS**
      window is displayed.
   2. Switch to the **Advanced** tab.
   3. In the **CPU Configuration** section, set **Virtualization
      Technology** to enabled.
   4. Save and exit.
   5. Restart the system.

-  **Permission denied while trying to connect to the Docker daemon
   socket at unix:///var/run/docker.sock**

   This happens when ``qsc-cli`` is already installed on the machine and
   the user is not part of the Docker group:

   ::

      sudo usermod -aG docker $USER
      newgrp docker

   To confirm that you are part of the Docker group, run the following
   command:

   ::

      sudo grep /etc/group -e "docker"
      # This command shows a list of users who are part of the Docker group; must include your user ID

   Log out and log in again for the access to take effect.

   ::

      # You can run the following command to check if you are part of the Docker group
      id -a
      # This command returns an output string which should include 'docker'

.. _section_w42_4gc_p1c_vinayjk_02-29-24-1706-59-554:

Sync
----

-  **repo init or sync failure with except ManifestInvalidRevisionError,
   e:**

   You might encounter this issue after installing the Repo package:

   -  If you have redirection in your ``/etc/gitconfig`` or
      ``~/.gitconfig`` to an internal mirror.
   -  If your internal mirror has prefix to branches while mirroring.
      For example, if ``/etc/gitconfig`` redirects and the internal
      mirror has a stable branch from upstream git mirrored as
      ``aosp/stable``, then the following error appears while performing
      ``repo init``:

   ::

      [url "ssh://<internal mirror>:<port>/tools/repo"]
        insteadOf = https://android.googlesource.com/tools/repo
        insteadOf = https://gerrit.googlesource.com/git-repo

   ::

      File "/local/mnt/workspace/<userid>/test_repo/.repo/repo/main.py", line 126
          except ManifestInvalidRevisionError, e:
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      SyntaxError: multiple exception types must be parenthesized

   The steps to resolve this error are as follows:

   ::

      # Remove the older .repo folder. This will be in the directory where you ran 'repo init' command earlier
      rm -rf .repo

      # Export and run the repo commands to fix the repo issues. The REPO_REV must point to the mirrored
      # branch from upstream 'stable' branch of https://gerrit.googlesource.com/git-repo
      export REPO_REV='aosp/stable'

-  **Install repo “Server certificate verification failed”**

   If you see a certificate error such as ‘Server certificate
   verification failed. CAfile: none CRLfile: none’, configure git to
   disable SSL certificate verification with git configuration. Discuss
   with your IT administration for further guidance. You can use either
   of the following commands to disable SSL:

   ::

      export GIT_SSL_NO_VERIFY=1
      git config --global http.sslverify false

   If your region is blocking access to ``android.googlesource``, try
   the following configuration to fetch Repo from Codelinaro Mirror:

   ::

      git config --global url.https://git.codelinaro.org/clo/la/tools/repo.insteadOf https://android.googlesource.com/tools/repo

-  **error.GitError: git config (‘–replace-all’, ‘color.ui’, ‘auto’):
   error: could not write config file /home/$USER/.gitconfig: Device or
   resource busy**

   This error occurs when your gitconfig does not set the UI color
   configuration. This configuration is set by default in Git 1.8.4 and
   later versions. Run the following command to enable the color display
   in your account:

   ::

      git config --global color.ui auto

-  **[Error]: Failed preparing build for compilation. Error: Error
   setting docker credentials. Error: “Error saving credentials: error
   closing temp file: close
   /usr2/<userid>/.docker/config.json3322274803: disk quota
   exceeded\\n”**

   QSC CLI uses the home directory only for a few kilo bytes (kB). Clear
   a few MBs from your home directory.

-  **[Error]: The “path” argument must be of type string. Received
   undefined**

   **Error excerpt**

   ::

      qsc-cli download --workspace-path '/local/mnt/workspace/<userid>/K2L/QSC_CLI_BUILD/build' --product 'QCM6490.LE.1.0' --build 'QCM6490.LE.1.0-00134-STD.PROD-1' --distribution 'Qualcomm_Linux.SPF.1.0|TEST|DEVICE|PUBLIC'
      [Info]: Starting qsc-cli version 0.0.0.7 
      (node:2924765) ExperimentalWarning: The Fetch API is an experimental feature. This feature could change at any time
      (Use `qsc-cli --trace-warnings ...` to show where the warning was created)
      [Info]: Checking if Workspace already exists
      [Info]: Saved updated Workspace info
      [Info]: Workspace Setup Completed
      [Error]: The "path" argument must be of type string. Received undefined

   **Solution**

   This error occurs because QSC-CLI is incompatible with Qlauncher.
   Qlauncher is going to be deprecated and replaced with a new
   application from the QSC. If you have Qlauncher in the workspace, you
   can run the following commands:

   ::

      # Find your workspace within the Qlauncher UI
      # Take a backup of the following metadata file if you want to preserve the older workspace created with Qlauncher.
      # These will work only with Qlauncher app. You can reinstall the app at a later time again to access. If you do not 
      # need the workspaces, you can delete this file using:
      mv /var/lib/qcom/data/qualcomm_launcher/workspaces2.json /var/lib/qcom/data/qualcomm_launcher/workspaces2.json.bak
      # Uninstall Qlauncher with the following command:
      qpm-cli --uninstall qualcomm_launcher

-  **docker: Error response from daemon: error while creating mount
   source path ‘/usr2/<userid>/.netrc’: mkdir /usr2/<userid>/.netrc:
   permission denied**

   **Error excerpt**

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

   This could happen due to the way IT has set up your home directory.
   Work with your IT administrator for any further changes to your home
   directory.

-  **fatal: couldn’t find remote ref refs/heads/qcom-linuxSTXkirkstone**

   If you see any junk characters while copying commands from the PDF,
   remove or replace the junk characters with appropriate symbols and
   rerun the command. Alternately, you can open the guide in HTML mode
   and use the copy command option.

   **Example**

   ::

      # Replace the following command
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linuxSTXkirkstone -m qcom-6.6.28-QLI.1.1-Ver.1.1.xml
      # with
      repo init -u https://github.com/quic-yocto/qcom-manifest -b qcom-linux-kirkstone -m qcom-6.6.28-QLI.1.1-Ver.1.1.xml

.. _section_ays_4gc_p1c_vinayjk_02-29-24-1707-9-256:

Build
-----

-  **ERROR: linux-kernel-qcom-6.6-r0 do_menuconfig: No valid terminal
   found, unable to open devshell**

   This error might be triggered while running a
   ``bitbake linux-kernel-qcom -c menuconfig`` command.

   **Error excerpt**

   ::

      ERROR: linux-kernel-qcom-6.6-r0 do_menuconfig: No valid terminal found, unable to open devshell.
      Tried the following commands:
              tmux split-window -c "{cwd}" "do_terminal"
              tmux new-window -c "{cwd}" -n "linux-kernel-qcom Configuration" "do_terminal"
              xfce4-terminal -T "linux-kernel-qcom Configuration" -e "do_terminal"
              terminology -T="linux-kernel-qcom Configuration" -e do_terminal

   **Solution**

   ::

      sudo apt install screen
      sudo apt install tmux

-  **NOTE: No reply from server in 30s**

   If you are seeing this error during the build on the rerun of
   ``qsc-cli compile`` or ``bitbake`` commands, you can try to delete
   ``bitbake.lock``, ``bitbake.sock``, and ``hashserve.lock`` from your
   partially built workspace and retry the build. For example, if you
   are building with ``qsc-cli``, then these files are found under
   ``<absoute_workspace_path>/DEV/LE.QCLINUX.1.0.r1/build-qcom-wayland``.

-  **do_fetch: BitBake Fetcher Error: FetchError(‘Unable to fetch URL
   from any source’)**

   These are intermittent fetch failures. Check if there is a
   network/host issue at your end, else it might be the server creating
   this issue. You could increase ``postBuffer`` and
   ``maxRequestBuffer`` settings in your ``.gitconfig`` file if the
   errors occur while fetching the git repositories. If you are using
   ``qsc-cli``, then these configurations are already taken care by the
   ``qsc-cli`` tool:

   ::

      git config --global http.postBuffer 1048576000
      git config --global http.maxRequestBuffer 1048576000

   If these configurations do not work, you can retry the compile to get
   past these intermittent errors for the first time.

   A few large git projects may show this error. For such projects, a
   feasible option is to manually clone as follows:

   ::

      cd <workspace_path>/downloads/git2/
      git clone --bare --mirror https://<url>/<project-name>.git <workspace_path>/downloads/git2/<local-name>.git
      touch <workspace_path>/downloads/git2/<local-name>.git.done

   For example, when ``do_fetch`` fails for
   ``qualcomm_linux-spf-1-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git``,
   run the following command:

   ::

      git clone --bare --mirror https://qpm-git.qualcomm.com/home2/git/revision-history/qualcomm_linux-spf-1-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git <workspace_path>/downloads/git2/qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-1-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git
      touch <workspace_path>/downloads/git2/qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-1-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git.done

   After creating the ``.done`` file, proceed with the
   ``bitbake <image-recipe>`` command. After the first build completes,
   it is recommended to set up your own `download
   directory <https://docs.yoctoproject.org/4.0.16/singleindex.html#term-DL_DIR>`__.

-  **make[4]: /bin/sh: Argument list too long**

   This happens when the path of the workspace exceeds 90 characters.
   Reduce the workspace path length to avoid this failure.

-  **kernel-source/arch/arm64/boot/dts/qcom/qcm6490-idp.dts:8:10: fatal
   error: dt-bindings/iio/qcom,spmi-adc7-pmk8350.h: No such file or
   directory**

   The file in question ``qcom,spmi-adc7-pmk8350.h`` is part of the
   kernel source
   ``<kernel-src>/include/dt-bindings/iio/qcom,spmi-adc7-pmk8350.h``.

   Check the workspace for this file and ensure that the environment is
   properly initialized to pick this file. While compiling dtbs, the
   kernel build system runs a GCC preprocessor to replace the macros in
   dts files by its definition. The mentioned path is one such place
   where several ``includes`` reside.

   Check if you have ``core.symlinks`` set to ``false`` in your git
   configuration. If yes, set it to true:

   ::

      git config --global core.symlinks true

-  **qpm-git.qualcomm.com.home2.git.revision-history.qualcomm_linux-spf-1-0-le-qclinux-1-0-r1_api-linux_history_prebuilts.git
   –progress failed with exit code 128, no output**

   128 is a masking error and this error needs further triage as it
   could be a network issue at your end or a genuine issue accessing
   Qualcomm or upstream mirrors. As a workaround for this error, see
   `do_fetch: BitBake Fetcher Error: FetchError(‘Unable to fetch URL
   from any source’) <#do_fetch_error>`__. You can triage it further by
   following the subsequent instructions to dump verbose logs during
   fetch.

   By default, verbose logging is not enabled for Yocto git fetch. To
   enable the same for all git projects, edit ``local.conf`` file and
   change ``BB_GIT_VERBOSE_FETCH`` value to **1**. Verbose logging can
   also be enabled for each recipe. For example, to enable verbose
   logging and debug a ``do_fetch()`` failure in a diag recipe, perform
   the following steps:

   1. Edit ``layers/meta-qcom-hwe/recipes-bsp/diag/daig_15.0.bb`` and
      add the line ``BB_GIT_VERBOSE_FETCH = "1"``.
   2. Clean the previous downloaded artifacts using
      ``bitbake -fc cleanall diag``.
   3. Fetch the source again using ``bitbake -fc fetch diag``.
   4. Fetch the log with verbose logging available under the diag
      recipe’s working directory
      ``build-qcom-wayland/tmp-glibc/work/qcm6490-qcom-linux/diag/15.0-r0/temp``.
   5. Share ``log.do_fetch`` from this path with the Qualcomm CE point
      of contact. **Note:** Enabling git verbose logging for all recipes
      can significantly increase the build time. It is recommended to
      enable it only in required recipes on a need basis.

-  **Failed SP Download with error: <> Sp Download failed. ExitCode: 128
   Signal: 0 with errorcode 4**

   **Error excerpt**

   ::

      warning: redirecting to https://git-na-ssl.chipcode.qti.qualcomm.com/57f0ec058e47f7a82b2de7b95111c74a/qualcomm/qualcomm-linux-spf-1-0_ap_standard_oem_nomodem.git/
      remote: Counting objects: 129803, done.
      remote: Compressing objects: 100% (114948/114948), done.
      fatal: write error: No space left on device5 GiB | 1.63 MiB/s
      fatal: fetch-pack: invalid index-pack output
      2024-03-02T14:32:18: Sync Command Failed
      [Error]: Failed SP Download with error: 2024-03-02T14:32:18: Sp Download failed. ExitCode: 128 Signal: 0 with errorcode 4
      [Error]: 2024-03-02T14:32:18: Sp Download failed. ExitCode: 128 Signal: 0

   **Solution**

   Error log indicates that there is no space on the device **“fatal:
   write error: No space left on device”**.

   Clean up the space and retrigger.

-  **File “/usr/lib/python3.10/locale.py”, line 620, in setlocale return
   \_setlocale(category, locale)locale.Error: unsupported locale
   setting**

   To resolve this error, run the following commands and recompile:

   ::

      sudo locale-gen en_US.UTF-8
      sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
      export LC_ALL=en_US.UTF-8
      export LANG=en_US.UTF-8

-  **layer directories do not exist
   build-qcom-wayland/conf/../../layers/meta-qcom-qim-product-sdk**

   This error occurs due to one of the following reasons:

   -  Git clone of ``meta-qcom-qim-product-sdk`` did not complete
      successfully.
   -  ``meta-qcom-qim-product-sdk`` layer is not exported to
      EXTRALAYERS. **Error excerpt**

   ::

      xxxx@xxxx:~/github_un/build-qcom-wayland$ bitbake qcom-multimedia-image
      ERROR: The following layer directories do not exist:
      ERROR: <workspace_path>/build-qcom-wayland/conf/../../layers/meta-qcom-qim-product-sdk
      ERROR: Please check BBLAYERS in <workspace_path>/build-qcom-wayland/conf/bblayers.conf

   **Solution**

   -  Remove the ``build-qcom-wayland`` directory.
   -  Rerun the commands in `Build QIMP SDK
      image <build_with_standalone_commands_unreg.rst#section_lrb_1nd_fbc>`__.

-  **failed: database disk image is malformed. abort()ing pseudo client
   by server request**

   The Pseudo tool encounters path mismatch and corrupt database issues
   when processing file system operations. When Pseudo simulates file
   system operations in a Yocto project, problems might occur in the
   process of handling file paths and permissions.

   This is a known issue in the `Yocto
   community <https://wiki.yoctoproject.org/wiki/Pseudo_Abort>`__.

   **Solution**

   Run the following commands:

   ::

      rm -rf <workspace_path>/build-qcom-robotics-ros2-humble/tmp-glibc
      bitbake -c cleanall pseudo-native & bitbake pseudo-native
      ../qirp-build qcom-robotics-full-image

.. _section_uwl_lhc_p1c_vinayjk_02-29-24-1713-48-740:

Flash
-----

No known errors.
