---
aliases: []
---

- npm 먼저 설치
	- apt install npm
- pm2 설치
	- npm install pm2 -g
- 프로세스 상태 확인
	- pm2 list
	
---

- pm2 로 실행한 애플리케이션 확
```
oot@was-instance:/home/ubuntu/web# pm2 show insider
 Describing process with id 2 - name insider
┌───────────────────┬───────────────────────────────────┐
│ status            │ online                            │
│ name              │ insider                           │
│ namespace         │ default                           │
│ version           │ N/A                               │
│ restarts          │ 0                                 │
│ uptime            │ 8m                                │
│ script path       │ /usr/local/bin/yarn               │
│ script args       │ start                             │
│ error log path    │ /root/.pm2/logs/insider-error.log │
│ out log path      │ /root/.pm2/logs/insider-out.log   │
│ pid path          │ /root/.pm2/pids/insider-2.pid     │
│ interpreter       │ node                              │
│ interpreter args  │ N/A                               │
│ script id         │ 2                                 │
│ exec cwd          │ /home/ubuntu/web                  │
│ exec mode         │ cluster_mode                      │
│ node.js version   │ 12.22.9                           │
│ node env          │ N/A                               │
│ watch & reload    │ ✘                                 │
│ unstable restarts │ 0                                 │
│ created at        │ 2025-01-18T08:49:53.947Z          │
└───────────────────┴───────────────────────────────────┘
 Actions available
┌────────────────────────┐
│ km:heapdump            │
│ km:cpu:profiling:start │
│ km:cpu:profiling:stop  │
│ km:heap:sampling:start │
│ km:heap:sampling:stop  │
└────────────────────────┘
 Trigger via: pm2 trigger insider <action_name>

 Code metrics value
┌────────────────────────┬───────────┐
│ Used Heap Size         │ 22.71 MiB │
│ Heap Usage             │ 92.08 %   │
│ Heap Size              │ 24.66 MiB │
│ Event Loop Latency p95 │ 1.96 ms   │
│ Event Loop Latency     │ 0.17 ms   │
│ Active handles         │ 2         │
│ Active requests        │ 0         │
└────────────────────────┴───────────┘
 Divergent env variables from local env


 Add your own code metrics: http://bit.ly/code-metrics
 Use `pm2 logs insider [--lines 1000]` to display logs
 Use `pm2 env 2` to display environment variables
 Use `pm2 monit` to monitor CPU and Memory usage insider
 Describing process with id 3 - name insider
┌───────────────────┬───────────────────────────────────┐
│ status            │ online                            │
│ name              │ insider                           │
│ namespace         │ default                           │
│ version           │ N/A                               │
│ restarts          │ 0                                 │
│ uptime            │ 8m                                │
│ script path       │ /usr/local/bin/yarn               │
│ script args       │ start                             │
│ error log path    │ /root/.pm2/logs/insider-error.log │
│ out log path      │ /root/.pm2/logs/insider-out.log   │
│ pid path          │ /root/.pm2/pids/insider-3.pid     │
│ interpreter       │ node                              │
│ interpreter args  │ N/A                               │
│ script id         │ 3                                 │
│ exec cwd          │ /home/ubuntu/web                  │
│ exec mode         │ cluster_mode                      │
│ node.js version   │ 12.22.9                           │
│ node env          │ N/A                               │
│ watch & reload    │ ✘                                 │
│ unstable restarts │ 0                                 │
│ created at        │ 2025-01-18T08:49:54.445Z          │
└───────────────────┴───────────────────────────────────┘
 Actions available
┌────────────────────────┐
│ km:heapdump            │
│ km:cpu:profiling:start │
│ km:cpu:profiling:stop  │
│ km:heap:sampling:start │
│ km:heap:sampling:stop  │
└────────────────────────┘
 Trigger via: pm2 trigger insider <action_name>

 Code metrics value
┌────────────────────────┬───────────┐
│ Used Heap Size         │ 22.35 MiB │
│ Heap Usage             │ 91.56 %   │
│ Heap Size              │ 24.41 MiB │
│ Event Loop Latency p95 │ 2.24 ms   │
│ Event Loop Latency     │ 0.18 ms   │
│ Active handles         │ 2         │
│ Active requests        │ 0         │
└────────────────────────┴───────────┘
 Divergent env variables from local env


 Add your own code metrics: http://bit.ly/code-metrics
 Use `pm2 logs insider [--lines 1000]` to display logs
 Use `pm2 env 3` to display environment variables
 Use `pm2 monit` to monitor CPU and Memory usage insider

```



```
root@was-instance:/home/ubuntu/web# sudo apt-get install -y nodejs
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following packages will be upgraded:
  nodejs
1 upgraded, 0 newly installed, 0 to remove and 56 not upgraded.
Need to get 29.6 MB of archives.
After this operation, 59.5 MB of additional disk space will be used.
Get:1 https://deb.nodesource.com/node_18.x nodistro/main amd64 nodejs amd64 18.20.5-1nodesource1 [29.6 MB]
Fetched 29.6 MB in 5s (6530 kB/s)
(Reading database ... 136575 files and directories currently installed.)
Preparing to unpack .../nodejs_18.20.5-1nodesource1_amd64.deb ...
Detected old npm client, removing...
Unpacking nodejs (18.20.5-1nodesource1) over (16.20.2-1nodesource1) ...
Setting up nodejs (18.20.5-1nodesource1) ...
Processing triggers for man-db (2.10.2-1) ...
Scanning processes...
Scanning candidates...
Scanning linux images...

Restarting services...
Service restarts being deferred:
 /etc/needrestart/restart.d/dbus.service
 systemctl restart networkd-dispatcher.service
 systemctl restart systemd-logind.service
 systemctl restart unattended-upgrades.service
 systemctl restart user@1001.service

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.
root@was-instance:/home/ubuntu/web# node -v
npm -v
v18.20.5
10.8.2
root@was-instance:/home/ubuntu/web# npm run build

> insider@0.0.0 build
> tsc -b && vite build

vite v6.0.7 building for production...
✓ 60 modules transformed.
dist/index.html                   0.40 kB │ gzip:  0.30 kB
dist/assets/index-CSZsnhRY.css   10.85 kB │ gzip:  2.95 kB
dist/assets/index-D8qBQRVW.js   226.83 kB │ gzip: 90.33 kB
✓ built in 9.57s
root@was-instance:/home/ubuntu/web# ll
total 188
drwxrwxr-x   7 ubuntu   ubuntu    4096 Jan 18 18:01 ./
drwxr-x---  10 ubuntu   ubuntu    4096 Jan 18 15:35 ../
drwxrwxr-x   8 ubuntu   ubuntu    4096 Jan 13 23:32 .git/
-rw-rw-r--   1 ubuntu   ubuntu     260 Jan 12 23:54 .gitignore
-rw-rw-r--   1 ubuntu   ubuntu     230 Jan 12 23:54 .prettierrc
-rw-rw-r--   1 ubuntu   ubuntu    1607 Jan 12 23:54 README.md
drwxr-xr-x   3 www-data www-data  4096 Jan 21 22:13 dist/
-rw-rw-r--   1 ubuntu   ubuntu     740 Jan 12 23:54 eslint.config.js
-rw-rw-r--   1 ubuntu   ubuntu     320 Jan 18 14:22 index.html
-rw-rw-r--   1 ubuntu   ubuntu     437 Jan 13 01:00 insider-pm2.json
drwxrwxr-x 205 ubuntu   ubuntu   12288 Jan 13 01:20 node_modules/
-rw-rw-r--   1 ubuntu   ubuntu     940 Jan 13 01:20 package.json
-rw-rw-r--   1 ubuntu   ubuntu      80 Jan 12 23:54 postcss.config.js
drwxrwxr-x   2 ubuntu   ubuntu    4096 Jan 12 23:54 public/
drwxrwxr-x   5 ubuntu   ubuntu    4096 Jan 18 17:37 src/
-rw-rw-r--   1 ubuntu   ubuntu     691 Jan 13 23:32 tailwind.config.js
-rw-rw-r--   1 ubuntu   ubuntu     665 Jan 12 23:54 tsconfig.app.json
-rw-rw-r--   1 ubuntu   ubuntu     119 Jan 12 23:54 tsconfig.json
-rw-rw-r--   1 ubuntu   ubuntu     593 Jan 12 23:54 tsconfig.node.json
-rw-rw-r--   1 ubuntu   ubuntu     164 Jan 13 01:12 vite.config.ts
-rw-rw-r--   1 ubuntu   ubuntu   99413 Jan 13 01:20 yarn.lock

```


### pm2 List 서버 제거

```
pm2 delete 4
```
 

