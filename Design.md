# Change MasterPage Powershell
Subwebs inherit MasterPage from parent site collection so you don't have to use subwebs.
```powershell
$webApplication = Get-SPWebApplication "WebApplicationUrl"
foreach ($site in $webApplication.Sites) {
    $web = $site.OpenWeb()
    write-host $web.Url
    $WebURL = $web.ServerRelativeUrl.TrimEnd("/")
    $web.CustomMasterUrl = $WebURL+"/_catalogs/masterpage/MasterPageName.master";
    $web.Update();
    Write-Host $web.Url $web.MasterUrl $web.CustomMasterUrl;
    $site.Dispose()
}
```
