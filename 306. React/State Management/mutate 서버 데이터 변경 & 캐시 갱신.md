

mutate는 React Query, SWR, TanStack Query 같은 데이터 패칭 라이브러리에서 많이 나오는 함수.

보통 데이터 수정·추가·삭제 요청을 서버로 보내고, 캐시를 업데이트하는 역할


```tsx
// 예: React Query 예시
import { useMutation } from '@tanstack/react-query';
import { addPost } from './api';

const { mutate } = useMutation({
  mutationFn: addPost,
  onSuccess: () => {
    console.log("게시글 추가 성공");
  }
});

// 호출
mutate({ title: "새 글" });
```


- mutate를 쓰면 서버에 데이터 변경 요청을 보낸 뒤, 
  성공 시 자동으로 UI를 업데이트하거나, 캐시를 무효화하여 최신 데이터를 다시 불러오게 합니다.