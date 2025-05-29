
```bash
scp -i "C:\Users\user\Desktop\insider\key.pem" -P 22 -r ubuntu@150.230.252.242:/data/* C:\backup

```
- server -> window

```bash
scp -i "C:\Users\user\Desktop\insider\s2it-server-ssh-key.key.pem" -P 22 -r C:\backup\* ubuntu@144.24.82.57:/home/ubuntu/backup_data/
scp -i "C:\Users\user\Desktop\insider\s2it-server-ssh-key.key.pem" -P 22 -r C:\backup\* ubuntu@144.24.82.57:/home/ubuntu/backup_data

```
```bash
scp -i "C:\Users\user\Desktop\insider\s2it-server-ssh-key.key.pem" -P 22 -r C:\backup\Insider\* ubuntu@144.24.82.57:/home/ubuntu
```
- window -> server