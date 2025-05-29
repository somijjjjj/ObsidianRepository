---
tistory: https://jn-silveryoung.tistory.com/45
sticker: lucide//coffee
---

오라클 클라우드 MFA는 계정 보안을 강화하기 위해 사용되는 인증 방식이다.
일반적으로 처음 가입하면 Oracle Authenticator 어플을 설치해서 인증을 등록하게 된다.

문제는 Oracle Authenticator 어플은 휴대폰 분실 또는 휴대폰 초기화가 되면 인증정보가 날라가게 되어 다른 인증방법을 추가해놓은게 아니라면 오라클 클라우드 로그인 시 인증할 수 있는 방법이 없어지게 된다.

MAF 분실 시 대처 방법을 검색하게 되면 2025년 1월 현재와 다른 부분이 많다. 상단에 나오는 방법들을 따라 해보고, SR을 생성하여 문의하는 법도 찾아봤지만 나에게는 적용이 안되는 방법들 이였다.
결국 찾고 찾아 타 서비스 지원팀에 문의하여 MFA 분실 시 문의하는 곳을 전달받게 되었다.


#### MFA 재설정 방법

1. https://www.oracle.com/cloud/ OCI에 접속한다.

![[Pasted image 20250107215324.png]]
2. 우측하단에 Chat now 을 클릭한다.

![[Pasted image 20250107215419.png]]
3. Get Support 클릭

![[Pasted image 20250107215445.png]]
4. Cloud Infrastructure (OCI) including Free Trial 클릭

![[Pasted image 20250107215528.png]]
5. 마지막 줄에 Cloud Support Chat 클릭

![[Pasted image 20250107215608.png]]
6. Korea, the Republic of 선택
7. 본인 이메일 주소 입력
8. I agree~ 체크박스 클릭 
9. Start chat 클릭
10. 채팅이 시작되면 Ohter issues 선택

![[Pasted image 20250107122123.png]]

11. chat leave a live agent 전송 후 Yes 선택
12. 상담사 연결 될 때까지 대기 


---

상담은 영어로 진행되므로 번역기 돌려서 대화함

> Welcome to Oracle Cloud Chat. My name is Gadhimuthaka, please hold for a moment while I review your request.
> 
> Hello

처음에 이렇게 대화가 시작된다. 나는 대기하는 동안 문의할 내용과 필요한 항목들을 준비했다.
필요한 항목은 4개다. 이메일 주소, 테넌시 이름, 휴대폰번호, 등록된 카드번호 뒷 4자리, 카드 유효기간

> I lost my Oracle cloud MAF. 
>  My phone reset. So I can't log in.
>  This is my information.
>  email address : somijjjjj@gmail.com 
>  tenancy name : 
>  phone number : 
>  Card number last 4 digits: 
>  Expiration date: 


이렇게 보내고 난 뒤 5분정도 기다리면
내가 보낸 정보와 나의 정보가 일치하는지 확인 후 MFA를 재설정해주었다.

tenancy name은 클라우드 로그인을 하게되면 URL 정보에 https://idcs-yourIDCS_Stripe_here.identity.oraclecloud.com/ 이렇게 나타날 것이다. yourIDCS_Stripe_here 이부분을 복사해서 입력하면 된다.


---


#### 구글 Authenticator 등록


![[Pasted image 20250107195426.png]]

MFA가 재설정되면 Oracle Authenticator에서 QR코드 추가 후 통지 허용하여 로그인을 할 수 있다.

이번에 Oracle Authenticator MFA 분실로 2일동안 시달렸기 때문에 이건 버리고 Google로 변경했다.


![[Pasted image 20250107195600.png]]
클라우드 우측 상단에 프로파일 접속

![[Pasted image 20250107195629.png]]
보안 클릭

![[Pasted image 20250107195822.png]]
모바일 앱 추가

![[Pasted image 20250107195704.png]]
오프라인 모드 또는 다른 인증자 사용 클릭 후
Google Authenticator 에서 QR코드로 추가
우회코드 입력 후 추가


![[Pasted image 20250107195810.png]]

기존에 있었던 Oracle 인증은 제거했다.




