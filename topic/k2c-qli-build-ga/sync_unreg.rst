Sync
-----

QLI uses the kas tool that was installed during the :ref:`Ubuntu host setup <ubuntu_host_setup_github_unreg>` to download the dependency metalayer repositories for the build. Lock files recording the metalayer repository information are stored in `meta-qcom-releases <https://github.com/qualcomm-linux/meta-qcom-releases>`__ for each critical release. 

You can checkout the lock files for each release by the `meta-qcom-release-tag`. The meta-qcom release tag syntax is as follows:

``qcom-<Linux LTS Kernel Version>-QLI.<version>-Ver.<release>``
  
For example, the meta-qcom release tag ``qcom-6.18-QLI.2.0-Ver.1.0`` denotes the following:
   
- 6.18: Qualcomm Linux kernel
- QLI.2.0: Qualcomm Linux v2.0
- 1.0: Milestone release
