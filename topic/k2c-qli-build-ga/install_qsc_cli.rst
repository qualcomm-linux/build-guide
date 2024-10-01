.. _one_time_host_setup:

Install QSC CLI
--------------------

.. note:: QSC CLI supports x86 architecture only.

1. Install curl (if not installed already):

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

.. note:: For more information, see ``qsc-cli`` related topics in :doc:`How to Sync <howto_sync>`.