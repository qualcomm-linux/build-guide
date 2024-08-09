.. _one_time_host_setup:

Install QSC CLI
===============

Set up ``qsc-cli``:

1. Install curl (if not already installed):

   ::

      sudo apt install curl

2. Download the Debian package for ``qsc-cli``:

   ::

      cd <workspace_path>
      curl -L https://softwarecenter.qualcomm.com/api/download/software/qsc/linux/latest.deb -o qsc_installer.deb

3. Install the ``qsc-cli`` Debian package:

   ::

      sudo dpkg -i qsc_installer.deb

4. Log in to ``qsc-cli``:

   ::

      qsc-cli login -u <username>

**Note:** A one-time login into
`chipcode.qti.qualcomm.com <http://chipcode.qti.qualcomm.com/>`__ is
required to download Qualcomm proprietary git repositories. Use your
Qualcomm login credentials to complete this step.

For more details, see ``qsc-cli`` related topics in `How
to <how_to.rst>`__.


