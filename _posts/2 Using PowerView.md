## Powerview for AD Enumeration
* Get Current Domain : Get-NetDomain
 * Enum other Domain : Get-NetDomain -Domain (Domain Name)
 * Get Domain sid : Get-DomainSID
### Get Domain Policy : 
```bash
Get-DomainPolicy

#Will show us the policy configurations of the Domain about system access or kerberos
(Get-DomainPolicy)."system access"
(Get-DomainPolicy)."kerberos policy"
```

### Get Domain Controlers :
```bash
Get-NetDomainController
Get-NetDomainController -Domain <DomainName>
```

### Enumerate Domain Users :
```bash 
Get-NetUser
Get-NetUser -SamAccountName <user> 
Get-NetUser | select cn
Get-UserProperty

#Check last password change
Get-UserProperty -Properties pwdlastset

#Get a spesific "string" on a user's attribute
Find-UserField -SearchField Description -SearchTerm "wtver"

#Enumerate user logged on a machine
Get-NetLoggedon -ComputerName <ComputerName>

#Enumerate Session Information for a machine
Get-NetSession -ComputerName <ComputerName>

#Enumerate domain machines of the current/specified domain where specific users are logged into
Find-DomainUserLocation -Domain <DomainName> | Select-Object UserName, SessionFromName
```

 ### Enumerate Domain Computers :
```bash 
Get-NetComputer -FullData
Get-DomainGroup

#Enumerate Live machines 
Get-NetComputer -Ping
```

### Enumerate Group Members :
```bash
Get-NetGroupMember -GroupName "<GroupName>" -Domain <DomainName>

#Enumerate the members of a specified group of the domain
Get-DomainGroup -Identity <GroupName> | Select-Object -ExpandProperty Member

#Returns all GPOs in a domain that modify local group memberships through Restricted Groups or Group Policy Preferences
Get-DomainGPOLocalGroup | Select-Object GPODisplayName, GroupName
```

### Enumerate Shares :
```bash
#Enumerate Domain Shares
Find-DomainShare

#Enumerate Domain Shares the current user has access
Find-DomainShare -CheckShareAccess
```

### Enumerate Group Policies :
```bash
Get-NetGPO

# Shows active Policy on specified machine
Get-NetGPO -ComputerName <Name of the PC>
Get-NetGPOGroup

#Get users that are part of a Machine's local Admin group
Find-GPOComputerAdmin -ComputerName <ComputerName>
```

### Enumerate ACLs :
```bash 
# Returns the ACLs associated with the specified account
Get-ObjectAcl -SamAccountName <AccountName> -ResolveGUIDs
Get-ObjectAcl -ADSprefix 'CN=Administrator, CN=Users' -Verbose

#Search for interesting ACEs
Invoke-ACLScanner -ResolveGUIDs

#Check the ACLs associated with a specified path (e.g smb share)
Get-PathAcl -Path "\\Path\Of\A\Share"
```

### Enumerate Domain Trust :
```bash
Get-NetDomainTrust
Get-NetDomainTrust -Domain <DomainName>
```

### Enumerate Forest Trust :
```bash
Get-NetForestDomain
Get-NetForestDomain Forest <ForestName>

#Domains of Forest Enumeration
Get-NetForestDomain
Get-NetForestDomain Forest <ForestName>

#Map the Trust of the Forest
Get-NetForestTrust
Get-NetDomainTrust -Forest <ForestName>
```

### Enumerate users :
```bash
#Finds all machines on the current domain where the current user has local admin access
Find-LocalAdminAccess -Verbose

#Find local admins on all machines of the domain:
Invoke-EnumerateLocalAdmin -Verbose

#Find computers were a Domain Admin OR a spesified user has a session
Invoke-UserHunter
Invoke-UserHunter -GroupName "RDPUsers"
Invoke-UserHunter -Stealth

#Confirming admin access:
Invoke-UserHunter -CheckAccess
```
