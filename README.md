# O365-API-ResearchOffice 365 Management API: 

Resource Link: https://msdn.microsoft.com/en-us/office-365/get-started-with-office-365-management-apis 
^^^This is probably deprecated 

## Required information for the call:
```
$ClientID       = "Client ID"  
$ClientSecret   = "Client Secret"  
$TenantGUID     = "Tenant Guid"  
$tenantdomain   = "Tenant Domain"    
$loginURL       = "https://login.microsoftonline.com"
$resource       = "https://graph.windows.net"            # Azure AD Graph API resource URI This will change 
```


# Get an Oauth 2 access token based on client id, secret and tenant domain 

```
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret} 
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body 
$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 

$url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta

$myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)  

($myReport.Content | ConvertFrom-Json).value
```


# Available Office 365 calls (for the URI field): 

Base URL: `https://manage.office.com/api/v1.0/$tenantGUID`

###### display office activity feeds: `/activity/feed/subscriptions/list`

###### display all services: `/ServiceComms/Services`

###### display all service status: `/ServiceComms/CurrentStatus`

###### display all service historical status: `/ServiceComms/HistoricalStatus`

###### display services messages (lots of data): `/ServiceComms/Messages`

###### display availible content: `Sharepoint Audit Logs: /activity/feed/subscriptions/content?contentType=Audit.SharePoint`
(available content types can be found above under "display office activity feeds")
Sharepoint Audit Logs: `/activity/feed/subscriptions/content?contentType=Audit.SharePoint`

###### Retrieve content from content Uri (note the single quote insead of the double for the URL): `/activity/feed/audit/20180321155043789031580$20180321155043789031580$audit_sharepoint$Audit_SharePoint`

###### Display DLP sensitivity types: `/activity/feed/resources/dlpSensitiveTypes`

###### List notifications (Normally has nothing and based on feed): `/activity/feed/subscriptions/notifications?contentType=Audit.SharePoint`
