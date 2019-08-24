## Passo a Passo

#### No terminal

```
$ nmap 10.10.10.140

Starting Nmap 7.70 ( https://nmap.org ) at 2019-07-28 16:07 -03
Nmap scan report for 10.10.10.140
Host is up (0.12s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
**22/tcp open  ssh**
80/tcp open  http
```

Com a porta SSH aberta, tentarei me conectar

nao deu

```
$ searchsploit 39838

----------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 Exploit Title                                                                                                                         |  Path
                                                                                                                                       | (/usr/share/exploitdb/)
----------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
Magento < 2.0.6 - Arbitrary Unserialize / Arbitrary Write File                                                                         | exploits/php/webapps/39838.php
----------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
Shellcodes: No Result
```