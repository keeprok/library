> 타입스크립트를 기반으로 Next.js 를 설치 하는법
> `npx create-next-app@latest --ts`

## package.json

- 프로젝트 구동에 필요한 모든 명령어 및 의존성 포함 ( 프로젝트의 대략적인 모습 확인 가능)
- 몇가지 주요 의존성
  - next : Next.js 의 기반이 되는 패키지
  - eslint-config-next : 구글 과 협업해서 만들어진 핵심 웹 지표(core web

## next.config.js

- Next.js 프로젝트의 환경 설정을 담당 , 자유 자재로 다루려면 반드시필요

```tsx
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  swcMinify: true,
};

export default nextConfig;
```

> @type
>
> 자바스크립트 파일에 타입스크립트의 타입 도움을 받기위한 주석이다. **특별한 이유가 없다면 사용한다.**

추가된 몇가지 옵션

> reactStrictMode : react 의 엄격모드 관련 옵션 , react 내부에러를 개발자에게 알리기 위한 도구
> , 특별한 이유가 없을 경우 true

> swcMinify : Vercel에서 만든 오픈소스 인 SWC를 기반으로 코드 최소화 작업 여부를 설정
>
> > SWC의 경우 바벨에 비해 더빠른 속도를 보여주어 특별한 이유없을 경우 true

## pages/\_app.tsx

- pages 폴더의 경우 src 하단 , src 안에 또는 프로젝트 루트에 있어도 동일하게 작동
- \_app.tsx, 그리고 export default 로 내보낸 함수는 애플리케이션 전체 페이지의 시작점
- 시작점인 특징으로 공통으로 설정해야하는부분을 이부분에서 실행
  - 에러 바운더리 사용해 애플리케이션 전역에서 발생하는 에러 처리
  - react.css 같은 전역 CSS 선언
  - 모든 페이지에 공통으로사용 또는 제공해야하는 데이터 제공

## pages/\_document.tsx

- create-next-app 으로 생성했을경우 해당페이지 에 존재하지않음
  - \_document.tsx 없어도 실행에 지장이없음
  ### \_app.tsx 와 \_document.tsx의 차이
  **\_app.tsx**
  > 1. 애플리케이션 페이지 전체 초기화
  > 2. 렌더링이나 라우팅에따라 서버 or 클라이언트에서 실행가능
  **\_document.tsx**
  > 1. 애플리케이션 HTML 을 초기화
  > 2. 무조건 서버에서 실행 ( 이벤트 핸들러 추가 x)
  > 3. <html ><body> 에 Dom 속성 추가
  > 4. next 의 <head>와 next/document<head>중 후자는 \_document에서만 사용가능
  >
  > > next/documet<Head/> 내부에서는 <title/> 사용 x

## pages/\_error.tsx

- creat-next-app 이 기본으로 생성하는 파일이 아님(없어도 실행가능)
- 500 에러 처리 위한 목적 페이지
  - 전역에서 발생하는에러 처리
- 단 개발모드에서는 접근 x
  - Next.js 에서 제공하는 개발자 에러 팝업 표시
  - 평소에는 프로덕션 빌드로 확인

## pages/404.tsx

- 404페이지 정의 파일
- 원하는 404 페이지 를 수정 가능 ( 생성 x않을시 기본 404페이지 제공 )

## pages/500.tsx

- 서버에서 발생하는 에러를 핸들링하는 페이지
- \_error.tsx 보다 우선하여 실행
- 500.tsx, \_error.tsx 두 파일이 모두 없을경우 Next.js 가 제공하는 페이지가 출력

## pages/index.tsx

- 개발자가 명칭 지정 가능

**/page/index**

> 웹사이트의 루트
> localhost:3000

**/page/helllo.tsx**

> pages 생략
> localhost:3000/hello

/page/hello/world.tsx

> localhost:3000/hello/world
> 디렉터리의 깊이만큼 주소설정 가능
>
> > /hello = /hello/index (같은주소)

/page/hello/[greeting].tsx

> localhost:3000/page/hello/[]
> [] : 어떠한것도 가능
>
> > 단 /page/hello/world 가있을경우 localhost:3000/page/hello/world 가 우선

/page/hello/[…props].tsx

> 형식이 전개 연산자와 동일
> localhost:3000/page/hello 를제외한 모든 하위 주소가 가능
> ex : localhost:3000/hello/hi , localhost:3000/hello/hi/my , localhost:3000/hello/hi/my/name
> props 의 경우 마지막같은 부분 [”my”,”name”]이 할당

**서버 라우팅과 클라이언트 라우팅의 차이**

- Next.js는 SSR도 수행하지만 SPA 도 수행
- <a> : SSR의 특성을 가짐
- <Link> : SPA의 특성을 가짐
- 내부페이지 이동시
  - <a>대신 <Link> 사용
  - window.location.push 대신 router.push 사용

### 페이지의 getServerSideProps의 유무

- <a/> , <Link/> 상관없이 서버에 로그가 남지 않음
- **있는 빌드**
  - 서버 사이드 런타임이 발생할수있다
- **없는 빌드**
  - 빌드의 크기도 약간준다
  - SSR이 필요없는 정적 페이지로 분류된다
    - 클라이언트에서 작업처리되며 빌드시점에서 미리 트리쉐이킹을 한다

## /pages/api/hello.ts

- 서버의 API 를 정의 하는 폴더
- 기본적인 디렉터리 에따른 라우팅 구조는 페이지와 동일
  - /pages/api가 /api 가 접두가붙는것만다름
  - /pages/api/hello > /api/hello로 호출 가능
- 해당 디렉토리의 하위 파일은 모두 서버에서만 실행
