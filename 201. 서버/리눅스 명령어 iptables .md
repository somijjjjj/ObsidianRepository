

> 규칙 확인 

`sudo iptables -L --line-numbers`

```
root@was-instance:/home/ubuntu/web# sudo iptables -L --line-numbers
Chain INPUT (policy DROP)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http-alt
2    ACCEPT     all  --  anywhere             anywhere             state RELATED,ESTABLISHED
3    ACCEPT     icmp --  anywhere             anywhere
4    ACCEPT     all  --  anywhere             anywhere
5    ACCEPT     udp  --  anywhere             anywhere             udp spt:ntp
6    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:https
7    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:http
8    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http-alt state NEW,ESTABLISHED
9    ACCEPT     tcp  --  anywhere             anywhere             state NEW tcp dpt:ssh
10   REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
11   ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
12   ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https
13   ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:domain
14   ufw-before-logging-input  all  --  anywhere             anywhere
15   ufw-before-input  all  --  anywhere             anywhere
16   ufw-after-input  all  --  anywhere             anywhere
17   ufw-after-logging-input  all  --  anywhere             anywhere
18   ufw-reject-input  all  --  anywhere             anywhere
19   ufw-track-input  all  --  anywhere             anywhere

Chain FORWARD (policy DROP)
num  target     prot opt source               destination
1    REJECT     all  --  anywhere             anywhere             reject-with icmp-host-prohibited
2    ufw-before-logging-forward  all  --  anywhere             anywhere
3    ufw-before-forward  all  --  anywhere             anywhere
4    ufw-after-forward  all  --  anywhere             anywhere
5    ufw-after-logging-forward  all  --  anywhere             anywhere
6    ufw-reject-forward  all  --  anywhere             anywhere
7    ufw-track-forward  all  --  anywhere             anywhere

Chain OUTPUT (policy ACCEPT)
num  target     prot opt source               destination
1    InstanceServices  all  --  anywhere             link-local/16
2    ufw-before-logging-output  all  --  anywhere             anywhere
3    ufw-before-output  all  --  anywhere             anywhere
4    ufw-after-output  all  --  anywhere             anywhere
5    ufw-after-logging-output  all  --  anywhere             anywhere
6    ufw-reject-output  all  --  anywhere             anywhere
7    ufw-track-output  all  --  anywhere             anywhere

Chain InstanceServices (1 references)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  anywhere             169.254.0.2          owner UID match root tcp dpt:iscsi-target /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
2    ACCEPT     tcp  --  anywhere             169.254.2.0/24       owner UID match root tcp dpt:iscsi-target /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
3    ACCEPT     tcp  --  anywhere             169.254.4.0/24       owner UID match root tcp dpt:iscsi-target /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
4    ACCEPT     tcp  --  anywhere             169.254.5.0/24       owner UID match root tcp dpt:iscsi-target /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
5    ACCEPT     tcp  --  anywhere             169.254.0.2          tcp dpt:http /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
6    ACCEPT     udp  --  anywhere             169.254.169.254      udp dpt:domain /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
7    ACCEPT     tcp  --  anywhere             169.254.169.254      tcp dpt:domain /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
8    ACCEPT     tcp  --  anywhere             169.254.0.3          owner UID match root tcp dpt:http /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
9    ACCEPT     tcp  --  anywhere             169.254.0.4          tcp dpt:http /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
10   ACCEPT     tcp  --  anywhere             169.254.169.254      tcp dpt:http /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
11   ACCEPT     udp  --  anywhere             169.254.169.254      udp dpt:bootps /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
12   ACCEPT     udp  --  anywhere             169.254.169.254      udp dpt:tftp /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
13   ACCEPT     udp  --  anywhere             169.254.169.254      udp dpt:ntp /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */
14   REJECT     tcp  --  anywhere             link-local/16        tcp /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */ reject-with tcp-reset
15   REJECT     udp  --  anywhere             link-local/16        udp /* See the Oracle-Provided Images section in the Oracle Cloud Infrastructure documentation for security impact of modifying or removing this rule */ reject-with icmp-port-unreachable

Chain ufw-after-forward (1 references)
num  target     prot opt source               destination

Chain ufw-after-input (1 references)
num  target     prot opt source               destination
1    ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:netbios-ns
2    ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:netbios-dgm
3    ufw-skip-to-policy-input  tcp  --  anywhere             anywhere             tcp dpt:netbios-ssn
4    ufw-skip-to-policy-input  tcp  --  anywhere             anywhere             tcp dpt:microsoft-ds
5    ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:bootps
6    ufw-skip-to-policy-input  udp  --  anywhere             anywhere             udp dpt:bootpc
7    ufw-skip-to-policy-input  all  --  anywhere             anywhere             ADDRTYPE match dst-type BROADCAST

Chain ufw-after-logging-forward (1 references)
num  target     prot opt source               destination
1    LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-after-logging-input (1 references)
num  target     prot opt source               destination
1    LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-after-logging-output (1 references)
num  target     prot opt source               destination

Chain ufw-after-output (1 references)
num  target     prot opt source               destination

Chain ufw-before-forward (1 references)
num  target     prot opt source               destination
1    ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
2    ACCEPT     icmp --  anywhere             anywhere             icmp destination-unreachable
3    ACCEPT     icmp --  anywhere             anywhere             icmp time-exceeded
4    ACCEPT     icmp --  anywhere             anywhere             icmp parameter-problem
5    ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
6    ufw-user-forward  all  --  anywhere             anywhere

Chain ufw-before-input (1 references)
num  target     prot opt source               destination
1    ACCEPT     all  --  anywhere             anywhere
2    ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
3    ufw-logging-deny  all  --  anywhere             anywhere             ctstate INVALID
4    DROP       all  --  anywhere             anywhere             ctstate INVALID
5    ACCEPT     icmp --  anywhere             anywhere             icmp destination-unreachable
6    ACCEPT     icmp --  anywhere             anywhere             icmp time-exceeded
7    ACCEPT     icmp --  anywhere             anywhere             icmp parameter-problem
8    ACCEPT     icmp --  anywhere             anywhere             icmp echo-request
9    ACCEPT     udp  --  anywhere             anywhere             udp spt:bootps dpt:bootpc
10   ufw-not-local  all  --  anywhere             anywhere
11   ACCEPT     udp  --  anywhere             mdns.mcast.net       udp dpt:mdns
12   ACCEPT     udp  --  anywhere             239.255.255.250      udp dpt:1900
13   ufw-user-input  all  --  anywhere             anywhere

Chain ufw-before-logging-forward (1 references)
num  target     prot opt source               destination

Chain ufw-before-logging-input (1 references)
num  target     prot opt source               destination

Chain ufw-before-logging-output (1 references)
num  target     prot opt source               destination

Chain ufw-before-output (1 references)
num  target     prot opt source               destination
1    ACCEPT     all  --  anywhere             anywhere
2    ACCEPT     all  --  anywhere             anywhere             ctstate RELATED,ESTABLISHED
3    ufw-user-output  all  --  anywhere             anywhere

Chain ufw-logging-allow (0 references)
num  target     prot opt source               destination
1    LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW ALLOW] "

Chain ufw-logging-deny (2 references)
num  target     prot opt source               destination
1    RETURN     all  --  anywhere             anywhere             ctstate INVALID limit: avg 3/min burst 10
2    LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 10 LOG level warning prefix "[UFW BLOCK] "

Chain ufw-not-local (1 references)
num  target     prot opt source               destination
1    RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type LOCAL
2    RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type MULTICAST
3    RETURN     all  --  anywhere             anywhere             ADDRTYPE match dst-type BROADCAST
4    ufw-logging-deny  all  --  anywhere             anywhere             limit: avg 3/min burst 10
5    DROP       all  --  anywhere             anywhere

Chain ufw-reject-forward (1 references)
num  target     prot opt source               destination

Chain ufw-reject-input (1 references)
num  target     prot opt source               destination

Chain ufw-reject-output (1 references)
num  target     prot opt source               destination

Chain ufw-skip-to-policy-forward (0 references)
num  target     prot opt source               destination
1    DROP       all  --  anywhere             anywhere

Chain ufw-skip-to-policy-input (7 references)
num  target     prot opt source               destination
1    DROP       all  --  anywhere             anywhere

Chain ufw-skip-to-policy-output (0 references)
num  target     prot opt source               destination
1    ACCEPT     all  --  anywhere             anywhere

Chain ufw-track-forward (1 references)
num  target     prot opt source               destination

Chain ufw-track-input (1 references)
num  target     prot opt source               destination

Chain ufw-track-output (1 references)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  anywhere             anywhere             ctstate NEW
2    ACCEPT     udp  --  anywhere             anywhere             ctstate NEW

Chain ufw-user-forward (1 references)
num  target     prot opt source               destination

Chain ufw-user-input (1 references)
num  target     prot opt source               destination
1    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http-alt
2    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:http
3    ACCEPT     udp  --  anywhere             anywhere             udp dpt:80
4    ACCEPT     tcp  --  anywhere             anywhere             tcp dpt:https
5    ACCEPT     udp  --  anywhere             anywhere             udp dpt:https

Chain ufw-user-limit (0 references)
num  target     prot opt source               destination
1    LOG        all  --  anywhere             anywhere             limit: avg 3/min burst 5 LOG level warning prefix "[UFW LIMIT BLOCK] "
2    REJECT     all  --  anywhere             anywhere             reject-with icmp-port-unreachable

Chain ufw-user-limit-accept (0 references)
num  target     prot opt source               destination
1    ACCEPT     all  --  anywhere             anywhere

Chain ufw-user-logging-forward (0 references)
num  target     prot opt source               destination

Chain ufw-user-logging-input (0 references)
num  target     prot opt source               destination

Chain ufw-user-logging-output (0 references)
num  target     prot opt source               destination

Chain ufw-user-output (1 references)
num  target     prot opt source               destination

```

> 규칙 추가

```
root@was-instance:/home/ubuntu# sudo iptables -I INPUT 6 -m state --state NEW -p tcp --dport 80 -j ACCEPT

```
- **`sudo`**
    - 관리자 권한으로 명령을 실행합니다. `iptables`는 시스템 네트워크 설정을 변경하므로 관리자 권한이 필요합니다.
- **`iptables`**
    - Linux 방화벽을 관리하는 명령어입니다. 방화벽 규칙을 추가, 삭제, 수정할 때 사용됩니다.
- **`-I INPUT 6`**
    - `-I`는 **새 규칙을 삽입(Insert)**하는 옵션입니다.
    - `INPUT`은 **수신되는 네트워크 트래픽**에 적용되는 체인입니다.
    - `6`은 이 규칙을 **INPUT 체인의 6번째 위치**에 삽입하라는 뜻입니다. (기존 규칙에 영향을 줄 수 있으므로 주의가 필요합니다.)
- **`-m state --state NEW`**
    - `-m state`: **state 모듈**을 활성화합니다. 네트워크 연결 상태를 기반으로 필터링할 수 있습니다.
    - `--state NEW`: 새로 시작된 연결에만 이 규칙을 적용합니다.
- **`-p tcp`**
    - 프로토콜을 지정합니다. 여기서는 **TCP** 트래픽에만 규칙이 적용됩니다.
- **`--dport 80`**
    - 특정 **목적지 포트**를 지정합니다. 여기서는 HTTP 요청에 사용되는 **포트 80**을 의미합니다.
- **`-j ACCEPT`**
    - 트래픽을 허용(Accept)합니다. 규칙에 해당하는 트래픽은 차단되지 않고 서버로 전달됩니다.