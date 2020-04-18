> 수정 날짜 (2020-04-18) / 작성 날짜 : (2020-04-18)

# 디도스 공격

### 💥 DOS 공격이란?
" 공격자가 수 많은 요청의 서버로 보내 서버가 버티지 못하도록 하여 서비스 유지에 타격을 주는 공격법 "  

대충 고 언어로 짜보면 이런 식이다.
```go
package main

import "http/Get"
import "runtime"

func main() {
  runtime.GOMAXPROCS(runtime.NumCPU())

  for i := 0; i < 10000; i++ {
    go func() {
      http.Get("https://~~")
    }()
  }
}
```

하지만 요즘 대부분 서버는 아이피 차단 및 여러가지 기술을 사용해서 방어 한다.

### 🔥 DDOS 공격
DDOS 공격은 DOS 공격의 업그레이드판이다.  
도스 공격은 한 사용자가 공격했다면 디도스 공격은 많은 컴퓨터 (좀비 pc ), IOT 기기 등을 이용하여   
서버로  요청하여 서비스를 마비 시킨다.

### 💣 효과적인 DDOS 공격?
DDOS 공격을 더 효과적으로 할려면 로그인 / 회원가입 처럼 DB를 사용하는 (캐싱이 안되는)  
곧으로 DDOS 공격을 하면 더 효율적이다.

### 📜 XSS 를 사용한 DDOS 공격
대표적으로 트래픽이 많은 사이트 (구글, 네이버) 같은 곳에 XSS 해킹을 하여  
다른 사이트에 자바스크립트를 사용하여 요청을 보내는 것이다.

```js
const xmlHttp = new XMLHttpRequest();
xmlHttp.open('GET', 'https://~~', false); // false for synchronous request
xmlHttp.send(null);
```

### 🛡 DOS 공격 대응법
도스 공격은 간단하다. 한 아이피가 비정상적으로 요청을 많이 할때 도스 공격을 의심해보고 차단하면 된다.