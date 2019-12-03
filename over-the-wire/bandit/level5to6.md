### Walkthrough

```
$ ssh bandit5@bandit.labs.overthewire.org -p 2220
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit5@bandit.labs.overthewire.org's password: koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```
**find**  
human-readable  
1033 bytes in size  
not executable  

```
bandit5@bandit:~$ find ./inhere/ -size 1033c ! -executable
./inhere/maybehere07/.file2

bandit5@bandit:~$ cat ./inhere/maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

FLAG -> DXjZPULLxYr17uwoI01bNLQbtFemEgo7