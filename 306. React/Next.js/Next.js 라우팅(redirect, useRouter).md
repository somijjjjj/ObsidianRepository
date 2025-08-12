


### useRouter

- **Next.js**에서 제공하는 훅(Hook)    
- 현재 페이지 경로 정보, 쿼리 파라미터, 페이지 이동 기능을 제공
    

```tsx
import { useRouter } from 'next/navigation';

const Page = () => {
  const router = useRouter();
  return <button onClick={() => router.push('/home')}>홈으로 이동</button>;
};

```



### redirect`

- 페이지를 **강제로 다른 경로로 이동**시킴    
- 서버 컴포넌트나 특정 클라이언트 코드에서 사용
    


```tsx
import { redirect } from 'next/navigation';

if (!isLoggedIn) {
  redirect('/login'); // 로그인 안 했으면 로그인 페이지로
}

```


