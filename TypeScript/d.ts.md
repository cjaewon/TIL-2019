> 수정 날짜 (2020-03-24) / 작성 날짜 : (2020-03-24)

# d.ts 작성법 정리
d.ts 는 타입스크립트의 타입 추론을 도와주는 파일이다. JS 모듈을 타입스크립트 프로젝트에서 사용될 때
자동완성 및 오류를 잡기 위해 만듬
### ⚙️ d.ts 생성 - 모듈 안에 만드는 법 (오픈소스 또는 개인프로젝트 때)

```ts
// 모듈 안에 @types/index.d.ts
// declare const|let name = 10; 은 전역 변수
declare const abc = 10;

// declare module name; 은 전역 함수
declare module '모듈 이름' { 
  // 변수
  let express: string;
  const koa: number;

  // 인터페이스
  interface Number {
    num: number;
    str: string;
  }
}
```