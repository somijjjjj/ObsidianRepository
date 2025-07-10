---
tistory: https://jn-silveryoung.tistory.com/47
tags:
  - publish/done
---
1\. MariaDB 설치

```
sudo apt update
sudo apt install mariadb-server -y
```

---

2\. MariaDB 보안 설정

```
sudo mysql_secure_installation
```

프롬프트에 따라 root 비밀번호 설정, 익명 사용자 제거, 원격 root 로그인 비활성화, 테스트 데이터베이스 제거, 권한 테이블 재로드 등을 설정할 수 있습니다.

-   **Enter current password for root**
    -   MySQL의 root 계정에 대한 현재 비밀번호를 묻습니다. 만약 비밀번호가 설정되어 있지 않다면 그냥 Enter 키를 누르면 됩니다.
-   **Set root password? (Y/n)**
    -   root 계정의 비밀번호를 설정할지 여부를 묻습니다. 비밀번호를 설정하고 싶으면 Y를 입력하고 새 비밀번호를 입력합니다. 이미 설정되어 있다면 이 부분을 건너뛸 수도 있습니다.
-   **Remove anonymous users? (Y/n)**
    -   익명 사용자(anonymous users)를 제거할지 묻습니다. 보안을 위해 Y를 입력하여 익명 사용자를 제거하는 것이 좋습니다.
-   **Disallow root login remotely? (Y/n)**
    -   원격으로 root 계정으로 로그인하는 것을 차단할지 묻습니다. 보안을 위해 Y를 입력하는 것이 권장됩니다.
-   **Remove test database and access to it? (Y/n)**
    -   MySQL 설치 시 기본으로 생성되는 test 데이터베이스를 제거할지 묻습니다. 사용하지 않는 경우 Y를 입력하여 제거하는 것이 좋습니다.
-   **Reload privilege tables now? (Y/n)**
    -   권한 테이블을 다시 로드하여 변경 사항을 즉시 반영할지 묻습니다. Y를 입력하여 변경 사항을 적용합니다.
-   **Switch to unix\_socket authentication? (Y/n)**
    -   로컬에서 root 계정에 비밀번호 없이 로그인하고 싶다면 Y를 선택하세요.
    -   root 계정에 비밀번호를 유지하고 싶다면 n을 선택해 기존 방식으로 유지할 수 있습니다.
-   Change the root password?는 MySQL의 root 계정 비밀번호를 변경할지 묻는 질문
    -   **Yes (Y)**:Y를 입력하면 현재 root 계정 비밀번호를 변경할 수 있는 화면이 나타납니다. 새로운 비밀번호를 입력하고 확인하라는 메시지가 표시되면 다시 한 번 비밀번호를 입력하면 됩니다.
    -   **No (n)**:n을 선택하면 root 계정 비밀번호를 변경하지 않고 그대로 유지하게 됩니다.

---

3\. MariaDB 서비스 설정

MariaDB를 부팅 시 자동으로 시작하도록 설정

```
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

---

4\. 방화벽 설정

MariaDB의 기본 포트인 3306 허용

```
sudo ufw allow 3306/tcp
```

---

5\. MariaDB 접속 확인

MariaDB 서버에 접속하여 제대로 설치되었는지 확인

```
sudo mysql -u root -p
```

---

6\. 데이터베이스 및 사용자 생성

필요한 데이터베이스와 사용자 생성

```
CREATE DATABASE insider;
CREATE USER 'insider'@'%' IDENTIFIED BY 'insider!@';
GRANT ALL PRIVILEGES ON insider.* TO 'insider'@'%';
FLUSH PRIVILEGES;

USE insider;
SHOW GRANTS FOR 'insider'@'%';
```