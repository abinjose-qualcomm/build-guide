.. _install_qsc_cli:

Install QSC CLI
--------------------

1. Install curl (if not installed already):

   .. container:: nohighlight
      
      ::

         sudo apt install curl

2. Download the Debian package for ``qsc-cli``:

   .. container:: nohighlight
      
      ::

         cd <workspace_path>
         # For x86
         curl -L https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Software_Center/Linux/Debian/latest.deb -o qsc_installer.deb
         # For ARM64
         curl -L https://softwarecenter.qualcomm.com/api/download/software/tools/Qualcomm_Software_Center/Linux/ARM64/Debian/latest.deb -o qsc_installer.deb

3. Install the ``qsc-cli`` Debian package:

   .. container:: nohighlight
      
      ::

         sudo apt update
         sudo apt install ./qsc_installer.deb

4. Sign in to ``qsc-cli`` using your registered email ID:

   .. note:: To register with Qualcomm, go to https://www.qualcomm.com/support/working-with-qualcomm.

   .. container:: nohighlight
      
      ::

         qsc-cli login -u <username>

For more information about ``qsc-cli``, see :ref:`How to Sync <howto_sync>`.