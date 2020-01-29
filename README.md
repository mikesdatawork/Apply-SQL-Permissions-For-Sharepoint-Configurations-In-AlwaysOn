![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Apply SQL Permissions For Sharepoint Configurations In AlwaysOn
**Post Date: July 27, 2016**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Lets say you have a brand new SQL AlwaysOn environment for SQL 2012 or SQL 2014 and now the Sharepoint Admins are connecting to the Listener name so they can install and configure Sharepoint. The Sharepoint configuration process may run into a few permissions errors whenever they try to create their databases. So you 'the DBA' will check to ensure that all the appropriate permissions are set on each Node in the Cluster for the Sharepoint Administrator account which in this example is MyDomainMySharepointAccount. You notice that it's set as the Sysadmin for both the OS and SQL Server and has already been added to the database owner permissions.
(Basically it's been added to Master dbo, and the local administrators group on the Server it's self, and it's already added to the Sysadmin role under each SQL Server instance.)

However; you still get issues under 'Specify Configuration Database Settings' for the Sharepoint configuraiton process. They see this error:

Cannot connect to database master at SQL Server at MyAvailabilityGroupName. 

The database might not exist or the current user does not have permission to connect to it.
Well… in usual Microsoft fashion just because it's been added to the appropriate roles for the OS, and SQL Server it doesn't mean the setup process from another Microsoft application will choose to check those to validate. In this case you'll need to grant specific permissions in the Securables so the Sharepoint setup process will validate that the appropriate permissions have been set.
You can do this in 2 ways of course… Add the securables through the SSMS client GUI, or simply run the following logic. 

Note: This will need to be run on all Nodes in the Cluster configuration.</p>      


## SQL-Logic
```SQL
use [master]
GO
 
CREATE USER [MyDomain\MySharepointAccount] FOR LOGIN [MyDomain\MySharepointAccount]
ALTER ROLE [db_owner] ADD MEMBER [MyDomain\MySharepointAccount]
GRANT ALTER ANY AVAILABILITY GROUP TO [MyDomain\MySharepointAccount];
GRANT CONTROL SERVER TO [MyDomain\MySharepointAccount];
GRANT CREATE ANY DATABASE TO [MyDomain\MySharepointAccount];


```

![Set Permissions]( https://mikesdatawork.files.wordpress.com/2016/07/image0023.png "Set SQL Permissions For Sharepoint")


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

