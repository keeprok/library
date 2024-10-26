- SSR을 위한 몇가지 데이터 불러오기 전략

## **getStaticPaths 와 getStaticProps**

- 사용자 와 관계없이 정적인페이지( CMS나 블로그, 게시판 등) 를 보여주고자 할 때 사용
- getStaticPaths 와 getStaticProps 는 반드시 함께 사용되어야 한다.
- 제공해야되는 페이지 수에 따라
  - 적을경우 : 페이지 빌드 시점에 미리 준비
  - 많을경우 : fallback을 사용해 요청이 있을때만 빌드하는 최적화를 추가 가능

### getStaticPaths

- 접근 가능한 주소를 정의하는 함수 또한 반환까지
- 정의하지않은 주소로 사용자가 접근시 404 반환

### getStaticProps

- getStaticPaths의 요청이 왓을때 제공할 props를 반환하는함수

### post

- getStaticProps가 반환한 post를 렌더링하는역할

## getServerSideProps

- 서버에서 실행되는 함수 (페이지 진입전 이함수 실행)
  - 브라우저에서만 접근할수있는 객체 사용 x
  - API 호출시 protocol , domain 없이 fetch 사용 x
    - 브라우와 다르게 서버는 자신의 호스트 유추 x , 반드시 완전한 주소를 제공해야 fetch 가능
  - 에러 발생시 500.tsx 에서 처리
- 사용자가 페이지 호출시 매번 실행 , 실행끝난후 HTML 제공
  - 그렇게 때문 반드시 화면에 렌더링해야하는 중요한 데이터만 가져오는것이 좋다
- redirect를 통해 조건 설정후 다른 페이지로 보낼수있다

## **getInitialProps**

- getStaticProps , getServerSideProps \*\*\*\*나오기 이전에 사용하던 유일한 수단
- 굉장히 제한적인 예시 에서만 사용 가능
  - 페이지의 루트함수에 정적 메서드로 추가
  - props 객체반환 x
  - 바로 객체 반환한다.
- 서버와 클라이언트 모두에서 실행 가능하다.
