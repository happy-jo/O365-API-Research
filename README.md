# O365-API-ResearchOffice 365 Management API: 

Resource Link: https://msdn.microsoft.com/en-us/office-365/get-started-with-office-365-management-apis 
^^^This is probably deprecated 

$ClientID = "<Client ID>"  

$ClientSecret = "<Client Secret>"  

$TenantGUID = "Tennt Guid>"  

$tenantdomain = "Tenant Domain"    

$loginURL = "https://login.microsoftonline.com"

$resource       = "https://graph.windows.net"            # Azure AD Graph API resource URI This will change 


# Get an Oauth 2 access token based on client id, secret and tenant domain 

$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret} 

$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body 

$headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"} 

$url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta

$myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)  

($myReport.Content | ConvertFrom-Json).value | Format-Table signinDateTime, userPrincipalName, appDisplayName, ipAddress, loginStatus 

# Available Office 365 calls (for the URI field): 

  
display office activity feeds 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/list" -UseBasicParsing).content | ConvertFrom-Json 


display all services 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/ServiceComms/Services" -UseBasicParsing).content 
 

display all service status 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/ServiceComms/CurrentStatus" -UseBasicParsing).content 

display all service historical status 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/ServiceComms/HistoricalStatus" -UseBasicParsing).content 

display services messages (lots of data) 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/ServiceComms/Messages" -UseBasicParsing).content 

display availible content 
available content types can be found above under "display office activity feeds" 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/content?contentType=Audit.SharePoint" -UseBasicParsing).content | ConvertFrom-Json 

adding "&amp;startTime=2015-10-01&amp;endTime=2015-10-02&amp;nextPage=2015101900R022885001761" to the end of the request to search based on time 

Retrieve content from content Uri (note the single quote insead of the double) 
(Invoke-WebRequest -Headers $headerParams -Uri 'https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/audit/20180321155043789031580$20180321155043789031580$audit_sharepoint$Audit_SharePoint' -UseBasicParsing).content | ConvertFrom-JsonDisplay DLP Sensitive datatype 

(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/resources/dlpSensitiveTypes" -UseBasicParsing).content | ConvertFrom-Json 


List notifications (Normally has nothing) 
(Invoke-WebRequest -Headers $headerParams -Uri "https://manage.office.com/api/v1.0/$tenantGUID/activity/feed/subscriptions/notifications?contentType=Audit.SharePoint" -UseBasicParsing).content | ConvertFrom-Json
