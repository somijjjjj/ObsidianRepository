
`.dockerignore` 파일은 Docker 이미지를 빌드할 때, 빌드 컨텍스트에 포함시키지 않을 파일이나 폴더를 지정하는 역할을 합니다. 

즉, 불필요한 파일이 도커 이미지로 복사되는 것을 방지해 빌드 속도를 높이고 이미지 크기를 줄일 수 있습니다.


- **.dockerignore 파일 위치:** 주로 Dockerfile과 같은 디렉토리에 위치합니다.
    
- **작성 방법:** .gitignore와 유사하게 제외하고 싶은 파일 혹은 폴더명을 한 줄에 하나씩 적으면 됩니다.
    
- 예시:
    
    ```text
    
    node_modules
    *.md
    .git
    .idea
    ```
    

이렇게 설정하면 해당 파일·폴더가 이미지 빌드 시 컨텍스트에 포함되지 않습니다.

- 지정한 파일 : `.idea`, `.git`, `Dockerfile`, `.gitignore`, `.dockerignore` 등
- 특정 확장자 전체 : `*.md`, `*.sh`, `*.yml`
- 불필요한 폴더 :  `node_modules`, `scripts`, 테스트 코드 폴더 등
- 사용자 지정 : 파일이나 경로 자유롭게 지정하여 제외 


---

1. [https://nomad-programmer.tistory.com/310](https://nomad-programmer.tistory.com/310)
2. [https://yoo11052.tistory.com/162](https://yoo11052.tistory.com/162)
3. [https://www.inflearn.com/community/questions/1505059/dockerignore](https://www.inflearn.com/community/questions/1505059/dockerignore)
4. [https://simplecode.kr/101](https://simplecode.kr/101)
5. [https://allofdater.tistory.com/35](https://allofdater.tistory.com/35)
6. [https://seanshkim.tistory.com/113](https://seanshkim.tistory.com/113)
7. [https://ohjinn.tistory.com/95](https://ohjinn.tistory.com/95)
8. [https://www.reddit.com/r/docker/comments/1akw05k/dockerignore_does_not_ignore_files/?tl=ko](https://www.reddit.com/r/docker/comments/1akw05k/dockerignore_does_not_ignore_files/?tl=ko)
9. [https://velog.io/@sunwupark/dockerignore](https://velog.io/@sunwupark/dockerignore)
10. [https://www.reddit.com/r/docker/comments/12iavel/creating_dockerignore_using_gitignore/?tl=ko](https://www.reddit.com/r/docker/comments/12iavel/creating_dockerignore_using_gitignore/?tl=ko)