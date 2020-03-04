---
title: "Post: Get CPU"
categories:
  - Blog
tags:
  - Get CPU
  - Post Formats
---

Simple could function to get CPU of a Target computer

```ruby
	$property = “systemname”,”maxclockspeed”,”addressWidth”,“numberOfCores”, “NumberOfLogicalProcessors”
	Get-WmiObject -class win32_processor -Property  $property -ComputerName $serverName | Select-Object -Property $property
```
