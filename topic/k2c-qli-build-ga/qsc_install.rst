.. _concept_rr1_5dn_w1c:

Install QSC
---------------------

There are two methods to install QSC:

-  Using a GUI
-  Using a CLI

.. _section_mkf_b2n_w1c:

Install QSC using a GUI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Use your Qualcomm ID to log in to
   https://softwarecenter.qualcomm.com, then click **Download Software
   Center**. The QSC Debian package (``.deb``) downloads to your
   machine.

   .. image:: ../../media/k2c-qli-build-ga/qcs_launcher_gui.png

2. Install the downloaded QSC Debian package using GDebi package installer:

   .. image:: ../../media/k2c-qli-build-ga/deb_gui_installer.png

.. _section_hrf_d2n_w1c:

Install QSC using a CLI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Install curl (if not already installed):

   ::

      sudo apt install curl

2. Download the QSC Debian package:

   ::

      cd <workspace_path>
      curl -L https://softwarecenter.qualcomm.com/api/download/software/qsc/linux/latest.deb -o qsc_installer.deb

3. Install the Debian package:

   ::

      sudo dpkg -i qsc_installer.deb

   After a successful installation, the message
   ``Installed Qualcomm Software Center vX.X.X successfully`` is
   displayed.


