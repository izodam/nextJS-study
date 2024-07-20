## Route

page.tsx는 /일때의 페이지!!! (기본 Home)

그렇다면 /about-us라는 페이지를 만들고 싶으면??

1. about-us라는 폴더 생성
2.

```ts
// about-us/page.tsx
export default function AboutUs() {
  return <h1>About us!</h1>;
}
```

사용자가 url에 접근했을 때 보여주는 모든걸 담는 파일은 page.tsx!!!

원하는 라우터 경로로 폴더를 만들고 그 안에 page.tsx를 작성한다면 페이지 렌더링이 된다!!!!

## not-found

route에 없는 페이지일 경우 404에러가 뜬다

이 404에러 페이지를 구현하고 싶으면

'not-found.tsx' 파일을 만들어서 구현하면 된다!!

```ts
// app/not-found.tsx

export default function NotFound() {
  return <h1>Not found!</h1>;
}
```

## 모든 페이지에서 사용되는 네비게이션 바 구현

```ts
// app/components/navigation.tsx

"use client";

import Link from "next/link";
import { usePathname } from "next/navigation";

export default function Navigation() {
  const path = usePathname();
  console.log(path);
  return (
    <nav>
      <ul>
        <li>
          <Link href={"/"}>Home</Link> {path === "/" ? "🔥" : ""}
        </li>
        <li>
          <Link href={"/about-us"}>About us</Link>{" "}
          {path === "/about-us" ? "🔥" : ""}
        </li>
      </ul>
    </nav>
  );
}
```

## Hydration

서버사이드 렌더링(SSR)을 통해 만들어 진 인터랙티브 하지 않는 HTML을 클라이언트 측 자바스크립트를 사용하여 인터랙티브한 리액트 컴포넌트로 변환하는 과정을 말한다.

(서버 환경에서 이미 렌더링된 HTML에 React를 붙이는 것)

### "use-client"

서버와 클라이언트 컴포넌트 모듈 간의 경계를 선언하는 데 사용

파일에 "use client"을 정의하면 하위 컴포넌트를 포함하여 해당 파일로 가져온 다른 모든 모듈이 클라이언트 번들의 일부로 간주

오직 client에서만 render 된다는 것을 의미하지 않음!!

backend에서 render되고 frontend에서 hydrate됨을 의미함

정의하지 않으면(server components), server에서 먼저 render되고, hydrate되지 않음!
