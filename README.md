# MySQL: Backup + Репликация
## Домашнее задание - "Репликация mysql"
###Что нужно сделать?
В материалах приложены ссылки на вагрант для репликации и дамп базы bet.dmp  
Базу развернуть на мастере и настроить так, чтобы реплицировались таблицы:  
| bookmaker          |  
| competition        |  
| market             |  
| odds               |  
| outcome            |


Настроить GTID репликацию
Варианты которые принимаются к сдаче
-    рабочий вагрантафайл
-    скрины или логи SHOW TABLES

Установка Percona-Server
```
vagrant@master:~$ history
    1  sudo apt update
    2  sudo apt install gnupg2
    3  wget https://repo.percona.com/apt/percona-release_latest.$(lsb_release -sc)_all.deb
    4  sudo dpkg -i percona-release_latest.$(lsb_release -sc)_all.deb
    5  sudo apt update
    6  sudo percona-release enable-only ps-57
    7  sudo apt install percona-server-server-5.7
    8  history

vagrant@slave:~$ sudo apt install percona-server-server-5.7
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  debsums libdpkg-perl libfile-fcntllock-perl libfile-fnmatch-perl libmecab2 mysql-common
  percona-server-client-5.7 percona-server-common-5.7
Suggested packages:
  debian-keyring gcc | c-compiler binutils bzr
The following NEW packages will be installed:
  debsums libdpkg-perl libfile-fcntllock-perl libfile-fnmatch-perl libmecab2 mysql-common
  percona-server-client-5.7 percona-server-common-5.7 percona-server-server-5.7
0 upgraded, 9 newly installed, 0 to remove and 65 not upgraded.
Need to get 75.1 MB of archives.
After this operation, 239 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://archive.ubuntu.com/ubuntu focal/main amd64 mysql-common all 5.8+1.0.5ubuntu2 [7496 B]
Get:2 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 libdpkg-perl all 1.19.7ubuntu3.2 [231 kB]
Get:3 http://archive.ubuntu.com/ubuntu focal/universe amd64 libfile-fnmatch-perl amd64 0.02-2build6 [10.1 kB]
Get:4 http://archive.ubuntu.com/ubuntu focal/universe amd64 debsums all 2.2.5 [42.2 kB]
Get:5 http://archive.ubuntu.com/ubuntu focal/main amd64 libmecab2 amd64 0.996-10build1 [233 kB]
Get:6 http://archive.ubuntu.com/ubuntu focal/main amd64 libfile-fcntllock-perl amd64 0.22-3build4 [33.1 kB]
Get:7 http://repo.percona.com/ps-57/apt focal/main amd64 percona-server-common-5.7 amd64 5.7.44-48-1.focal [877 kB]
Get:8 http://repo.percona.com/ps-57/apt focal/main amd64 percona-server-client-5.7 amd64 5.7.44-48-1.focal [14.0 MB]
Get:9 http://repo.percona.com/ps-57/apt focal/main amd64 percona-server-server-5.7 amd64 5.7.44-48-1.focal [59.7 MB]
Fetched 75.1 MB in 10min 56s (114 kB/s)
Preconfiguring packages ...
Selecting previously unselected package mysql-common.
(Reading database ... 63794 files and directories currently installed.)
Preparing to unpack .../mysql-common_5.8+1.0.5ubuntu2_all.deb ...
Unpacking mysql-common (5.8+1.0.5ubuntu2) ...
Selecting previously unselected package libdpkg-perl.
Preparing to unpack .../libdpkg-perl_1.19.7ubuntu3.2_all.deb ...
Unpacking libdpkg-perl (1.19.7ubuntu3.2) ...
Selecting previously unselected package libfile-fnmatch-perl.
Preparing to unpack .../libfile-fnmatch-perl_0.02-2build6_amd64.deb ...
Unpacking libfile-fnmatch-perl (0.02-2build6) ...
Selecting previously unselected package debsums.
Preparing to unpack .../archives/debsums_2.2.5_all.deb ...
Unpacking debsums (2.2.5) ...
Setting up mysql-common (5.8+1.0.5ubuntu2) ...
update-alternatives: using /etc/mysql/my.cnf.fallback to provide /etc/mysql/my.cnf (my.cnf) in auto mode
Setting up libdpkg-perl (1.19.7ubuntu3.2) ...
Setting up libfile-fnmatch-perl (0.02-2build6) ...
Setting up debsums (2.2.5) ...
Selecting previously unselected package percona-server-common-5.7.
(Reading database ... 63996 files and directories currently installed.)
Preparing to unpack .../percona-server-common-5.7_5.7.44-48-1.focal_amd64.deb ...
Unpacking percona-server-common-5.7 (5.7.44-48-1.focal) ...
Selecting previously unselected package percona-server-client-5.7.
Preparing to unpack .../percona-server-client-5.7_5.7.44-48-1.focal_amd64.deb ...
Unpacking percona-server-client-5.7 (5.7.44-48-1.focal) ...
Selecting previously unselected package libmecab2:amd64.
Preparing to unpack .../libmecab2_0.996-10build1_amd64.deb ...
Unpacking libmecab2:amd64 (0.996-10build1) ...
Selecting previously unselected package percona-server-server-5.7.
Preparing to unpack .../percona-server-server-5.7_5.7.44-48-1.focal_amd64.deb ...
Unpacking percona-server-server-5.7 (5.7.44-48-1.focal) ...
Selecting previously unselected package libfile-fcntllock-perl.
Preparing to unpack .../libfile-fcntllock-perl_0.22-3build4_amd64.deb ...
Unpacking libfile-fcntllock-perl (0.22-3build4) ...
Setting up libmecab2:amd64 (0.996-10build1) ...
Setting up libfile-fcntllock-perl (0.22-3build4) ...
Setting up percona-server-common-5.7 (5.7.44-48-1.focal) ...
update-alternatives: using /etc/mysql/percona-server.cnf to provide /etc/mysql/my.cnf (my.cnf) in auto mode
Setting up percona-server-client-5.7 (5.7.44-48-1.focal) ...
Setting up percona-server-server-5.7 (5.7.44-48-1.focal) ...


 * Percona Server is distributed with several useful UDF (User Defined Function) from Percona Toolkit.
 * Run the following commands to create these functions:

        mysql -e "CREATE FUNCTION fnv1a_64 RETURNS INTEGER SONAME 'libfnv1a_udf.so'"
        mysql -e "CREATE FUNCTION fnv_64 RETURNS INTEGER SONAME 'libfnv_udf.so'"
        mysql -e "CREATE FUNCTION murmur_hash RETURNS INTEGER SONAME 'libmurmur_udf.so'"

 * See http://www.percona.com/doc/percona-server/5.7/management/udf_percona_toolkit.html for more details


Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service → /lib/systemd/system/mysql.service.
Processing triggers for systemd (245.4-4ubuntu3.23) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.16) ...
```
Копируем конфиг сервера и перезагружаем 
```
vagrant@master:~$ sudo cp /vagrant/conf/conf.d/* /etc/mysql/conf.d/
vagrant@master:~$ ls -la /etc/mysql/conf.d/
total 36
drwxr-xr-x 2 root root 4096 Jan 12 16:35 .
drwxr-xr-x 4 root root 4096 Jan 12 16:30 ..
-rw-r--r-- 1 root root  207 Jan 12 16:35 01-base.cnf
-rw-r--r-- 1 root root   48 Jan 12 16:35 02-max-connections.cnf
-rw-r--r-- 1 root root  487 Jan 12 16:35 03-performance.cnf
-rw-r--r-- 1 root root   66 Jan 12 16:35 04-slow-query.cnf
-rw-r--r-- 1 root root  385 Jan 12 16:35 05-binlog.cnf
vagrant@master:~$ sudo systemctl start mysql
mysql -uroot -p'*mulP>&68v/A'
```
Подключаемся. Меняем пароль. Проверяем настройки и создаем базу bet.
```
vagrant@master:~$ mysql -uroot -p'*mulP>&68v/A'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.44-48 Percona Server (GPL), Release '48', Revision '497f936a373'

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER USER() IDENTIFIED BY 'Otus2025';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @@server_id;
+-------------+
| @@server_id |
+-------------+
|           1 |
+-------------+
1 row in set (0.00 sec)

mysql> SHOW VARIABLES LIKE 'gtid_mode';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| gtid_mode     | ON   |
+---------------+-------+
1 row in set (0.01 sec)

mysql> CREATE DATABASE bet;
Query OK, 1 row affected (0.00 sec)

```
Копируем дамп bet.dmp на master хост
```
vagrant@master:~$ mysql -uroot -p -D bet < /vagrant/bet.dmp
```
Проверим наполнение базы. Создадим полþзователя для репликации и даем ему права репликацию
```
mysql> USE bet;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+------------------+
| Tables_in_bet    |
+------------------+
| bookmaker        |
| competition      |
| events_on_demand |
| market           |
| odds             |
| outcome          |
| v_same_event     |
+------------------+
7 rows in set (0.00 sec)

mysql> CREATE USER 'repl'@'%' IDENTIFIED BY '!OtusLinux2025';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT user,host FROM mysql.user where user='repl';
+------+------+
| user | host |
+------+------+
| repl | %    |
+------+------+
1 row in set (0.00 sec)

mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' IDENTIFIED BY '!OtusLinux2025';
Query OK, 0 rows affected, 1 warning (0.01 sec)
```
Дампим базу
```
root@master:~# mysqldump --all-databases --triggers --routines --master-data --ignore-table=bet.events_on_demand                                                                                                 --ignore-table=bet.v_same_event -uroot -p > master.sql
Enter password:
Warning: A partial dump from a server that has GTIDs will by default include the GTIDs of all transactions, even                                                                                                 those that changed suppressed parts of the database. If you don't want to restore GTIDs, pass --set-gtid-purged                                                                                                =OFF. To make a complete dump, pass --all-databases --triggers --routines --events.
root@master:~# ls -la
total 1016
drwx------  5 root root    4096 Jan 12 16:59 .
drwxr-xr-x 20 root root    4096 Jan 12 16:08 ..
-rw-r--r--  1 root root    3106 Dec  5  2019 .bashrc
-rw-------  1 root root      36 Jan 12 16:52 .lesshst
drwxr-xr-x  3 root root    4096 Jan 12 16:43 .local
-rw-r--r--  1 root root     161 Dec  5  2019 .profile
drwx------  2 root root    4096 Jan 12 16:08 .ssh
-rw-r--r--  1 root root 1006098 Jan 12 16:59 master.sql
drwx------  3 root root    4096 Jan 12 16:08 snap
```
Переключаемся на slave.  
Правим в /etc/my.cnf.d/01-basics.cnf директиву server-id = 2  
Раскомментируем в /etc/my.cnf.d/05-binlog.cnf строки  
```
root@slave:~# cp /vagrant/conf/conf.d/* /etc/mysql/conf.d/
root@slave:~# nano /etc/my.cnf.d/01-basics.cnf
root@slave:~# nano /etc/mysql/conf.d/01-basics.cnf
root@slave:~# nano /etc/mysql/conf.d/01-base.cnf
.bashrc   .local/   .profile  .ssh/     snap/
root@slave:~# nano /etc/mysql/conf.d/01-base.cnf
root@slave:~# nano /etc/mysql/conf.d/05-binlog.cnf
root@slave:~# rm /etc/mysql/conf.d/mysql
mysql.cnf      mysqldump.cnf
root@slave:~# rm /etc/mysql/conf.d/mysql.cnf
root@slave:~# rm /etc/mysql/conf.d/mysqldump.cnf
root@slave:~# systemctl restart mysql.service
root@slave:~# cp /vagrant/master.sql /mnt/master.sql
```
Заливаем дамп на master и убеждаемся что база есть и она без лишних таблиц
```
root@slave:~# mysql -uroot -p'*mulP>&68v/A'
mysql> ALTER USER USER() IDENTIFIED BY 'Otus2025';
Query OK, 0 rows affected (0.00 sec)

mysql> SOURCE /mnt/master.sql
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)
--------
--------
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected, 1 warning (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> SHOW DATABASES LIKE 'bet';
+----------------+
| Database (bet) |
+----------------+
| bet            |
+----------------+
1 row in set (0.00 sec)

mysql> USE bet;
Database changed
mysql> SHOW TABLES;
+---------------+
| Tables_in_bet |
+---------------+
| bookmaker     |
| competition   |
| market        |
| odds          |
| outcome       |
+---------------+
5 rows in set (0.00 sec)

```
Подключаем и запускаем slave
```
mysql> CHANGE MASTER TO MASTER_HOST = "192.168.11.150", MASTER_PORT = 3306, MASTER_USER = "repl", MASTER_PASSWORD = "!OtusLinux2025", MASTER_AUTO_POSITION = 1;
Query OK, 0 rows affected, 2 warnings (0.01 sec)

mysql> START SLAVE;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.11.150
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000006
          Read_Master_Log_Pos: 450
               Relay_Log_File: slave-relay-bin.000002
                Relay_Log_Pos: 663
        Relay_Master_Log_File: mysql-bin.000006
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table: bet.events_on_demand,bet.v_same_event
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 450
              Relay_Log_Space: 870
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 1
                  Master_UUID: 7f2b3fc1-d102-11ef-a02d-02d2a9007a16
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set: 7f2b3fc1-d102-11ef-a02d-02d2a9007a16:1
            Executed_Gtid_Set: 7f2b3fc1-d102-11ef-a02d-02d2a9007a16:1,
938c5319-d102-11ef-b15d-02d2a9007a16:1
                Auto_Position: 1
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.00 sec)
```
Проверим репликацию в действии на master
```
mysql> USE bet;
mysql> INSERT INTO bookmaker (id,bookmaker_name) VALUES(1,'1xbet');
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 sec)

```
На slave
```
mysql>  USE bet;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 sec)
```
![изображение](https://github.com/user-attachments/assets/f848af64-75d6-4fc9-8a56-9c0b4718f716)
![изображение](https://github.com/user-attachments/assets/4de07514-d752-4565-b39c-66291c5d9bdf)

