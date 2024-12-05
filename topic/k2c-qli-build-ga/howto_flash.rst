.. _howto_flash:

Flash
------------

.. _ufs_provisioning:

Provision UFS
^^^^^^^^^^^^^^^^^

Universal Flash Storage (UFS) provisioning helps to divide the storage into multiple LUNS allowing different types of data to be stored separately. This improves access efficiency and system organization.

.. note::
    - This procedure is available for registered users only.
    - UFS is provisioned by default. If there are any changes in LUNs, UFS re-provisioning must be done again. To download the provision XML file and to check the applicability of UFS provisioning for different SoCs, see *UFS Provisioning* table in `Release Specific Information <https://docs.qualcomm.com/bundle/publicresource/topics/RNO-240929204440/ReleaseNote.html#release-specific-information>`__.

**Prerequisites**

* :ref:`Install QSC CLI <one_time_host_setup>`.
* Install PCAT and QUD on the host machine using qpm-cli:

  ::

    qpm-cli --login
    qpm-cli --install quts --activate-default-license
    qpm-cli --install qud --activate-default-license
    qpm-cli --install pcat --activate-default-license

.. note::
   
   - The ``qpm-cli --help`` command lists the help options.
   - For Ubuntu 22.04, you might encounter an issue when installing QUD. To ensure a successful installation, you may need to enroll the public key on your Linux host. For additional details, follow the steps outlined in the ``signReadme.txt`` file available at ``/opt/QCT/sign/``.

**Procedure**

1. Verify whether PCAT can detect the device in EDL mode:

   ::

      PCAT -DEVICES
   
   **Sample output**

   ::

      Searching devices in Device Manager, please wait for a moment…
      ID | DEVICE TYPE | DEVICE STATE | SERIAL NUMBER | ADB SERIAL NUMBER | DESCRIPTION
      NA | NA          | EDL          | BE116704      | be116704          | Qualcomm USB Composite Device:QUSB_BULK_CID:042F_SN:BE116704

#. Provision UFS:

   ::

      PCAT -PLUGIN SD -DEVICE [PCAT SERIAL NUMBER] -DEVICEPROG [DEVICE PROGRAMMER] - MEMORYTYPE UFS -UFSPROV TRUE -UFSPROVXML [UFS PROVISION XML]
   
   For example:

   ::

      PCAT -PLUGIN SD -DEVICE BE116704 -DEVICEPROG <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image/prog_firehose_ddr.elf -MEMORYTYPE UFS -UFSPROV TRUE -UFSPROVXML <downloaded_provision_XML_file>

.. _flash_CDT:

Flash Qualcomm configuration data table (CDT) with QDL
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For the Qualcomm hardware, the source of platform information on a device is the CDT, which is a blob that contains information about the current platform and its subvariants. You can update CDT by flashing a CDT binary:

1. Download the CDT binary.

   -  For the Core Kit, download the following file:

      ::

         wget https://artifacts.codelinaro.org/artifactory/codelinaro-le/qcom-device-config/CORE_KIT_CDT.bin

   -  For the Vision Kit, download the following file:

      ::

         wget https://artifacts.codelinaro.org/artifactory/codelinaro-le/qcom-device-config/VISION_KIT_CDT.bin

   -  For the IQ-9100 Kit, download the following file:

      ::

         wget https://codelinaro.jfrog.io/artifactory/codelinaro-le/qcom-device-config/QCS9100/IQ-9100_KIT_CDT.bin

2. Use the CDT binary.

   Copy the downloaded CDT binary to a compiled build location. The
   following is a sample command:

   ::

      cp <the downloaded CDT bin file> <workspace_path>/build-qcom-wayland/tmp-glibc/deploy/images/qcs6490-rb3gen2-vision-kit/qcom-multimedia-image/

   .. note:: ``<workspace_path>`` is the directory where you created the Yocto build.

3. Update the CDT section in ``rawprogram3.xml`` with the intended CDT filename. The following is a sample command:

   ::

      <program SECTOR_SIZE_IN_BYTES="4096" file_sector_offset="0" filename="CORE_KIT_CDT.bin" label="cdt" num_partition_sectors="32" partofsingleimage="false" physical_partition_number="3" readbackverify="false" size_in_KB="128.0" sparse="false" start_byte_hex="0x20000" start_sector="32"/>

Continue with the flashing instructions.