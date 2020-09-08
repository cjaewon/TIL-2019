> 작성 중 / 작성 날짜 : (2020-03-24)

# CORS 설명 및 관련 오류 해결법
`Cross-Origin Resource Sharin`의 줄임말로 보안의 이유로 다른 주소 간 요청하는 매커니즘이다.

### ⚠️ Axios withCertificate CORS 오류
만약 Axios 에서 withCertificate 을 true 로 한다면 cors 에서 주소를 `*`  
가 아닌 localhost:8000 같이 정확하게 적어나야 한다.

백엔드 또한 Credentials 설정을 해주어야 한다.
