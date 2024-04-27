# 获取 Windows Key

* cmd: \
  wmic path softwareLicensingService get OA3xOriginalProductKey
* powershell:\
  powershell "(Get-WmiObject -query 'select \* from SoftwareLicensingService').OA3xOriginalProductKey"
* regedit:\
  Computer\HKEY\_LOCAL\_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SoftwareProtectionPlatform
