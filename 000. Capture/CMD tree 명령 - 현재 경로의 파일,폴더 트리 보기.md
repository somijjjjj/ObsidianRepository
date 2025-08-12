

기본(폴더만):

```cmd
tree
```

파일까지 포함:

```cmd
tree /F
```

문자 깨짐 방지(ASCII) + 파일 포함(추천):

```cmd
tree /F /A
```

결과를 파일로 저장:

```cmd
tree /F /A > tree.txt
```

페이지 단위로 보기:

```cmd
tree /F /A | more
```

특정 하위 폴더만:

```cmd
tree function /F /A
```
