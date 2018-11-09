
```powershell
$rootURL="sitecollectionUrl"

$site = Get-SPSite $rootURL
$web = Get-SPWeb $rootURL
$pubWeb  = [Microsoft.SharePoint.Publishing.PublishingWeb]::GetPublishingWeb($web)    
$list = $pubWeb.Lists["Cache Profiles"]
foreach ($list in  $pubWeb.Lists)
{
    Write-Host $list.Title
}

$profile["Title"] = "My Custom Cache Profile"
$profile["Display Name"] = "My Custom Cache Profile"
$profile["Description"] = "My Custom Cache Profile"
$profile["Perform ACL Check"] = $true
$profile["Enabled"] = $true
$profile["Duration"] = "180"
$profile["Check for Changes"] = $false
$profile["Vary by Custom Parameter"] = "Browser"
$profile["Vary by HTTP Header"] = ""
$profile["Vary by Query String Parameters"] = ""
$profile["Vary by User Rights"] = $true
$profile["Cacheability"] = "ServerAndPrivate"
$profile["Safe for Authenticated Use"] = $true
$profile["Allow writers to view cached content"] = $false
$profile.Update()



$cacheSettings = New-Object Microsoft.SharePoint.Publishing.SiteCacheSettingsWriter($rootURL)
$cacheSettings.EnableCache = $true
$cacheSettings.SetAuthenticatedPageCacheProfileId($site, $profile.ID)
$cacheSettings.Update()

```
