.. _sync_build_flash:

Use QSC CLI
--------------

This section explains how to configure, download, compile, and flash Qualcomm Linux using the QSC CLI.

.. _qsc_cli_software_download:

Software download
^^^^^^^^^^^^^^^^^^^

-  Download a software release by specifying the absolute workspace path, product ID, distribution, and release ID as shown in the following example:

   ::

      qsc-cli download --workspace-path '<Base_Workspace_Path>' --product '<Product_ID>' --distribution '<Distribution>' --release '<Release_ID>'
      # Example, qsc-cli download --workspace-path '/local/mnt/workspace/sample_workspace' --product 'QCM6490.LE.1.0' --distribution 'Qualcomm_Linux.SPF.1.0|AP|Standard|OEM|NoModem' --release 'r00263.4'

   .. note::
      - If you are downloading more than one distribution, create a new workspace for each distribution that you download.
      - For the Product_ID, Distribution, and Release_ID values, see the *QSC-CLI Input Parameters* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
      - For more information on the Yocto layers, see `Qualcomm Linux metadata layers and descriptions <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-27/platform_software_features.html#id7>`__.

.. _section_yhy_11w_q1c_vinayjk_03-07-24-006-28-270:

Build default configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. _compile_qsc_cli:

Compile
''''''''

.. note:: For information on the default configurations, see the *Default values of MACHINE and QCOM_SELECTED_BSP parameters for QSC* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

Start the compilation after the download is complete:

.. note:: Depending on the size of the software and host machine configuration, the compilation process may take a few hours.

::

   qsc-cli compile --workspace-path '<Base_Workspace_Path>'
    
   # Example
   qsc-cli compile --workspace-path '/local/mnt/workspace/sample_workspace'

This process builds the necessary Qualcomm firmware and also completes the Qualcomm Linux build.

.. note:: If you encounter a BitBake fetcher error, try recompiling to resolve the issue. If the issue persists, see :ref:`BitBake Fetcher Error <do_fetch_error_1>` for a solution.

.. _section_qb2_sp1_q1c_vinayjk_03-04-24-129-15-322:

Recompile
'''''''''''

To recompile after any modifications to the software release, use your existing workspace built using QSC CLI:

::

   # qsc-cli compile --image '<software_image_name>' --workspace-path '<Base_Workspace_Path>'
    
   # Example
   qsc-cli compile --image LE.QCLINUX.1.0.r1 --workspace-path '/local/mnt/workspace/sample_workspace'

.. note:: For information on software image names (``--image``), see the *QSC-CLI Input Parameters* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.

.. _section_x2k_vnf_w1c:

Flash
'''''''''

.. note:: For the QSC CLI to detect the connected devices and flash the software builds, ensure that the Qualcomm Product Configuration Assistant Tool (PCAT) and Qualcomm USB Driver (QUD) are installed on the host machine. Use ``qpm-cli`` to install PCAT and QUD:

::

  qpm-cli --login
  qpm-cli --install pcat --activate-default-license
  qpm-cli --install qud --activate-default-license

The ``qpm-cli --help`` command lists the help options.

When installing QUD on Ubuntu 22.04, you might have to enroll the public key on your Linux host to complete the installation. For more information, see the ``signReadme.txt`` file in the ``/opt/QTI/sign/`` directory.

.. note::
   - If your reference kit matches the :ref:`configuration compiled <compile_qsc_cli>`, then continue with the following steps. Else, skip to :ref:`Build your own configuration <build_own_config>`.
   - Before you flash the software, ensure that the device is in Emergency Download (EDL) mode. For more information on how to force the device into EDL mode, see :ref:`Move to EDL mode <section_vgg_mly_v1c>`.
  
1. Flash a device.

   ::

      qsc-cli flash --workspace-path <Base_Workspace_Path> --buildflavor "sa2150p_emmc" --serial <serial number>
      # Example:
      qsc-cli flash --workspace-path '/local/mnt/workspace/sample_workspace' --serial 'be116704'
   
   The ``--buildflavor`` argument is optional and only required for devices that have multiple flavors. To list the build flavors, run the following command:
      
   ::

      qsc-cli flash --workspace-path <workspace path> --list-buildflavor

   .. note::
      - To know the `<serial number>`, run the following command:

        ::
      
          pcat -devices

        **Sample output**
        
        .. container:: screenoutput

            Searching devices in Device Manager, please wait for a moment…
            ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
            NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

      - To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.

.. _build_own_config:

Build your own configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Compile the build for default machine configuration.

   a. :ref:`Download the software <qsc_cli_software_download>`.
   
   #. :ref:`Compile the default build <compile_qsc_cli>`.
   
2. Compile the ``LE.QCLINUX.1.0.r1`` image with your own MACHINE and QCOM_SELECTED_BSP.
   
   .. note:: For information on the supported configurations, see the *MACHINE and QCOM_SELECTED_BSP parameter value* table in the `Release Notes <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/>`__.
   
   a. Execute the build commands for a specific configuration:

      ::

         qsc-cli open-build-env --workspace-path <Base_Workspace_Path> --image <software_image_name>
         # Example
         qsc-cli open-build-env --workspace-path '/local/mnt/workspace/sample_workspace' --image 'LE.QCLINUX.1.0.r1' 

      This command opens the terminal.
   
      .. note:: An environment is setup to execute your own build commands for a given software image. QSC will not track the status of input workspaces in the future releases and flash using ``qsc-cli`` will not be supported for these workspaces.

   b. Update the highlighted command as per your own configuration and run it on the terminal:

      .. image:: ../../media/k2c-qli-build-ga/compile_terminal_new.png

      For example, to build for Qualcomm® RB3 Gen 2 Core Development Kit, update the ``MACHINE`` in the above Default Build Command to ``qcs6490-rb3gen2-core-kit``.
   
   c. After a successful build, check that the ``system.img`` is in the ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image`` directory with updated timestamp. For example:

      ::

         cd <Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image
         ls -al system.img
         
   .. note::      
       The compilation of any individual software images except ``LE.QCLINUX.1.0.r1`` must be followed by compiling the software product and ``LE.QCLINUX.1.0.r1`` in the same order.
 
       For example, if ``BOOT.MXF.1.0.c1`` is compiled, ensure to compile the software product (for example, QCM6490.LE.1.0) and ``LE.QCLINUX.1.0.r1``.
      
3. :ref:`Flash your configuration <flash_images>`.

   .. note:: 
      Build images path must be updated with the compiled build images workspace path ``<Base_Workspace_Path>/DEV/LE.QCLINUX.1.0.r1/build-<DISTRO>/tmp-glibc/deploy/images/<MACHINE>/qcom-multimedia-image``.
         
      For example, ``<Base Workspace Path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-core-kit/qcom-multimedia-image``.  

.. note::
   -  To connect to the device, see :ref:`How to SSH <section_hmw_vsh_p1c_vinayjk_03-01-24-1110-45-279>`.
   -  The device reboots after the flashing procedure is completed successfully. To verify the updated software version, see `Check software version <https://docs.qualcomm.com/bundle/publicresource/topics/80-70015-253/ubuntu_host.html#sub$check_sw_version_uart>`__.