.. _concept_rr1_5dn_w1c:

Install QSC using a GUI
-----------------------------

.. note:: QSC Launcher supports x86 architecture only.

1. Use your Qualcomm ID to log in to
   https://softwarecenter.qualcomm.com, then click **Download Software Center**.

   .. image:: ../../media/k2c-qli-build-ga/gui.png

   The QSC Debian package (``.deb``) is downloaded to your machine.

2. Install the downloaded QSC Debian package using the Ubuntu GNOME GUI (GDebi) or run the following command:

   :: 

      sudo dpkg -i <installer.deb>

   To install GDebi, run the following command in a terminal window:

   ::

      sudo apt install gdebi

3. Right-click on the downloaded QSC Debian file and click **Open with GDebi Package Installer**.

   .. image:: ../../media/k2c-qli-build-ga/open_with_gdebi.png

4. Click **Install Package**.

   .. image:: ../../media/k2c-qli-build-ga/install_package.png

5. Enter the password for the current user. In the following image, the current user is Ubuntu.

   .. image:: ../../media/k2c-qli-build-ga/current_user.png

6. Accept the license agreement from within GDebi. Click **Next** on the first license agreement screen.

   .. image:: ../../media/k2c-qli-build-ga/lic_agreement_next.png

7. Click **I ACCEPT**.

   .. image:: ../../media/k2c-qli-build-ga/accept.png

   Wait for the installation to complete.

   .. image:: ../../media/k2c-qli-build-ga/installation_finished.png

9. Click **Show Applications** to find the Qualcomm Software Center.

   .. image:: ../../media/k2c-qli-build-ga/show_apps.png

