.. _concept_n2t_tjn_w1c:

Install PCAT and QUD
-------------------------

For the Launcher workflow to detect the connected devices and flash the software builds, ensure that the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) are installed on the host machine. Click **PCAT** to install PCAT and **QUD** to install QUD as shown in the following image:

.. image:: ../../media/k2c-qli-build-ga/QSC_has_PCAT_QUD_install_info.png

**Or**

Install PCAT and QUD using ``qpm-cli``:

::

   qpm-cli --login
   qpm-cli --install pcat --activate-default-license
   qpm-cli --install qud --activate-default-license

The ``qpm-cli --help`` command lists the help options.

For Ubuntu 22.04, you may encounter an issue while installing QUD where you are asked to enroll the public key on your Linux host for a successful QUD installation. For more information, follow the steps provided in the ``signReadme.txt`` file available at the ``/opt/QUIC/sign/`` directory.