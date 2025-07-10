---
tistory: https://jn-silveryoung.tistory.com/18
tags:
  - publish/done
---


# 1.'eclipse.ini' 설정 파일 설정

![[Pasted image 20231113124038.png]]
- vm
	- jdk의 경로를 직접 지정. jdk를 여러 개 설치하고 작업한다면 직접 위치를 지정하여 사용할 수 있음
	- vmargs 라인 이전에 설정
- -Dosgi.requiredJavaVersion=1.8
	- 사용할 자바 버전
- -Xverify:none  
	- 초기 실행 시 클래스 유효성 검사, 생략
- -XX:UseParallelOldGC
	- 병렬 GC사용
- -XX:+AggressiveOpts
    - 컴파일러의 소수점 최적화 기능 작동 설정.
- -XX:-UseConcMarkSweepGC  
	- 이클립스 gui 응답 최적화 설정
- XX:PermSize=1024M 
	- JVM 클래스와 메서드를 위한 공간
	- OutOfMemoryError:PermGenspace 발생시 이 설정값을 늘려야 한다.
- XX:MaxPermSize=1024M
	- Max Permanet Generation Size
- XX:MaxNewSize=256M  
	- 새로 생성된 객체들을 위한 공간
- XX:NewSize=256M  
	- 새로 생성된 객체들을 위한 공간
- Xms2048m
	- 이클립스 Heap Memory Size Min
- Xmx4096m
	- 이클립스 Heap Memory Size Max
	- 보통 내장된 메모리의 4분의 1을 최대 Heap 메모리로 설정하여 사용
## eclipse.ini
```
-startup
plugins/org.eclipse.equinox.launcher_1.6.500.v20230717-2134.jar
--launcher.library
C:/Users/admin/.p2/pool/plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.2.700.v20221108-1024
-product
org.eclipse.epp.package.jee.product
-showsplash
C:\Users\admin\.p2\pool\plugins\org.eclipse.epp.package.common_4.29.0.20230907-1200
--launcher.defaultAction
openFile
--launcher.appendVmargs
-vm
C:/Users/admin/.p2/pool/plugins/org.eclipse.justj.openjdk.hotspot.jre.full.win32.x86_64_17.0.8.v20230831-1047/jre/bin
-vmargs
-Dosgi.requiredJavaVersion=1.8
-Dosgi.instance.area.default=@user.home/eclipse-workspace
-XX:+UseG1GC
-XX:+UseStringDeduplication
--add-modules=ALL-SYSTEM
-Dosgi.dataAreaRequiresExplicitInit=true
-Xverify:none
-XX:UseParallelOldGC
-XX:+AggressiveOpts
-XX:-UseConcMarkSweepGC 
-XX:MaxPermSize=1024m
-XX:MaxNewSize=1024m
-Xms2048m
-Xmx4096m
--add-modules=ALL-SYSTEM
-Declipse.p2.max.threads=10
-Doomph.update.url=https://download.eclipse.org/oomph/updates/milestone/latest
-Doomph.redirection.index.redirection=index:/->https://raw.githubusercontent.com/eclipse-oomph/oomph/master/setups/
--add-opens=java.base/java.lang=ALL-UNNAMED
```


# 2.인코딩 설정
1. Preferences > General > Content Types > Java Class File > Default Encoding
2. Windows > Preferences > General > Editors > Spelling > Encoding
3. Windows > Preferences > General > Workspace > Text File Encoding
4. Windows > Preferences > Web > (CSS, HTML, JSP) > Encoding
5. Windows > Preferences > XML > XML Files > Encoding

# 3.메모리 사용 상태 표시
![[Pasted image 20231113130656.png]]

# 4.Spell Checking 해제
![[Pasted image 20231113130745.png]]

# 5.실행/종료 속도 개선
![[Pasted image 20231113130847.png]]
- 불필요한 플러그인 체크 해제

# 6.자동 업데이트 해제
![[Pasted image 20231113130921.png]]

# 7.불필요한 플러그인 삭제
- Windows > Preferences > Install/Update
- Uninstall or Update 클릭 
	![[Pasted image 20231113131008.png]]
- 불필요한 플러그인 삭제

# 8.코드 자동완성 기능 해제
- Windows > Preferences > Java > Editor > Content Assist - Auto Activation - Enable Auto Activation 체크 해제
- Windows > Preferences > JavaScript > Editor > Content Assist - Auto Activation - Enable Auto Activation 체크 해제
- Windows > Preferences > HTML > Editor > Content Assist - Auto Activation - Enable Auto Activation 체크 해제
- Windows > Preferences > XML > Editor > Content Assist - Auto Activation - Enable Auto Activation 체크 해제

# 9.JSP 유효성 체크 해제
![[Pasted image 20231113131450.png]]

