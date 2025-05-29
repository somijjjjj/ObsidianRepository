---
tags:
  - Nginx
---

- nginx 서비스 상태
	- systemctl status nginx
```
 nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset:>
     Active: active (running) since Sat 2025-01-18 11:56:53 KST; 1h 53min ago
       Docs: man:nginx(8)
    Process: 1465582 ExecStartPre=/usr/sbin/nginx -t -q -g daemon on; master_pr>
    Process: 1465583 ExecStart=/usr/sbin/nginx -g daemon on; master_process on;>
   Main PID: 1465584 (nginx)
      Tasks: 3 (limit: 1046)
     Memory: 5.6M
        CPU: 45ms
     CGroup: /system.slice/nginx.service
             ├─1465584 "nginx: master process /usr/sbin/nginx -g daemon on; mas>
             ├─1465585 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "">
             └─1465586 "nginx: worker process" "" "" "" "" "" "" "" "" "" "" "">

```
- 프로세스 80포트 사용중인지 확인
	- netstat -tuln | grep :80
```

tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN
tcp6       0      0 :::80                   :::*                    LISTEN
tcp6       0      0 :::8080                 :::*                    LISTEN

```
- 방화벽 443 허용
	- ufw allow 443
```
root@was-instance:/home/ubuntu# sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
8080/tcp                   ALLOW       Anywhere
80                         ALLOW       Anywhere
443                        ALLOW       Anywhere
8080/tcp (v6)              ALLOW       Anywhere (v6)
80 (v6)                    ALLOW       Anywhere (v6)
443 (v6)                   ALLOW       Anywhere (v6)
```

- 라우팅 테이블 확인
```
root@was-instance:/home/ubuntu# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         10.0.0.1        0.0.0.0         UG    0      0        0 ens3
0.0.0.0         10.0.0.1        0.0.0.0         UG    100    0        0 ens3
10.0.0.0        0.0.0.0         255.255.255.0   U     100    0        0 ens3
10.0.0.1        0.0.0.0         255.255.255.255 UH    100    0        0 ens3
169.254.0.0     0.0.0.0         255.255.0.0     U     0      0        0 ens3
169.254.0.0     0.0.0.0         255.255.0.0     U     100    0        0 ens3
169.254.169.254 10.0.0.1        255.255.255.255 UGH   100    0        0 ens3

```

- [[리눅스 명령어 iptables ]] 명령어 [참고](https://stackoverflow.com/questions/62326988/cant-access-oracle-cloud-always-free-compute-http-port)
```
root@was-instance:/home/ubuntu# curl http://150.230.252.242
curl: (7) Failed to connect to 150.230.252.242 port 80 after 0 ms: No route to host
root@was-instance:/home/ubuntu# sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT

```
- 저장 `netfilter-persistent`
```
root@was-instance:/home/ubuntu# netfilter-persistent save
run-parts: executing /usr/share/netfilter-persistent/plugins.d/15-ip4tables save
run-parts: executing /usr/share/netfilter-persistent/plugins.d/25-ip6tables save


root@was-instance:/home/ubuntu# curl http://150.230.252.242
{"timestamp":"2025-01-18T05:09:58.711+00:00","status":404,"error":"Not Found","path":"/"}root@was-instance:/home/ubuntu# curl http://150.230.252.242/index.html


```


- nginx 설정 리로드
```
sudo nginx -t
sudo systemctl reload nginx

```


