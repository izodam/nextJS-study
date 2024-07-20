## Library vs Framwork

React는 Library, Next는 framwork

### 라이브러리

- 라이브러리는 코드 내에서 사용 -> 사용의 주체는 사용자
- 구조의 모든 결정을 사용자가 내림
- 도움이 필요할 때만 불러와서 사용

### 프레임워크

- 사용의 주체는 프레임워크
- 결정을 대신해주고 자동화를 해줌
- 우리에게 통제권이 없음 -> 즉, 규칙을 지켜야한다!!!

### project 시작

1. `npm init -y` -> package.json 생성
2. `npm install react@latest next@latest react-dom@latest`
3. package.json 파일에서

```json
"scripts": {
    "dev": "next dev"
  },
```

-> dev 명령어를 실행하면 next JS를 실행 -> next가 page라는 폴더(app 폴더 안에 있어야함)를 찾아서 실행 4. app 폴더 안에 page.tsx를 생성 후

```ts
export default function Tomato() {
  return <h1>Hello NextJS!</h1>;
}
```

5. `npm run dev` 후에 http://localhost:3000 열면 웹페이지가 실행되는 것을 확인할 수 있다!
