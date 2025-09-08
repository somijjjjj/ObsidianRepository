


```bash
scp -i "G:\내 드라이브\insider\somijjjj-server-ssh-key.pem" -P 22 -r ubuntu@158.180.95.52:/home/ubuntu/all_dump.sql C:\backup
```
- server -> window


```bash
scp -i "G:\내 드라이브\insider\s2it-server-ssh-key.pem" -P 22 -r C:\backup\all_dump.sql ubuntu@144.24.82.57:/home/ubuntu
```
- window -> server



```bash
scp -i "G:\내 드라이브\insider\somijjjj-server-ssh-key.pem" -P 22 -r "G:\내 드라이브\insider\s2it-server-ssh-key.pem" ubuntu@158.180.95.52:/home/ubuntu
```