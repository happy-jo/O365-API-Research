# O365-API-ResearchOffice 365 Management API:
*Please keep in mind that this is a reference for the information that is useful to me, but there is much more available on the google.*

Resource Link: [MSFT O365 Management API](https://msdn.microsoft.com/en-us/office-365/get-started-with-office-365-management-apis)

*Microsoft Updates information all of the time so this link could be depricated.*
## Pre-requisites:
Create a Registered App in Azure AD. 
[Walkthrough found here](https://docs.microsoft.com/en-us/office/office-365-management-api/get-started-with-office-365-management-apis)

## Required information for the call:
```
$ClientID       = "Client ID"  
$ClientSecret   = "Client Secret"  
$TenantGUID     = "Tenant Guid"  
$tenantdomain   = "Tenant Domain"    
$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.windows.net"            # Azure AD Graph API resource URI This will change 
```


# Complete OAuth request for O365 data (example):

```
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret} 
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body 
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 

$url = "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/list"

$myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)  

($myReport.Content | ConvertFrom-Json).value
```


# Available Office 365 calls (for the URI field): 

###### Base URL:
`https://manage.office.com/api/v1.0/$tenantGUID`

(Example of complete URL: `https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint`)

###### Display availible office activity feeds: 
`/activity/feed/subscriptions/list`

###### Display all services: 
`/ServiceComms/Services`

###### Display all service status: 
`/ServiceComms/CurrentStatus`

###### Display all service historical status:
`/ServiceComms/HistoricalStatus`

###### Display services messages (lots of data):
`/ServiceComms/Messages`

###### Display available content for (SharePoint Audit Logs): 
`Sharepoint Audit Logs: /activity/feed/subscriptions/content?contentType=Audit.SharePoint`

(available content types can be found above under "display office activity feeds")
```
Audit.AzureActiveDirectory
Audit.Exchange
Audit.SharePoint
Audit.General 
DLP.All
```

###### Display DLP sensitivity types: 
`/activity/feed/resources/dlpSensitiveTypes`

###### List notifications (Normally has nothing and based on feed): 
`/activity/feed/subscriptions/notifications?contentType=Audit.SharePoint`

###### Retrieve content from content Uri (note the single quote instead of the double for the URL): 
`/activity/feed/audit/20180321155043789031580$20180321155043789031580$audit_sharepoint$Audit_SharePoint`

