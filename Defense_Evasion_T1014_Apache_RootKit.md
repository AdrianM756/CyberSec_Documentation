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
<br>

## Targetting Webmin

Based on the Nmap scan result, ```Webmin``` is running on version ```1.920```. We can try and run [searchsploit](https://www.exploit-db.com/searchsploit) query to search the [exploit-db exploit database](https://www.exploit-db.com/) for any known vulnerabilities affecting ```Webmin 1.920```. To perform the following, we will use the following commands:
```
searchsploit "Webmin 1.920"
```
<br>

This is the result that we get:
<br>
<img width="1895" height="174" alt="image" src="https://github.com/user-attachments/assets/c614cb6e-4087-4d62-993d-06d44a9d4cd4" />
<br>

Based on the on the result, two of the exploits are accessible in the form of [Metasploit](https://www.metasploit.com/) Modules and one exploit is in the form of a shell script. But before we do anything like running ```metasploit```, Let us first research more on the vulnerabilities to know more about it.

For Example, The ```Webmin <= 1.920 - Unauthenticated Remote Command Execution``` vulnerability with a CVE code/identifier: [CVE-2019-15107](https://nvd.nist.gov/vuln/detail/cve-2019-15107), when successfully executed, allows remote attackers to execute arbitrary commands with root privileges. However, this will only work whether the Webmin server running on the target web server has the "Password Change" functionality enabled. If this functionality is disabled, the vulnerability cannot be exploited.
<br>

## Analyzing the exploit

Before we run the exploit to the target, Let us first check it's content. We can copy the exploit script from the local "exploitdb" exploit directory to our current working directory for analysis using this command:
```
cp /usr/share/exploitdb/exploits/linux/webapps/47293.sh .
```
<br>

To view the content, we will use the ```cat``` command.
```
cat 47293.sh
```
<br>
<img width="1902" height="600" alt="image" src="https://github.com/user-attachments/assets/03f8fba0-3b8f-4aa2-9197-20be12ac7357" />
<br>

Based on the content, itdoes leverage the ```password_change.cgi``` script used to facilitate password changes and replaces the value of the "old" password parameter with a system command, in this case, the UNIX ```id``` command. The script will determine whether the Webmin server is vulnerable based on the response.
<br>

## Run the exploit

To run the exploit, let us first set the file as an executable file using the ```chmod```command.
```
chmood +x 47293.sh
```
<br>

Let's execute the exploit now.
```
./47293.sh http://<IP ADDRESS OF THE WEB SERVER>:10000
```
<br>








