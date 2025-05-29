
```
ps
```

- ps : process status 
리눅스와 유닉스 계열 운영체제에서 현재 실행 중인 프로세스에 대한 정보를 확인하는 데 사용됩니다.

ps는 실행 중인 프로세스의 PID(프로세스 ID), 사용자, CPU/메모리 사용량, 실행 경로 등 다양한 정보를 제공합니다.


```
ubuntu@was-instance:~/web$ ps aux | grep node
root     3337103  0.1  2.7 11132112 26928 ?      Ssl  Jan22   2:37 node /home/ubuntu/web/server.js
ubuntu   3349003  0.0  0.2   7008  2304 pts/0    S+   20:37   0:00 grep --color=auto node
```