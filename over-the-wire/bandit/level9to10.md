### Goal

The password for the next level is stored in the file data.txt in one of the few human-readable strings, beginning with several ‘=’ characters.

### Walkthrough

```
izabellamelo@10-48-48-11 ~ % ssh bandit9@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit9@bandit.labs.overthewire.org's password: UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

bandit9@bandit:~$ ls
data.txt

bandit9@bandit:~$ strings data.txt | grep ==
2========== the
========== password
========== isa
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

```

FLAG -> truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
