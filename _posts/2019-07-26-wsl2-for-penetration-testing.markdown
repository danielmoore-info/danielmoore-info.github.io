---
layout: post
title:  "WSL2 for Penetration Testing"
date:   2019-07-26 10:25:49 +1000
categories: jekyll update
---

When WSL1 was released, I was excited at the possibility of being able to run docker, and have all my penetration testing tools in a Windows machine. However, as you probably know, WSL1 was quite the let down for these use cases. Firstly WSL1 only translated some Linux system calls to their appropriate Windows sytem calls. Secondly, many important tools did not run on WSL1, for example, nmap. Thirdly, I/O performance was horrible, this was especially noticeable when downloading many files during updated or cloning large github repositories.

Microsoft has recently announced [WSL2](https://devblogs.microsoft.com/commandline/announcing-wsl-2/) which will ship with a complete Linux Kernel. WSL2 will run the Linux Kernel in a 'lightweight VM', instead of trying to translate the system calls, the VM will deal with them.

I wanted to explore the possibility of using WSL2 during for penetration testing, instead of using virtual machines, as they have significant overhead, and WSL2 is supposed to have minimal impact on resource usage. The rest of this blog will look at getting early access to WSL2 and determining if it could be used for penetration testing.

## Getting WSL2

**Note: Opting into the program requires you to use a Microsoft online account, they will also change some of your privacy settings for "feedback"**

WSL2 is currently only available to users who have opted in to the Windows Insider Program. To join this program, users can go to **Settings** > **Update & Security** > **Windows Insider Program** and go through the process of joining the program.

Once you have joined the program, select the fast release track and then you will need to restart your computer. Once you have restarted, go to **Settings** > **Update & Security** and download the latest updates. You need a version of Windows that is released with WSL2 => Windows 10 Insider Preview build 18917. Download and apply these updates.

After applying the updates and restarting, open an Administrator Powershell and enable WSL2 with the following commands (this assumes you have already [enabled](https://docs.microsoft.com/en-us/windows/wsl/install-win10) WSL1):

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName VirtualMachinePlatform
wsl --set-default-version 2
```

You can the start your linux distro like normal. 

## WSL2 for Penetration Testing?

WSL1 was not great for penetration testing due to limited support for raw sockets, and no docker. WSL2 appears to have resolved these issues (kind of). 

Nmap running on Ubuntu WSL2 works perfectly, as shown in the screenshot below:
![WSL2 Nmap](/assets/nmap-wsl2.PNG)

I have also confirmed that it is possible to run docker containers in WSL2, for example, the empire docker container:

![WSL2 Empire](/assets/wsl2-empire.PNG)

WSL2 does have a few limitations which limit it's usefulness for penetration testing. Most notably:
- No USB or GPU passthrough -> Limitation for wireless penetration testing and password cracking.
- Requires Hyper-v which means VMware and VirtualBox versions < 6 cannot run.
- Networking is currently implemented via 'NAT' mode (As opposed to bridged). This means the Linux environment is on a different broadcast domain then the host machine, making layer 2 attacks impossible from WSL2. This networking also means listeners, such as those in Metasploit or Empire do not work out of the box. However, the following [Powershell script](https://github.com/microsoft/WSL/issues/4150) can be used to forward ports from the Windows machine to WSL2 hosts.

```powershell
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  Write-Output "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,4000,10000,3000,5000);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
Invoke-Expression "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
Invoke-Expression "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
Invoke-Expression "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  Invoke-Expression "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  Invoke-Expression "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```