#To test do:
#To view the available DNS commands in powershell:
Get-Command -Module DnsServer

#View DNS zone available on remote PC with Powershell:
Get-DnsServerZone -computername ltest001.skylineseven.nl

#View A records from a zone (Change record type to view other records, like MX):
Get-DnsServerResourceRecord -computername ltest001.skylineseven.nl -RRType A  skylineseven.nl

#To backup or restore do the following steps:
#Create a local/share/symlink directory for backup(server what runs the DNS service):
cd /
mkdir temp

#To export a zone with powershell do:
Export-DnsServerZone -ComputerName ltest001.skylineseven.nl -Name skylineseven.nl -FileName \..\..\..\temp\skylineseven.local.dns
#To export a zone with dnscmd do:
dnscmd ltest001.skylineseven.nl /ZoneExport skylineseven.nl \..\..\..\temp\tslenter_skylineseven.nl.local.dns
#Default start location on remote server:
#C:\Windows\system32\dns 
#This command runs on the DNS server ltest001.skylineseven.nl and stores the file locally
#Feel free to save it to a share

#If the zone still exist do:
#Delete a zone with powershell:
Remove-DnsServerZone -ComputerName ltest001.skylineseven.nl -Name skylineSeven.nl
#Delete a zone with dnscmd:
dnscmd ltest001.skylineseven.nl /zonedelete "skylineseven.nl" /dsdel /f

#To load the zone with powershell do:
Add-DnsServerPrimaryZone -ComputerName 192.168.99.250 -Name "skylineseven.nl" -zonefile \..\..\..\temp\skylineseven.local.dns -loadexisting
#To load the zone dnscmd do:
dnscmd.exe 192.168.99.250 /zoneadd "skylineseven.nl" /primary /file \..\..\..\temp\tslenter_skylineseven.local.dns /load

#Reset the zone to active directory integraded (powershell):
ConvertTo-DnsServerPrimaryZone -computerName 192.168.99.250 -name skylineseven.nl -PassThru -Verbose -ReplicatinScope Domain -Force
#Reset the zone to active directory integraded (dnscmd):
dnscmd.exe 192.168.99.250 /zoneresettype "skylineseven.nl" /dsprimary

#Reloads the AD integraded DNS zone from the AD DS:
Restore-DnsServerPrimaryZone -ComputerName ltest001.skylineseven.nl -Name "skylineseven.nl" -PassThru