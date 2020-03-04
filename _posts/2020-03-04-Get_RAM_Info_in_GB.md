---
title: "Post: Get RAM"
categories:
  - Blog
tags:
  - Get RAM
  - Post Formats
---

Simple could function to get RAM of a Target computer

```ruby
$InstalledRAM = Get-WmiObject -Class Win32_ComputerSystem  -ComputerName $serverName
[Math]::Round(($InstalledRAM.TotalPhysicalMemory/ 1GB),2)
```
