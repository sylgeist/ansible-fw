# Adding new NIC firmware
There are two categories for adding new NIC firmware:
- Adding FW support for a _new_ NIC for an _existing_ vendor already supported in FWU (easy)
- Adding FW support for a _new_ NIC for a _new_ vendor (less easy)

To add support for new NICs for existing vendors:
* Update `nic_firmware_data[$YOUR_EXISTING_VENDOR]['pci_ids'] with the new NIC PCI ID
  * PCI IDs are formatted as `VENDOR:MODEL:SUBVENDOR:SUBMODEL`
  * Example command used to display all Ethernet (PCI Class 0200): `lspci -Dmmn | grep 0200 | sed -e 's/-r[0-9][0-9] //g' | awk '{gsub("\"","");print $3 ":" $4 ":" $5 ":" $6}'`
* Upload firmware to artifacts via `artifactctl`
* Add new `firmware_url`, `firmware_md5` and `expected_firmware_version` definitions for the new NIC FW (copy/pasta from previous examples)


To add support for a new NIC/vendor:
* Create a top level key under `nic_firmware_data` (e.g. `broadcom`, `intel`, `mellanox`)
* Add initial PCI ID for new NIC under `nic_firmware_data[$NEW_NIC_VENDOR]['pci_ids']
  * PCI IDs are formatted as `VENDOR:MODEL:SUBVENDOR:SUBMODEL`
  * Example command used to display all Ethernet (PCI Class 0200): `lspci -Dmmn | grep 0200 | sed -e 's/-r[0-9][0-9] //g' | awk '{gsub("\"","");print $3 ":" $4 ":" $5 ":" $6}'`
* Create a new ansible task named identically to the new top level vendor key in `nic_firmware_data` (e.g. `broadcom.yaml`, `intel.yaml`, `mellanox.yaml`)
* Add all vendor specific firmware detection and modification tasks to `$NEW_NIC_VENDOR.yaml`
* Every vendor FW modification process is different, but attempt to follow previous examples where possible.
  * Example: re-use variables like `firmware_url`, `firmware_md5` and `expected_firmware_version`
* Upload firmware to artifacts via `artifactctl`
