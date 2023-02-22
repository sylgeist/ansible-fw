# Adding new HBA firmware
To add support for new HBAS:
* If adding a new "family" of HBAs (aka adding Dell PERCs) Create a new play in under `tasks` with some friendly-ish name (e.g. `dell_perc.yml`) and include any steps required to download the firmware and apply to the HBA. Note that you do *not* need to handle HBA detection. This is addressed in `main.yml`
* Update the `hba_firmware_data` variable with a new dict using the following information:
  * The dict key _must_ be the friendly name used to identify the HBA. This should be identified using the HBA specific management software (`percli`, `ssacli`, etc).
  * Expected f/w version
  * URL to download the update
  * md5sum of the update
  * pci_id of the HBA (PCI IDs should be in the format `VENDOR_ID:DEVICE_ID:SUB_VENDOR_ID:SUB_DEVICE_ID` (run `lspci -mmns PCI_SLOT_LOCATION |sed -e 's/-r[0-9][0-9] //g' | awk '{gsub(""","");print $3 ":" $4 ":" $5 ":" $6}'` if you need to find this info. PCI_SLOT_LOCATION is something like `41:00.0`).

Example:

```
"HPE Smart Array E208i-a SR Gen10":
  hba_expected_version: "1.34-0"
  hba_url: "{{ artifact_base_url }}/firmware-smartarray-f7c07bdbbd-1.34-1.1.x86_64.rpm"
  hba_md5: "95254245fbc37555c43018727f34bf52"
  pci_id: "9005:028f:103c:0654"
```
