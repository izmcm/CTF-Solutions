### Goal

The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

### Walkthrough

```
izabellamelo@10-48-48-11 ~ % ssh bandit8@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit8@bandit.labs.overthewire.org's password: cvX2JJa4CFALtqS87jk27qwqGhBM9plV

bandit8@bandit:~$ ls
data.txt
```

this file is too big, so we use **sort** to sorts the file lexicographically and **uniq -u** to show the lines that are unique

```
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
bandit8@bandit:~$ 
```

FLAG -> UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
