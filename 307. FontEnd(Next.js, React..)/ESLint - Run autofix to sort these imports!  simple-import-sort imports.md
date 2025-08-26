



 - [[ESLint]] 플러그인 중 하나인 `simple-import-sort`가 import 순서가 정렬되지 않았다는 로그
- import 문이 정해진 규칙(예: 알파벳 순, 그룹 순서 등)에 맞게 **정렬되어 있지 않음**    
- `--fix` 옵션으로 **자동 정렬 가능**



## ✅ 해결 방법


터미널에서 프로젝트 루트에서 아래 명령어 입력:

```
npx eslint {파일경로} --fix
```
``

또는 프로젝트 전체:

```
npx eslint . --ext .ts,.tsx --fix
```

### 📌 import 정렬 예시

simple-import-sort는 보통 다음 순서로 정렬합니다:

1. 외부 패키지 (react, next, axios, ...)
2. 절대 경로 모듈
3. 상대 경로 모듈 (./, ../)
4. 스타일 또는 기타

예:
```ts
import React from 'react';
import axios from 'axios';

import { something } from '@/lib/utils';

import MyComponent from './MyComponent';
import './style.css';
```


