## RootKit

Adversaries may use [RootKit](https://attack.mitre.org/techniques/T1014/) to hide the presence of programs, files, network connections, services, drivers, and other system components. Rootkits are programs that hide the existence of malware by intercepting/hooking and modifying operating system API calls that supply system information.

## Initial Access - Port Scanning & Service Enumeration

First, we need to perform an [Nmap](https://nmap.org/) service scan on the target web server to get an idea of what services it is running or hosting. We can use the command:
```
sudo nmap -Pn -sS -sV -T4 -p- <IP ADDRESS OF THE WEB SERVER>
```
<br>

This may take a few seconds for the result to appear. once done, you can get a result similar to this:
<br>
<img width="1029" height="270" alt="image" src="https://github.com/user-attachments/assets/07c7f48a-4b33-4f6c-b548-63b91832d41a" />
<br>

Based on the image above, There are multiple ports and services that are open. We will focus on on the port ```80(Apache Web Servver)``` and port ```10000(Webmin)```.

If we try to access it on the browser using the port ```:10000```, it direct us to the Webmin login.
<br>
<img width="977" height="554" alt="image" src="https://github.com/user-attachments/assets/1bd5e2eb-ee39-4e86-b381-ed570fe2b23b" />
<br>

## What is Webmin?

[Webmin](https://webmin.com/) is a free, open-source web-based control panel that allows system administrators to manage Unix-like servers:

* Configuring operating systems where seurs can configure operating system internals, such as users, disk quotas, services, and configuration files.

* Modifying and controlling open-source apps where users can modify and control open-source apps, such as BIND, Apache HTTP Server, PHP, and MySQL.

* Managing email forwarding where users can create accounts and set up email forwarding.







