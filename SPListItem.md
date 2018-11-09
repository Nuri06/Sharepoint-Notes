# This document related to SPListItem object. You can find some of helpful powershells or c# codes.
### Check in all files within all webs.

```powershell
$webApplication = Get-SPWebApplication "WebApplicationUrl"
$listName = "Pages"
foreach ($site in $webApplication.Sites) {
    $web = $site.OpenWeb();
    foreach ($web in $site.AllWebs) {
        Write-Host $web.Url
        $list = $web.Lists |? {$_.Title -eq $listName}
        foreach ($item in $list.Items) 
        {
            $itemFile = $item.File
            if( $itemFile.CheckOutStatus -ne "None" )
            { 
                $itemFile.CheckIn("Automatic CheckIn. (Administrator)", [Microsoft.SharePoint.SPCheckinType]::MajorCheckIn);
                Write-Host $item["Name"] " Checked In" -ForeGroundColor Green
            }
        }
    }
}
```

### Delete All Previous Versions
```powershell
$SPweb = Get-SPWeb "SiteUrl"
$limit = (Get-Date).AddMonths(-6)
$SPlist = $SPweb.Lists["Pages"]

foreach ($SPitem in $SPlist.Items)
{
    $currentVersionsCount= $SPItem.Versions.count

    for($i=$currentVersionsCount-1;$i -ge 0;$i--)
        if($SPitem.Versions[$i].IsCurrentVersion){
            Write-Host "Current version"
        }
        else{ 
            if($limit -gt $SPitem.Versions[$i].Created)
            {
                $SPitem.Versions[$i].delete();
            }
        }
    }
}
```
