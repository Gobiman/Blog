---
title: "Post: DBA Tools 15 commands can't be missed by Chrissy LeMaire"
categories:
  - Blog
tags:
  - link
  - Post Formats
  - PowerShell
  - SQL
# link: https://docs.dbatools.io/
GitHub: https://gobiman.github.io/Blog/
---

15 Commands inroduced by Chrissy LeMaire at PSConfEn2019.
These commands can be found Some [DBA Tools Docs](https://docs.dbatools.io/) can also be shown..

To make sure the script does not run accidentaly, use the command.

```ruby
Break
```

```ruby
# Get-DbaRegisteredServer, aliased
Get-DbaRegisteredServer
Get-DbaRegisteredServer -SqlInstance localhost\sql2016 -IncludeLocal
Get-DbaRegisteredServer -Group onprem | Get-DbaDatabase | Select SqlInstance, Name | Format-Table -AutoSize
```

```ruby
# Connect-DbaInstance, supports everything!
Get-DbaRegisteredServer -Name azuresqldb | Connect-DbaInstance
```

```ruby
# CSV galore!
Get-ChildItem C:\temp\psconf\csv
Get-ChildItem C:\temp\psconf\csv | Import-DbaCsv -SqlInstance localhost\sql2017 -Database tempdb -AutoCreateTable -Encoding UTF8
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select top 10 * from [jmfh-year]"
```

```ruby
# Write-DbaDbTableData
Get-ChildItem -File | Write-DbaDbTableData -SqlInstance localhost\sql2017 -Database tempdb -Table files -AutoCreateTable
Get-ChildItem -File | Select *
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select * from files"
```

```ruby
# Gotta find it, run this once
Find-DbaInstance -ComputerName localhost
```

```ruby
# PII Management
Invoke-DbaDbPiiScan -SqlInstance localhost\sql2017 -Database AdventureWorks2014 | Out-GridView
```

```ruby
# Mask that
New-DbaDbMaskingConfig -SqlInstance localhost\sql2017 -Database AdventureWorks2014 -Table EmployeeDepartmentHistory, Employee -Path C:\temp | Invoke-Item
Invoke-DbaDbDataMasking -SqlInstance localhost\sql2017 -FilePath 'C:\github\community-presentations\chrissy-lemaire\mask.json' -ExcludeTable EmployeeDepartmentHistory
```

```ruby
# Very Large Database Migration
$params = @{
    Source                          = 'localhost'
    Destination                     = 'localhost\sql2017'
    Database                        = 'shipped'
    SharedPath                      = '\\localhost\backups'
    BackupScheduleFrequencyType     = 'Daily'
    BackupScheduleFrequencyInterval = 1
    CompressBackup                  = $true
    CopyScheduleFrequencyType       = 'Daily'
    CopyScheduleFrequencyInterval   = 1
    GenerateFullBackup              = $true
    Force                           = $true
}


Invoke-DbaDbLogShipping @params

# Recover when ready
Invoke-DbaDbLogShipRecovery -SqlInstance localhost\sql2017 -Database shipped
```

```ruby
# Install-DbaInstance / Update-DbaInstance
Update-DbaInstance -ComputerName sql2017 -Path \\dc\share\patch -Credential base\ctrlb
Invoke-Item 'C:\temp\psconf\Patch several SQL Servers at once using Update-DbaInstance by Kirill Kravtsov.mp4'
```

```ruby
# Spaghetti!
New-DbaDiagnosticAdsNotebook -TargetVersion 2017 -Path C:\temp\myNotebook.ipynb | Invoke-Item


# Dope - dbatools.io/timeline
Get-DbaAgentJobHistory -SqlInstance localhost\sql2017 -StartDate '2016-08-18 00:00' -EndDate '2018-08-19 23:59' -ExcludeJobSteps | ConvertTo-DbaTimeline | Out-File C:\temp\DbaAgentJobHistory.html -Encoding ASCII
Invoke-Item -Path C:\temp\DbaAgentJobHistory.html

# Prettier
Start-Process https://dbatools.io/wp-content/uploads/2018/08/Get-DbaAgentJobHistory-html.jpg

```

```ruby

# Availability Groups
Invoke-Item 'C:\temp\psconf\click-a-rama.mp4'
```


```ruby
# All in one, no hassle - includes credentials!
$docker1 = Get-DbaRegisteredServer -Name dockersql1
$docker2 = Get-DbaRegisteredServer -Name dockersql2

# setup a powershell splat (has docker been reset?)
$params = @{
    Primary = $docker1
    Secondary = $docker2
    Name = "test-ag"
    Database = "pubs"
    ClusterType = "None"
    SeedingMode = "Automatic"
    FailoverMode = "Manual"
    Confirm = $false
 }
 
# execute the command
 New-DbaAvailabilityGroup @params
```

```ruby

# Start-DbaMigration wraps 30+ commands
Start-DbaMigration -Source localhost -Destination localhost\sql2016 -UseLastBackup -Exclude BackupDevices, SysDbUserObjects -WarningAction SilentlyContinue | Out-GridView
```

```ruby
# Wraps like 20
Export-DbaInstance -SqlInstance localhost\sql2017 -Path C:\temp\dr
Get-ChildItem -Path C:\temp\dr -Recurse -Filter *database* | Invoke-Item
```

```ruby
#region BONUS
Get-ChildItem C:\github\community-presentations\*ps1 -Recurse | Invoke-DbatoolsRenameHelper | Out-GridView
```

```ruby
# Diagnostic
Invoke-DbaDiagnosticQuery -SqlInstance localhost\sql2017 | Export-DbaDiagnosticQuery -Outvariable exports
$exports | Select -Skip 3 -First 1 | Invoke-Item
```

```ruby
# Ola Hallengren supported
Install-DbaMaintenanceSolution -SqlInstance localhost, localhost\sql2016, localhost\sql2017 -ReplaceExisting -InstallJobs
```

```ruby
# ConvertTo-DbaXESession
Get-DbaTrace -SqlInstance localhost\sql2017 -Id 1 | ConvertTo-DbaXESession -Name 'Default Trace' | Start-DbaXESession
```

```ruby
 # Wraps a bunch
Test-DbaLastBackup -SqlInstance localhost -Destination localhost\sql2016 | Select * | Out-GridView
```

> Break---
title: "Post: DBA Tools 15 commands can't be missed by Chrissy LeMaire"
categories:
  - Blog
tags:
  - link
  - Post Formats
  - PowerShell
link: https://docs.dbatools.io/
GitHub: https://gobiman.github.io/Blog/
---

15 Commands inroduced by Chrissy LeMaire at PSConfEn2019.
These commands can be found Some [DBA Tools Docs](https://docs.dbatools.io/) can also be shown..

To make sure the script does not run accidentaly, use the command.

```ruby
> Break
```

```ruby
# Get-DbaRegisteredServer, aliased
Get-DbaRegisteredServer
Get-DbaRegisteredServer -SqlInstance localhost\sql2016 -IncludeLocal
Get-DbaRegisteredServer -Group onprem | Get-DbaDatabase | Select SqlInstance, Name | Format-Table -AutoSiz
```

```ruby
# Connect-DbaInstance, supports everything!
Get-DbaRegisteredServer -Name azuresqldb | Connect-DbaInstance
```

```ruby
# CSV galore!
Get-ChildItem C:\temp\psconf\csv
Get-ChildItem C:\temp\psconf\csv | Import-DbaCsv -SqlInstance localhost\sql2017 -Database tempdb -AutoCreateTable -Encoding UTF8
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select top 10 * from [jmfh-year]"
```

```ruby
# Write-DbaDbTableData
Get-ChildItem -File | Write-DbaDbTableData -SqlInstance localhost\sql2017 -Database tempdb -Table files -AutoCreateTable
Get-ChildItem -File | Select *
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select * from files"
```

```ruby
# Gotta find it, run this once
Find-DbaInstance -ComputerName localhost
```

```ruby
# PII Management
Invoke-DbaDbPiiScan -SqlInstance localhost\sql2017 -Database AdventureWorks2014 | Out-GridView
```

```ruby
# Mask that
New-DbaDbMaskingConfig -SqlInstance localhost\sql2017 -Database AdventureWorks2014 -Table EmployeeDepartmentHistory, Employee -Path C:\temp | Invoke-Item
Invoke-DbaDbDataMasking -SqlInstance localhost\sql2017 -FilePath 'C:\github\community-presentations\chrissy-lemaire\mask.json' -ExcludeTable EmployeeDepartmentHistory
```

```ruby
# Very Large Database Migration
$params = @{
    Source                          = 'localhost'
    Destination                     = 'localhost\sql2017'
    Database                        = 'shipped'
    SharedPath                      = '\\localhost\backups'
    BackupScheduleFrequencyType     = 'Daily'
    BackupScheduleFrequencyInterval = 1
    CompressBackup                  = $true
    CopyScheduleFrequencyType       = 'Daily'
    CopyScheduleFrequencyInterval   = 1
    GenerateFullBackup              = $true
    Force                           = $true
}


Invoke-DbaDbLogShipping @params

# Recover when ready
Invoke-DbaDbLogShipRecovery -SqlInstance localhost\sql2017 -Database shipped
```

```ruby
# Install-DbaInstance / Update-DbaInstance
Update-DbaInstance -ComputerName sql2017 -Path \\dc\share\patch -Credential base\ctrlb
Invoke-Item 'C:\temp\psconf\Patch several SQL Servers at once using Update-DbaInstance by Kirill Kravtsov.mp4'
```

```ruby
# Spaghetti!
New-DbaDiagnosticAdsNotebook -TargetVersion 2017 -Path C:\temp\myNotebook.ipynb | Invoke-Item


# Dope - dbatools.io/timeline
Get-DbaAgentJobHistory -SqlInstance localhost\sql2017 -StartDate '2016-08-18 00:00' -EndDate '2018-08-19 23:59' -ExcludeJobSteps | ConvertTo-DbaTimeline | Out-File C:\temp\DbaAgentJobHistory.html -Encoding ASCII
Invoke-Item -Path C:\temp\DbaAgentJobHistory.html

# Prettier
Start-Process https://dbatools.io/wp-content/uploads/2018/08/Get-DbaAgentJobHistory-html.jpg

```

```ruby

# Availability Groups
Invoke-Item 'C:\temp\psconf\click-a-rama.mp4'
```


```ruby
# All in one, no hassle - includes credentials!
$docker1 = Get-DbaRegisteredServer -Name dockersql1
$docker2 = Get-DbaRegisteredServer -Name dockersql2

# setup a powershell splat (has docker been reset?)
$params = @{
    Primary = $docker1
    Secondary = $docker2
    Name = "test-ag"
    Database = "pubs"
    ClusterType = "None"
    SeedingMode = "Automatic"
    FailoverMode = "Manual"
    Confirm = $false
 }
 
# execute the command
 New-DbaAvailabilityGroup @params
```

```ruby

# Start-DbaMigration wraps 30+ commands
Start-DbaMigration -Source localhost -Destination localhost\sql2016 -UseLastBackup -Exclude BackupDevices, SysDbUserObjects -WarningAction SilentlyContinue | Out-GridView
```

```ruby
# Wraps like 20
Export-DbaInstance -SqlInstance localhost\sql2017 -Path C:\temp\dr
Get-ChildItem -Path C:\temp\dr -Recurse -Filter *database* | Invoke-Item
```

```ruby
#region BONUS
Get-ChildItem C:\github\community-presentations\*ps1 -Recurse | Invoke-DbatoolsRenameHelper | Out-GridView
```

```ruby
# Diagnostic
Invoke-DbaDiagnosticQuery -SqlInstance localhost\sql2017 | Export-DbaDiagnosticQuery -Outvariable exports
$exports | Select -Skip 3 -First 1 | Invoke-Item
```

```ruby
# Ola Hallengren supported
Install-DbaMaintenanceSolution -SqlInstance localhost, localhost\sql2016, localhost\sql2017 -ReplaceExisting -InstallJobs
```

```ruby
# ConvertTo-DbaXESession
Get-DbaTrace -SqlInstance localhost\sql2017 -Id 1 | ConvertTo-DbaXESession -Name 'Default Trace' | Start-DbaXESession
```

```ruby
 # Wraps a bunch
Test-DbaLastBackup -SqlInstance localhost -Destination localhost\sql2016 | Select * | Out-GridView
```

```

```ruby
# Get-DbaRegisteredServer, aliased
Get-DbaRegisteredServer
Get-DbaRegisteredServer -SqlInstance localhost\sql2016 -IncludeLocal
Get-DbaRegisteredServer -Group onprem | Get-DbaDatabase | Select SqlInstance, Name | Format-Table -AutoSiz
```

```ruby
# Connect-DbaInstance, supports everything!
Get-DbaRegisteredServer -Name azuresqldb | Connect-DbaInstance
```

```ruby
# CSV galore!
Get-ChildItem C:\temp\psconf\csv
Get-ChildItem C:\temp\psconf\csv | Import-DbaCsv -SqlInstance localhost\sql2017 -Database tempdb -AutoCreateTable -Encoding UTF8
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select top 10 * from [jmfh-year]"
```

```ruby
# Write-DbaDbTableData
Get-ChildItem -File | Write-DbaDbTableData -SqlInstance localhost\sql2017 -Database tempdb -Table files -AutoCreateTable
Get-ChildItem -File | Select *
Invoke-DbaQuery -SqlInstance localhost\sql2017 -Database tempdb -Query "Select * from files"
```

```ruby
# Gotta find it, run this once
Find-DbaInstance -ComputerName localhost
```

```ruby
# PII Management
Invoke-DbaDbPiiScan -SqlInstance localhost\sql2017 -Database AdventureWorks2014 | Out-GridView
```

```ruby
# Mask that
New-DbaDbMaskingConfig -SqlInstance localhost\sql2017 -Database AdventureWorks2014 -Table EmployeeDepartmentHistory, Employee -Path C:\temp | Invoke-Item
Invoke-DbaDbDataMasking -SqlInstance localhost\sql2017 -FilePath 'C:\github\community-presentations\chrissy-lemaire\mask.json' -ExcludeTable EmployeeDepartmentHistory
```

```ruby
# Very Large Database Migration
$params = @{
    Source                          = 'localhost'
    Destination                     = 'localhost\sql2017'
    Database                        = 'shipped'
    SharedPath                      = '\\localhost\backups'
    BackupScheduleFrequencyType     = 'Daily'
    BackupScheduleFrequencyInterval = 1
    CompressBackup                  = $true
    CopyScheduleFrequencyType       = 'Daily'
    CopyScheduleFrequencyInterval   = 1
    GenerateFullBackup              = $true
    Force                           = $true
}


Invoke-DbaDbLogShipping @params

# Recover when ready
Invoke-DbaDbLogShipRecovery -SqlInstance localhost\sql2017 -Database shipped
```

```ruby
# Install-DbaInstance / Update-DbaInstance
Update-DbaInstance -ComputerName sql2017 -Path \\dc\share\patch -Credential base\ctrlb
Invoke-Item 'C:\temp\psconf\Patch several SQL Servers at once using Update-DbaInstance by Kirill Kravtsov.mp4'
```

```ruby
# Spaghetti!
New-DbaDiagnosticAdsNotebook -TargetVersion 2017 -Path C:\temp\myNotebook.ipynb | Invoke-Item


# Dope - dbatools.io/timeline
Get-DbaAgentJobHistory -SqlInstance localhost\sql2017 -StartDate '2016-08-18 00:00' -EndDate '2018-08-19 23:59' -ExcludeJobSteps | ConvertTo-DbaTimeline | Out-File C:\temp\DbaAgentJobHistory.html -Encoding ASCII
Invoke-Item -Path C:\temp\DbaAgentJobHistory.html

# Prettier
Start-Process https://dbatools.io/wp-content/uploads/2018/08/Get-DbaAgentJobHistory-html.jpg

```

```ruby

# Availability Groups
Invoke-Item 'C:\temp\psconf\click-a-rama.mp4'
```


```ruby
# All in one, no hassle - includes credentials!
$docker1 = Get-DbaRegisteredServer -Name dockersql1
$docker2 = Get-DbaRegisteredServer -Name dockersql2

# setup a powershell splat (has docker been reset?)
$params = @{
    Primary = $docker1
    Secondary = $docker2
    Name = "test-ag"
    Database = "pubs"
    ClusterType = "None"
    SeedingMode = "Automatic"
    FailoverMode = "Manual"
    Confirm = $false
 }
 
# execute the command
 New-DbaAvailabilityGroup @params
```

```ruby

# Start-DbaMigration wraps 30+ commands
Start-DbaMigration -Source localhost -Destination localhost\sql2016 -UseLastBackup -Exclude BackupDevices, SysDbUserObjects -WarningAction SilentlyContinue | Out-GridView
```

```ruby
# Wraps like 20
Export-DbaInstance -SqlInstance localhost\sql2017 -Path C:\temp\dr
Get-ChildItem -Path C:\temp\dr -Recurse -Filter *database* | Invoke-Item
```

```ruby
#region BONUS
Get-ChildItem C:\github\community-presentations\*ps1 -Recurse | Invoke-DbatoolsRenameHelper | Out-GridView
```

```ruby
# Diagnostic
Invoke-DbaDiagnosticQuery -SqlInstance localhost\sql2017 | Export-DbaDiagnosticQuery -Outvariable exports
$exports | Select -Skip 3 -First 1 | Invoke-Item
```

```ruby
# Ola Hallengren supported
Install-DbaMaintenanceSolution -SqlInstance localhost, localhost\sql2016, localhost\sql2017 -ReplaceExisting -InstallJobs
```

```ruby
# ConvertTo-DbaXESession
Get-DbaTrace -SqlInstance localhost\sql2017 -Id 1 | ConvertTo-DbaXESession -Name 'Default Trace' | Start-DbaXESession
```

```ruby
 # Wraps a bunch
Test-DbaLastBackup -SqlInstance localhost -Destination localhost\sql2016 | Select * | Out-GridView
```
