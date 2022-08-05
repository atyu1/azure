# AZURE AZ-104 Notes

- Labs:
https://www.azurecitadel.com/network/vdc/rbac/
https://github.com/MicrosoftLearning/AZ-104-MicrosoftAzureAdministrator/tree/master/Instructions/Labs

## PowerShell
- Install PowerShell 
- Instal AZ module
``` Install-Module -Name Az -AllowClobber -Force```

- Connect account to Azure
``` Connect-AzAccount ```
- After Open browser and register the device

- Verify commands, for example list VMs
```Get-AzVM```

## Manage Azure Identities and Governance

### Pricing Plan:
https://azure.microsoft.com/en-us/pricing/details/active-directory/#pricing
https://docs.microsoft.com/en-us/azure/active-directory/authentication/concept-mfa-licensing#available-versions-of-azure-ad-multi-factor-authentication

1. Free
- 500 Objects limit
- A lot of sercives free
- Password protection
- MFA
- Basic security usage and report
- Self service password reset

2. Office 365
- all from Free
- Cloud password reset for users
- Company branding (logo and so)
- SLA

3. Premium P1
- All from Office
- Device password sync (from on prem to cloud)
- Password protection (bann easy or words for passowrds)
- Azure ID join
- Dynamic groups
- Conditional Access (based on where it is logging in or location)
- 3rd party integration

4. Premium P2
- All from P1
- Vulnereability and risky accounts detection
- Risk events (AI)
- Risk based conditional access policies
- Helper: P2 = risky things
- Priviledged Identity Management
- Access review

- Steps to use:
1. Activate the license
2. Assign users which can use it

### Terminology
`Domain` - area of network orginzs by single auth db
`Domain Controller` - server that do authentication and authorization
`Domain Computer` - registered device to the auth db ==> AD Object
`AD Object` - an elemnt of AD like: User or Group or Printer or Computer 
`Group Policy Object` - virtual collection of policies for AD objects
`Organization Unit` - subidivion/subfolder of and AD - place inside objectets or other OU
`Directory Service` - provides method of storing directory data and making it availale

### Managing multi-tenant user
- tenant is represented by unique `domain` - like test.com 
- if domain is not specified we use: test.microsoft.com
- every account is part of at least 1 tenant
- tenant must have subscription associated in order to be able create resources
- tenant can have 2+ subscriptions too

- `Subscriptions` - independent to tenant or user 
- Paymenet agreement
- 3 types:
 1. Free
 2. Pay as you go
 3. Enterprise

### Azure AD connect
- Connect on prem to AD in Cloud (Azure AD)
- We can use cloud-only to authenticate to Windows Machines on Prem
- When you don't have on prem AD
- When you dont want to put some users to on prem AD
- Add support for SSO
- Features:
  - Password hash sync - sync hash for passwords with on prem
  - Pass-through - allow to use same user/pass on prem and on cloud too
  - Federation - hybrid environment using on prem AD FS infra
  - Synchronization - auto sync between om prem and cloud for users/groups/...
  - Health Monitoring - provides central monitoring in azure portal

### Administrative Units
- Used in large org. and divide them to multiple groups (administrative units)
- Easier management
- Assing roles to Administrative units
- Premium feature

### Manage Users and Groups
- username can be email only from associated domain
- its possible to invite too by emails
- Dynamic Groups - add users automatically to groups based on conditions
- Guest Users = users which are part of another organization - possible to invite

- Password reset can be enabled per group - after users can do it themself 
- Password reset can use email, phone num, Microsoft App Code, Office Phone, security questions 
  - Auth info is valid for 180 days by default
- Password reset can be enabled for selected users or all or none (no group option)
- Assign roles or applications directly to group
- Groups can have:
 - Owners - have permissions + manage members
 - Member - have permissions

- Assign access rights to users
  - direct assignements - assign manually to users
  - group assignements - assign permissions via group to users
  - rule based - create rule which dynamically assign users to groups
  - external authority - access come via external source like on prem or Saas

- Deleted remains for 30 days as deleted - can be reverted for accidental delete

### Bulk Operations:
- We can create, invite, delete & download users
- It can be through one bulk csv file which is uploaded

### Device Management
- Device can be added - external registration
- After permission can be specified
- We can limit only users from registered devices
- In Windows - there is App: Connect -> this can register to Azure -> enter user/pass and it will be done

### RBAC
- Used for Authorization
- Highest Role: Global Administrator
- We can greant access to recources or resource groups
- Custom roles can be created we need Premium Level plan
- Custom can specify like: Read and Update (don't delete) - add permsissions/actions
- We can include or exclude permissions (all except selected)

### Subscriptions
- Identity = name used for auth
- Account = user or app - used for authentication, Identity + More info
- Tenant = orgnaization - represented by domain name
- Subscription is billing agreement (Free, pay as you go)
- Resource is any entity managed by account - VM, WebApp, Storage Account, anything created
- Resource groups - Organize resources to group 
- All resources can belong to 1 resource group only

- We can add users to subscription, shared payment
- Susbscription can have access control - various roles can be chosed - cores are Owner/Contributor/Reader
- Contributor cannot assign other users to subscriptions (owner can)

- `Cost Management and Billing`
- It can do cost analysis and predict what we will pay
- See invoices and billings

- `Resource [Group] Locks`
- We can lock so users who don't have permission cannot create resources
- Users with permissions can delete locks
- Lock types:
 - Read-Only - don't delete/create/update any resource
 - Delete - just don't delete resource

- `Azure Policy`
- Limit users to not create expensive resources
- Define policies to enforce company standards and SLAs
- It can also limit Resource Types, versions, locations, default tagging, VM SKUs (Flavor)
- In GUI, its 'Policy"
- Scope - for full subscription or Resource Groups under Subscription
- Policies can be applied on Resource Groups level to for example ensure policicies like have tag or encrypted disk
- Policy can be associated with powershell too, example:
1. Save Policy Def
 ```$policy = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq "Policy name" }```

2. Create RG
 ``` New-AzResourceGroup -Name "test-rg" -Location "East EU" ```

3. Save it to variable
```$rg = Get-AzResourceGroup -Name "test-rg" -Location "East EU" ```

4. Assign Policy
``` New-AzPolicyAssignement -Name "Rulez" -DisplayName "Rulez" -Scope $rg.ResourceId -PolicyDefinition $policy ```

### Relationship between User Subscritpion Tenant
- User - lowest level
- Tenant - exist by defualt (contains users)
- To create resource we need subscription
- We can have multiple subscriptions 
- Subscriptions are associated with users
- Management group contains subscriptions or other MGMT groups
- We can add users to Management Groups and then they will have proper subscriptions

## Manage Azure Storage Accounts
- Like S3 in AWS
- Containers are like dirs/folders where we can upload files

### Providing access
- Access keys - 2x can be generated - can be changed if needed, permanent access, mostly for scripts
- Shred Access Signature (SAS) - additional access key with limited timeline, can be defined how long its valid

### Acces Tier
- Cool storage - less paid for storage - 2x for access, min 30 days the file will be there
- Hot storage - more for storage less for access
- Archive - the most less for storage price but no immediate access to files  - paying min for 180 days

### Lifcycle management
- Automated movement between tiers for files

### Object Replication
- Blob Service -> Object Replication
- Async copy to another account automatically

## Azure Virtual Machines

### Custom Scripts
- 2 options:
  - Extension - for Windows - we can add via Extension -> Custom Script Extension, after run Power Shell or other configs
  - CloudInit - for Linux machines

### Azure Bastion Server
- Jumphost server
- Create for specific subnet

