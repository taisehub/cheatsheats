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

---

## MSGraph PowerShell

### MSGraph PowerShell module

```powershell
Install-Module Microsoft.Graph

# Connect to MgGraph
Connect-MgGraph

# Connect to MgGraph with Token
$Token = eyJ0...
Connect-MgGraph -AccessToken ($Token | ConvertTo-SecureString -AsPlainText -Force)

# Get the current session state
Get-MgContext

# Get details of the curernt tenant
Get-MgOrganization | fl *
```

### MSGraph PowerShell module - Users

```powershell
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

### MSGraph PowerShell module - Groups

```powershell
# List all Groups
Get-MgGroup -All

# Enumerate a specific group
Get-MgGroup -GroupId 00000000-0000-0000-0000-000000000000

# To search for groups which contain the word "admin" in their name
Get-MgGroup -ConsistencyLevel eventual -Search '"DisplayName:Admin"'

# Get Groups that allow Dynamic membership
Get-MgGroup | ?{$_.GroupTypes -eq 'DynamicMembership'}

# Enumerate a specific group
Get-MgGroupMember -GroupId 00000000-0000-0000-0000-000000000000

# Get groups and roles where the specified user is a member
(Get-MgUserMemberOf -UserId taise@inugasky.net).AdditionalProperties

# Get all available role template
Get-MgDirectoryROleTemplate

# Get all available role templates
Get-MgDirectoryRoleTemplate

# Get all enabled roles
Get-MgDirectoryRole

# Enumerate users to whom roles are assigned
$RoleId = (Get-MgDirectoryRole -Filter "DisplayName -eq 'Global Administrator'").Id
(Get-MgDirectoryRoleMember -DirectoryRoleId $RoleId).AdditionalProperties
```

### MSGraph PowerShell module - Devices

```powershell
# Get all Azure joined and registered devices
Get-MgDevice -All | fl *

# List all the active devices
Get-MgDevice -All | ?{$_.ApproximateLastSignInDateTime -ne $null}


# List Registered owners of all the devices
$Ids = (Get-MgDevice –All).Id; foreach($i in $Ids){ (Get-MgDeviceRegisteredOwner -DeviceId $i).AdditionalProperties}
$Ids = (Get-MgDevice –All).Id; foreach($i in $Ids){ (Get-MgDeviceRegisteredOwner -DeviceId $i).AdditionalProperties.userPrincipalName}

# List devices owned by a user
(Get-MgUserOwnedDevice -userId taise@inugasky.net).AdditionalProperties

# List devices registered by auser
(Get-MgUserRegisteredDevice -userId taise@inugasky.net).AdditionalProperties

# List devices managed using Intune (list compliant devices)
Get-MgDevice -All | ?{$_.IsComliant -eq "True"} | fl *
```

### MSGraph PowerShell module - Apps

```powershell
# Get all the application objects registered with the current tenant
Get-MgApplication -All

# Get all details about an application
Get-MgApplicationByAppId -AppId 00000000-0000-0000-0000-000000000000 | fl *

# Get an application based on the display name
Get-MgApplication -All | ?{$_.DisplayName -match "app"}

# List all the apps with an application password
Get-MgApplication -All| ?{$_.PasswordCredentials -ne $null}

# Get owner of an application
(Get-MgApplicationOwner -ApplicationId 00000000-714e-0000-000000000000).AdditionalProperties.userPrincipalName

# Get Apps where a User has a role
Get-MgUserAppRoleAssignment -UserId taise@inugasky.net | fl *

# Get Apps where a Group has a role 

Get-MgGroupAppRoleAssignment -GroupId 00000000-0000-0000-0000-000000000000 | fl *
```

---

## Az PowerShell

```powershell
Install-Module Az
Connect-AzAccount

Connect-AzAccount -AccountId taise@inugasky.net -AccessToken eyJ0...

# Available ResouceType: AadGraph, AnalysisServices, Arm, Attestation, Batch, DataLake, KeyVault, MSGraph, OperationalInsights, ResourceManager, Storage, Synapse
Get-AzAccessToken -ResourceTypeNName MSGraph

Get-Command *azad*
Get-Command *az*
Get-Command *azvm*

# Get the information about the current context
Get-AzContext

# Enumerate subscriptions accessible by the current user
Get-AzSubscription

# Enumerate all resources visible to the current user
Get-AzResource

# Enumarate all Azure RBAC role assignments
Get-AzRoleAssignment

# All Azure roles assigned to the user
Get-AzRoleAssignment -SignInName taise@inugasky.net

# Enumerate VM
Get-AzVM -Name test* | fl *

# Enumerate Web App Services
Get-AzWebApp

# Enumerate Function Apps
Get-AzFunctionApp

# Enumerate StrageAccounts
Get-AzStorageAccount

# Check access restrictions by IP address
(Get-AzStorageAccount | select -ExpandProperty NetworkRuleSet).IpRules

# Enumerate KeyVault
Get-AzKeyVault
```
