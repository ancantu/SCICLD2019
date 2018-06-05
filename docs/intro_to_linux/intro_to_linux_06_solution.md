Note: Text following a pound sign `#` are comments

1) Identify which Stampede2 login node you are on (login1, login2, login3)
```
$ hostname
login1
$ hostname -f
login1.stampede2.tacc.utexas.edu
```

2) Remotely login to a different Stampede2 login node and list what files are available.
```
login1.stampede2$ ssh wallen@login2.stampede2.tacc.utexas.edu
login2.stampede2$ ls
... # the same files will be available on different login nodes
login2.stampede2$ ssh login3.stampede2.tacc.utexas.edu    # what happens if you omit the username?
login3.stampede2$ ssh login1                        # what happens if you omit most of the hostname?
login1.stampede2$
```

3) Logout until you are back to your original login node.
```
login1.stampede2$ logout
login3.stampede2$ logout
login2.stampede2$ logout
login1.stampede2$             # logout one more time and you will be back on the local system
```

4) Copy the file `chr21.fa` to your local computer.
```
# from new Terminal session
[local]$ ssh wallen@stampede2.tacc.utexas.edu:~/IntroToLinuxHPC/Lab02/chr21.fa ./
(enter password)
(enter token)
```

[Return](intro_to_linux_06.md)
