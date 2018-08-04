
# SharePoint Site Collection Migration Module

This module was built as part of a project I am working on to migrate a number of site collections between content databases.

**It is currently still under development as I look to add more features and tighten up the error handling.**

The module exposes a number of functions which mimic the steps I would follow to move multiple site collections:

1. Set the site collection(s) status to read-only
2. Run Backup-SPSite prior to moving each site incase we need to restore the site
3. Run Move-SPSite to carry out the actual site collection move to the target content database
4. Do an IIS Reset on all the servers in the SharePoint farm
5. Set the status of the site collections to Unlock

## What the module does
The module makes extensive use of verbose output as well as writing to a log file to maintain a history of all the actions during the migration.

### Module Functions


|Function Name  |Description  |
|---------|---------|
|Test-TISSPSiteMoveInfo         |Checks the site collections to be moved to ensure that the site (URL) exists, the target content database exists, the target content database is different from the site's current database and that the target database is associated with the same web application as the current site collection.         |
|Set-TISSPSiteLockStatus     |Used to set the site collection status to Unlock or Read Only         |
|Backup-TISSPSiteToDisk     |Backup the site to a specified directory         |
|Move-TISSPSite     |Move the site collection to the specified target content database         |
|Restart-TISSPIISAllServers     |Restarts IIS on all the servers in the SharePoint farm (does not include the SQL Server)         |
|Start-TISSPGradualDeleteTimerJob     |This is useful when testing on a dev. environment. If you try to move a site collection back to its original database before this job runs you will receive an error. Usually this job runs once a day usually at some point in then early hours of the morning          |
|Get-TISSPBackupDiskSpaceAvailable     |STILL UNDER DEV - Estimates how much storage space is required for the site collection back up and how much free space you have left on your target disk location         |


> [!IMPORTANT]
> At the moment I don't load the SharePoint Snap-in as part of the module as I noticed this could cause issues when reloading the module. Although I think this is more an issue during development rather than when running the module.
