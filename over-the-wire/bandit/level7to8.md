### Walkthrough

```
izabellamelo@10-48-48-11 ~ % ssh bandit7@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit7@bandit.labs.overthewire.org's password: HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

bandit7@bandit:~$ ls
data.txt
```

this file is too big, so we use **grep** to find line with "millionth" word

```
bandit7@bandit:~$ grep -i "millionth" data.txt 
millionth	cvX2JJa4CFALtqS87jk27qwqGhBM9plV
bandit7@bandit:~$ 
```

FLAG -> cvX2JJa4CFALtqS87jk27qwqGhBM9plV
