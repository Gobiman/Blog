---
title: "Post: Get RAM"
categories:
  - Blog
tags:
  - SSH connectivity to various clients
  - Post Formats
---

Connect using SSH from client (Win, Linux) to Server (Win, Linux) 

Client (Linux, Win) >> Server Linux

```ruby
ssh UserName@LinuxServer
prompts for password, Enter password
```

If this is first time connection, it will prompt for Yes/No & stores the server public key @ ~/.ssh/kown_hosts, otherwise it will ask for the password.

SSH using key-based authentication

Client Linux >> Serever Linux

```ruby
  ssh-keygen
  ll
  ssh-copy-id LinuxServer
  ssh UserName@LinuxServer
  ```

  This will generate pair of keys (Public & Private).
  copy the Public key to the Linux server 
  then login without prompting for password


  Client Win (PowerShell) >> Serever Linux

```ruby
  ssh-keygen
  cd ~/.ssh
  cat .\id_rsa.pub # copy content of public key file 
  ssh UserName@LinuxServer
  # become root
  vim /home/UserName/.ssh/authorized_keys
  # add content of public key into this file
  systemctl status sshd # check on SSH demon
  systemctl restart sshd # restart SSHD demon
  ```

  This will generate pair of keys (Public & Private).
  copy the Public key to the Linux server 
  then login without prompting for password

  ## Connecting to Windows server using SSH, 
  
  Requirements
  1. PWSH 6.0 or higher
  2. SSH installed on both client & server, on Win install OpenSSH
  
  OpenSSH can also be installed fron [Chocolatey](chocolatey.org)
  
  [OpenSSH installation](https://github.com/powershell/win32-openssh/wiki/install-win32-openssh)

  See [Powershell Remoting video](https://4sysops.com/archives/powershell-remoting-with-ssh-public-key-authentication/)
[OpenSSH installation](https://github.com/powershell/win32-openssh/wiki/install-win32-openssh)

[Blog, Remoting](https://adamtheautomator.com/ssh-with-powershell/)

```ruby
#EnterPS remote session [Creats an interactive remote session]
Enter-pssession -hostname Srv1 SSHTransport -username user1
```
when scripting you can use the new PS session Commandlet to open a persistent remote session.

configure opsnssh 