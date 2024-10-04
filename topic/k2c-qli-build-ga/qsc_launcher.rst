.. _concept_n2t_tjn_w1c:

Use QSC Launcher
-----------------

.. _launcher_download_sw:

Software download
^^^^^^^^^^^^^^^^^^^

1. To open the QSC Launcher desktop application, launch **Qualcomm Software Center** from the **Applications** menu.

2. Use your registered email ID to log in to the QSC desktop application. A dashboard page appears.

   .. image:: ../../media/k2c-qli-build-ga/start_launcher_ab.png

   -  If you do not have a connected device, click **Start Launcher** (A) on the top panel.
   -  If you have a connected device, click **Start Launcher** (B) for the appropriate device in the **Connected devices** panel.

3. On the Specify Environment page, select the appropriate values for **Category**, **Chipset**, **Host Operating System**, and **Target Operating System**.

   .. note:: For more information on the supported chip products, see `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

  .. image:: ../../media/k2c-qli-build-ga/specify_env.png

4. Click **Next**. The **Select Resources** page appears.
   
   .. image:: ../../media/k2c-qli-build-ga/select_resource_page.png

   a. In the **Base Workspace Path** text box, specify a directory where you want to download the software.

   b. Select the **Software Product**.

   c. Select the **Distribution** and the **Release Tag**.

      .. note::
         
         - For information on the supported distributions for your chip product, see the *Access Controlled Distribution* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
         - For information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#id7>`__.         
         - For information on the QIMP and QIRP SDKs, see the following guides:

           - `QIMP SDK Quick Start Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-51>`__
           - `QIRP SDK 2.0 User Guide <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-265>`__

5. Click **Download** to download the selected compilable distribution or flashable binary. After the software is downloaded succesfully, *Download complete* status appears.

   .. image:: ../../media/k2c-qli-build-ga/software_download_complete.png

   .. note:: Download progress is also available in the top menu bar **Downloads** option.

   Flashable binaries do not require compilation. If a flashable binary is selected, follow the onscreen prompts to flash to a connected device:

   .. image:: ../../media/k2c-qli-build-ga/prebuilt_compile.png

.. _launcher_build_default_config:

Build and flash default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _launcher_step1:

1. Compile the default build.

   .. note:: For information on the default configurations, see the *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

.. _launcher_compile_step:

   a. After the download is complete, select **Compile** to start compiling (depending on the size of the downloaded software and host machine configuration, the compilation process may take a few hours).

      .. image:: ../../media/k2c-qli-build-ga/download.png

   b. To view the compilation progress, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png

   .. note:: BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 1a <launcher_compile_step>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

2. Flash with the default configuration.

   .. note::
      - If your reference kit matches the configuration compiled in :ref:`step 1 <launcher_step1>`, then continue with the remaining steps in this procedure. Else, `build your own configuration <launcher_build_own_config>`.
      - Before you flash the software, ensure that the device is in Emergency Download (EDL) mode. For more information on how to force the device into EDL mode, see :ref:`Move to EDL mode <section_vgg_mly_v1c>`.

   a. Flash the software by selecting the device on which you want to flash the compiled software. If multiple devices are connected, select the correct target device:

      .. image:: ../../media/k2c-qli-build-ga/flash.png

   b. Click **Flash on device**. The page is updated and displays a progress bar as Launcher begins flashing the software. Leave the device connected while the software is being flashed.

   c. To view the compilation progress of individual software images, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_logs.png

   d. When the process is finished, *Flash Complete* is displayed on the page.

      .. image:: ../../media/k2c-qli-build-ga/flash_launcher_flashComplete.png

   e. Click **Done**. To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.

.. _launcher_build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
1. :ref:`Compile the build for default machine configuration <launcher_build_default_config>`.

   .. note:: For information on the default configurations, see the *Default values of "MACHINE" and "QCOM_SELECTED_BSP" parameters for QSC* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

   a. :ref:`Download the software <launcher_download_sw>`.
   
   b. After the download is complete, select **Compile** to start compiling (depending on the size of the downloaded software and host machine configuration, the compilation process may take a few hours).

      .. image:: ../../media/k2c-qli-build-ga/download.png

   c. To view the compilation progress of individual software images, expand the logs panel.

      .. image:: ../../media/k2c-qli-build-ga/QSC_compile_progress.png

   .. note:: BitBake fetch errors are typically intermittent fetch failures. To resolve these errors, retry :ref:`step 1a <launcher_compile_step>`. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

2. Compile the `LE.QCLINUX.1.0.r1` image with your own MACHINE and QCOM_SELECTED_BSP.
   
   .. note:: For information on the supported configurations, see the *MACHINE and QCOM_SELECTED_BSP parameter value* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

   a. To execute the build commands for a specific configuration, click **Compile using terminal**:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_page.png

      .. note:: Compilations executed through the terminal are not tracked by the Qualcomm Software Center and the ability to monitor their status on the Download page is lost.      

         .. image:: ../../media/k2c-qli-build-ga/compile_terminal_dialog.png
      
   b. Click **Open Terminal**. A terminal window with expanded default build command appears.

   c. Update the highlighted command with your own configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for Qualcomm® RB3 Gen 2 Core Development Kit, update the ``MACHINE`` in the above Default Build Command to ``qcs6490-rb3gen2-core-kit`` after opening the terminal.
   
   d. After a successful build, check that the ``system.img`` is in the ``<Base Workspace Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory. For example:

      ::

         cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image
         ls -al system.img
   
   .. note::      
      The compilation of any individual software images except ``LE.QCLINUX.1.0.r1`` must be followed by compiling the software product and ``LE.QCLINUX.1.0.r1`` in the same order.
 
      For example, if ``BOOT.MXF.1.0.c1`` is compiled, ensure to compile the software product (for example, QCM6490.LE.1.0) and ``LE.QCLINUX.1.0.r1``.

3. :ref:`Flash your configuration <flash_images>`.

   .. note::
      - Build images path must be updated with the compiled build images workspace path ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image``.

        For example, ``<Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image``.

.. note::
   -  To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.
   -  The device reboots after the flashing procedure is completed successfully. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-253/ubuntu_host.html#sub$check_sw_version_uart>`__.