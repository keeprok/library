- Next.js 실행에 필요한 설정을 추가할 수 있는 파일

```tsx
/** 
@type {import('next').NextConfig} 
*/
const nextConfig = {
  // 설정
};

export default nextConfig;
```

### **basePath**

- 애플리케이션 실행 시 호스트 아래 / 에 애플리케이션이 제공될것이다

### **swcMinify**

- swc 를 통해 코드를 압축할지 설정 가능
- 13버전부터는 기본값이 true 로 변경 , 설정없을경우 swc 를 활용해 압축해야한다

## **poweredByHeader**

- 보안 관련 솔루션에서는 powered-by 해더를 취약점으로 분류 , 그래서 false로 설정하는것이 좋다

## **redirects**

- 특정 주소로 다른주소로 보내고싶을때 사용됨
- 정규식도 사용가능하므로 다양한 방식으로 응용가능

## **reactStrictMode**

- 리액트에서 제공하는 엄격모드 설정 여부 선택
- 기본값은 false 지만, true 로 설정하는 편이 좋다.
  - 리액트 업데이트를 미리 대비하기위해

## **assetPrefix**

- next 빌드된부분을 동일 호스트가아닌 다른 CDN으로 업로드하고할때 사용
  - 옵션에 CDN 주소 명시 후사용
  ```tsx
  const isProuduction = process.env.NODE.ENV === 'production';

  module.exprots = {
    assetPrefix: isProducton ? ' CDN주소 ' : undefined,
  };
  ```
- 정적인 리소스를 별도 CDN 에 업로드 시 이기능 활용
