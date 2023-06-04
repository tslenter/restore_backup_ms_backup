# Control / back or restore a Microsoft DNS server (dnscmd + powershell)

### To view the available DNS commands in powershell:
```
Get-Command -Module DnsServer
```

### View DNS zone available on remote PC with Powershell:
```
Get-DnsServerZone -computername ltest001.remotesyslog.com
```

### View A records from a zone (Change record type to view other records, like MX):
```
Get-DnsServerResourceRecord -computername ltest001.remotesyslog.com -RRType A  remotesyslog.com
```

## To backup or restore do the following steps:

### Create a local/share/symlink directory for backup(server what runs the DNS service):
```
cd /
mkdir temp
```

### To export a zone with powershell do:
```
Export-DnsServerZone -ComputerName ltest001.remotesyslog.com -Name remotesyslog.com -FileName \..\..\..\temp\skylineseven.local.dns
```

### To export a zone with dnscmd do:
```
dnscmd ltest001.remotesyslog.com /ZoneExport remotesyslog.com \..\..\..\temp\tslenter_remotesyslog.com.local.dns
```

### Default start location on remote server:
```
C:\Windows\system32\dns 
```

### Delete a zone with powershell:
```
Remove-DnsServerZone -ComputerName ltest001.remotesyslog.com -Name remotesyslog.com
```

### Delete a zone with dnscmd:
```
dnscmd ltest001.remotesyslog.com /zonedelete "remotesyslog.com" /dsdel /f
```

### To load the zone with powershell do:
```
Add-DnsServerPrimaryZone -ComputerName 192.168.99.250 -Name "remotesyslog.com" -zonefile \..\..\..\temp\skylineseven.local.dns -loadexisting
```

### To load the zone dnscmd do:
```
dnscmd.exe 192.168.99.250 /zoneadd "remotesyslog.com" /primary /file \..\..\..\temp\tslenter_skylineseven.local.dns /load
```

### Reset the zone to active directory integraded (powershell):
```
ConvertTo-DnsServerPrimaryZone -computerName 192.168.99.250 -name remotesyslog.com -PassThru -Verbose -ReplicatinScope Domain -Force
```
### Reset the zone to active directory integraded (dnscmd):
```
dnscmd.exe 192.168.99.250 /zoneresettype "remotesyslog.com" /dsprimary
```

### Reloads the AD integraded DNS zone from the AD DS:
```
Restore-DnsServerPrimaryZone -ComputerName ltest001.remotesyslog.com -Name "remotesyslog.com" -PassThru
```
