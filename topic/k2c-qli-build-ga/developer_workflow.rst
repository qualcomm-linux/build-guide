.. _developer_workflow:

Developer workflow
--------------------------

Migrate from the earlier release to the next release
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The migration process depends on the development, branching, and integration workflows at your end. However, perform the following steps:

1. Integrate changes for the sources that you have branched:

   a. Skip this step if you haven't forked any underlying source code
      used by Qualcomm Yocto layers and are applying only ``.patch``
      files.
   #. Qualcomm provides git history to all the source repositories.
      You can see a reference list of repositories in the `Release
      Notes <https://docs.qualcomm.com/doc/80-80020-300/>`__.
      For the Qualcomm repositories branched and modified at your
      end, perform the following steps:

      i. Compare the Qualcomm Yocto layers with your Yocto layers, and
         identify the repositories you must migrate.
      #. Check the Qualcomm Yocto layer recipes and identify the SRC_URI
         updates.
      #. Clone these repositories using ``git clone`` and check out the
         appropriate branch and SHA as pointed by the SRC_URI.
      #. Use ``git merge`` or the appropriate mechanism to bring the delta
         to your own fork according to your need.
      #. Update your recipes as required.

2. Integrate the Yocto layer recipes – ``git merge`` the Yocto layer to
   pick up any changes.
3. Build and validate your resulting workspace.
4. Proceed with the next steps in your internal workflows.

Build a Qualcomm Linux kernel
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `building kernel image <https://docs.qualcomm.com/bundle/publicresource/topics/80-80020-3/getting_started_chapter2.html#build-the-device-image>`__.

Customize Qualcomm Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

See `User
customizations <https://docs.qualcomm.com/bundle/publicresource/topics/80-80020-27/customize_qualcomm_linux.html>`__.
