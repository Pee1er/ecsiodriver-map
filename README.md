Title: Elitegroup ECSIoDriver ecsiodriverx64.sys physical memory access

Vendor: Elitegroup Computer Systems Co., Ltd.
Product: ECSIoDriver
Component: ecsiodriverx64.sys
SHA-256: 7de1ce434f957df7bbdf6578dd0bf06ed1269f3cc182802d5c499f5570a85b3a
MD5: f34489c0f0d0a16b4db8a17281b57eba
SHA-1: 3a1f19b7a269723e244756dac1fc27c793276fe7
Size: 13864 bytes
PE timestamp: 2014-05-28 06:59:16 UTC
FileVersion:
ProductVersion:
OriginalFilename:
Authenticode status: Valid
Signer: CN="Elitegroup Computer Systems Co., Ltd.", OU="Elitegroup Computer Systems Co., Ltd.", O="Elitegroup Computer Systems Co., Ltd.", L=Taipei, S=Taiwan, C=TW
Certificate thumbprint: 89F0738CC1156B4D7FBD1D83AB7F1BBC8CB59146
Certificate not before: 2013-08-13 02:55:45 UTC
Certificate not after: 2016-08-13 02:55:45 UTC
Device: \\.\ECSIoDriver_1_1_0_0
PDB: D:\Kai_Work\small_ap\ECSIoDriver\ECSIoDriver\x64\Win7Release\ECSIoDriver.pdb

CVE:

Description:
ecsiodriverx64.sys in Elitegroup Computer Systems Co., Ltd. ECSIoDriver (SHA-256 7de1ce434f957df7bbdf6578dd0bf06ed1269f3cc182802d5c499f5570a85b3a) passes a caller-supplied 64-bit physical address to MmMapIoSpace on IOCTL 0xDDD5A4C8 (METHOD_BUFFERED), maps a caller-supplied length as non-cached, accepts only an access width of 1, 2, or 4, multiplies that width by a caller-supplied count, does not validate the address range, and copies caller-supplied bytes into that mapping, which allows a local user to read and write arbitrary physical memory and locally escalate privileges via \\.\ECSIoDriver_1_1_0_0. IOCTL 0xDDD564C4 reads those mapped bytes back into the start of the output buffer. This file has no VERSIONINFO resource. The Authenticode status is Valid, signed by CN="Elitegroup Computer Systems Co., Ltd.", OU="Elitegroup Computer Systems Co., Ltd.", O="Elitegroup Computer Systems Co., Ltd.", L=Taipei, S=Taiwan, C=TW. On 2026-11-03, on an isolated Windows 10 Enterprise LTSC 2021 virtual machine (10.0.19044.7807), an administrator loaded this file and the service started. The administrator locked one of its own pages at physical address 0x13c69f000, wrote 0x3C as the first byte, and did not send either IOCTL. Local account aida-low, a member of Users and not a member of Administrators, opened \\.\ECSIoDriver_1_1_0_0, sent IOCTL 0xDDD564C4 for that address with width 1 and count 1, and read 0x3C. That account then sent IOCTL 0xDDD5A4C8, wrote 0x5A, and read 0x5A back through IOCTL 0xDDD564C4, then wrote 0xA5 and read 0xA5 back the same way. The administrator read the first byte of that physical page and saw 0x5A, then 0xA5. The test did not show a crash. The device security descriptor was O:BAG:SYD:(A;;0x1201bf;;;WD)(A;;FA;;;SY)(A;;FA;;;BA)(A;;0x1200a9;;;RC)S:AI(ML;;NW;;;LW). Everyone (WD) has access mask 0x1201bf, which includes FILE_READ_DATA and FILE_WRITE_DATA.

Suggested fix: on IOCTL 0xDDD5A4C8 and IOCTL 0xDDD564C4, use only a physical address chosen inside the driver, and limit access to \\.\ECSIoDriver_1_1_0_0 to Administrators and SYSTEM.
