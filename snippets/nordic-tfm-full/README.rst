.. _snippet-nordic-tfm-full:

Nordic full TF-M Snippet (nordic-tfm-full)
##########################################

.. code-block:: console

   west build -S nordic-tfm-full [...]

Overview
********

Nordic nRF91 boards default to the minimal, bootloader-less Trusted Firmware-M
(TF-M) profile (:kconfig:option:`CONFIG_TFM_PROFILE_TYPE_MINIMAL`) with BL2
disabled, so that generic Zephyr samples fit into the non-secure application
area and boot without any additional overlays.

Some samples, such as those under :zephyr_file:`samples/tfm_integration`, need a
full TF-M with all secure services (crypto, protected storage, internal trusted
storage, platform) and the BL2 bootloader. This snippet restores that
configuration:

* It selects the base TF-M profile
  (:kconfig:option:`CONFIG_TFM_PROFILE_TYPE_NOT_SET`) and re-enables
  :kconfig:option:`CONFIG_TFM_BL2`, so all secure partitions and the bootloader
  are built again.
* On nRF91 non-secure targets it applies a device tree overlay with the classic
  TF-M flash and SRAM layout: a 64 KB BL2 partition, a 256 KB secure image, two
  update slots, and the protected storage / internal trusted storage / OTP data
  partitions.

Requirements
************

The device tree overlay is only applied for nRF91 non-secure board targets
(``*/nrf91xx/ns``). On other targets the snippet only changes the TF-M profile
and BL2 Kconfig options.
