---
title: Azure Recon
category: Azure
weight: -1
---

### Links

- Get if Azure tenant is in use, tenant name and Federation  
https://login.microsoftonline.com/getuserrealm.srf?login=[USERNAME@DOMAIN]&xml=1

- Get the Tenant ID  
https://login.microsoftonline.com/[DOMAIN]/.well-known/openid-configuration

- • Validate Email ID by sending requests to 
https://login.microsoftonline.com/common/GetCredentialType


### AADInternals
```powershell
# Get tenant name, authentication, brand name and domain name
Get-AADIntLoginInformation -Username taise@inugasky.net

# Get tenant ID
Get-AADIntTenantID -Domain inugasky.net

# Get tenant domains
Get-AADIntTenantDomains -Domain inugasky.net

# Get all the information (noizy?)
Invoke-AADIntReconAsOutsider -DomainName inugasky.nets
```

### MSGraph PowerShell module

```powershell
Install-Module Microsoft.Graph

# Connect to MgGraph
Connect-MgGraph

$Token = eyJ0...
Connect-MgGraph -AccessToken ($Token | ConvertTo-SecureString -AsPlainText -Force)


# Get the current session state
Get-MgContext

# Get details of the curernt tenant
Get-MgOrganization | fl *

Get-MgUser -All
Get-MgUser -UserId taise@Inugasky.net
Get-MgUser -Filter "startswith(DisplayName, 'a')" -ConsistencyLevel enentual
Get-MgUser -Search '"DisplayName:admin"' -ConsistencyLevel eventual

# List all the attributes for a user
Get-MgUser -UserId taise@inugasky.net | fl *

# Search attributes for all users that contain the string "password"
Get-MgUser -All |%{$Properties =
$_;$Properties.PSObject.Properties.Name | % {if
($Properties.$_ -match 'password')
{"$($Properties.UserPrincipalName) - $_ -
$($Properties.$_)"}}}

# Objects created by any user
Get-MgUserCreatedObject -UserId taise@Inugasky.net | fl *

# Objects owned by a specific user
Get-MgUserOwnedObject -UserId taise@inugasky.net | fl *
```

