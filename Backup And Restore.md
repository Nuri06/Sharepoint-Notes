
# Backup Site Collections to specific folder.
```powershell
$siteCollections=@("sitecollectionUrl1","sitecollectionUrl2")
foreach ($siteCol in $siteCollections) {
Try	
{	
	$fileName = $siteCol.Replace("http://", "")
	$fileName = $fileName.Replace(".com", "")
	$path="C:\BackupFolder\"+$fileName+".bak"
	write-host $siteCol  "   Starting"
	Backup-SPSite $siteCol -Path $path
	write-host $siteCol  "   Done"
}
Catch
{
	write-host ERROR $siteCol
}
}
```

# Restore Site Collection 
You can restore site collections with previous file.
```powershell
Restore-SPSite http://SiteCollectionUrl -path C:\Backup\SiteName.bak -HostHeaderWebApplication http://ApplicationUrl  -DatabaseServer databaseServer -DatabaseName dbName -force -confirm:$false
```

# Restore new Metadata Service from database backup
If you want to copy metadata service application, firstly you have copy database and create new service application with this code. 
Don't forget to relate this metadata service with the service application on central administration.
```powershell
$sa = New-SPMetadataServiceApplication -Name "Managed Metadata Service" -DatabaseName "CopiedDatabaseName" -ApplicationPool "SharePoint Web Services System" -SyndicationErrorReportEnabled
New-SPMetadataServiceApplicationProxy -Name "Managed Metadata Service Proxy" -ServiceApplication $sa -DefaultProxyGroup -ContentTypePushdownEnabled -DefaultKeywordTaxonomy –DefaultSiteCollectionTaxonomy
```
