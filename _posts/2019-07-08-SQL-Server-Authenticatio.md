---
title: "Post: SQL Server Authentication (Kerberos)"
categories:
  - Blog
tags:
  - link
  - Post Formats
  - PowerShell
  - SQL
link: 
GitHub: https://gobiman.github.io/Blog/
---
After changing the SQL service account, we faced a problem which the SQL server was running but we were unable to connect to the instance.

The error was
> **"The target principal name is incorrect. Cannot generate SSPI context. (.Net SqlClient Data Provider)"
The connection failed due to Kerberos connection with AD.**

The following pages explained how to troubleshoot the issue,
How to use Kerberos to authenticate against AD [registera service principal name for kerberos connections](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections?view=sql-server-2017)

Tested SPN but came with errors & SPN are missing

```ruby
import-module dbatools
Test-DbaSpn -ComputerName TargetServer | where {$_.isset -eq $false} | set-dbaspn -ServiceAccount DomainName\ServiceAccount -WhatIf
Test-DbaSpn -ComputerName TargetServer | where {$_.isset -eq $false} | set-dbaspn -ServiceAccount DomainName\ServiceAccount
```

This did not work becuase of the existing service account assoicated with the target server.

```ruby
Used setspn help command
    setspn /?
```

```ruby
Querired the SPN for SQL Server 
    setspn -Q MSSQLSvc/TargetServer.FQDN
```

```ruby
Delete the exiting SPN with & without the port (1433) as shown below
    setspn -D MSSQLSvc/TargetServer.FQDN DomainName\ServiceAccount # OR usev remove-dbaspn
    setspn -D MSSQLSvc/TargetServer.FQDN:1433 DomainName\ServiceAccount # OR usev remove-dbaspn
```

```ruby
Ran the following command to register the service account for this SQL server
    Test-DbaSpn -ComputerName TargetServer | where {$_.isset -eq $false} | set-dbaspn -ServiceAccount DomainName\ServiceAccount
```

```ruby
Once connected to the instance, open query & issue the command
    Invoke-DbaQuery -SqlInstance TargetServer -Query "SELECT auth_scheme FROM sys.dm_exec_connections WHERE session_id = @@spid ;"
The result should be Kerberos
```
