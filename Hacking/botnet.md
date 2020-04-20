> 수정 날짜 (2020-04-20) / 작성 날짜 : (2020-04-20)

# Botnet / 봇넷
**" 컴퓨터 또는 IOT 기기가 악성 봇에 감염되어 네트워크로 연결되어 있는 형태 "**

감염된 기기는 도스 공격, 키로거, 피싱, 개인정보 수집 등 다양한 명령을 받아 기능을 수행한다.

### 🗂 작동 방법
바이러스에 감염된 기기는 주위에 보안이 취약한 기기를 감염시킨다. 
그리고 그 감염 된 기기는 또 네트워크를 탐색하고 등의 방법을 계속 수행한다.

### 🚁 설계

용어 정리
- **봇 마스터 / 컨트롤러** : 공격자는 클라우드 서버 또는 C&C 서버를 통해 봇들한테 명령을 내린다.
- **C&C** : C&C는 Command and Control이라는 뜻으로 공격자의 명령을 감염된 기기들로 전달한다.


봇과 공격자가 통신하기에는 많은 설계가 있었는데 대표적인 것은

#### 스타 네트워크 토폴로지
C&C 한개에 모든 봇을 연결하는 방식
- 장점 : 명령을 한번에 보낼 수 있다.
- 단점 : C&C 서버가 제거될시 봇을 사용할 방법이 없다.

#### 다중 서버 네트워크 토폴로지
C&C 서버 여러개에 봇을 분산해서 연결시키는 방식
- 장점 : C&C 서버 중에 한개가 제거되고 다른 봇은 사용이 가능하다.
- 단점 : 구현이 어렵다.

#### 계층식 네트워크 토폴로지
C&C 서버 한대에 계층식 구조로 봇들이 통신하는 방식이다
```
      C&C
       |
  Bot     Bot
   |       |
Bot Bot Bot Bot
```
- 장점 : 추적하기가 어렵다.
- 단점 : 상위에 있는 봇이 제거되면 다른 봇들을 사용 할수 없다.

### 🤖 미라이 봇넷 (Mirai Botnet)
[소스코드 - Github](https://github.com/jgamblin/Mirai-Source-Code)

**" 가장 유명한 봇넷 "**

Paras Jha 라는 미국 러트거스뉴저지주립대학의 대학생이 개발한 봇넷으로 자신이 좋아하는   
애니메이션에 나오는 캐릭터 이름을 따서 미라이 봇넷으로 지었다고 한다.

작성언어
- 클라이언트 : C언어
- 서버 : Golang 

#### 공격 기록
**2016년 10월 12일**

Dyn (DNS 서비스 업체)에 DDOS 공격을 하여 마비시키는게 성공하여  
Github, 아마존, 트위터 등에 잠시동안 접속 오류가 발생했다.

[더 알아보기](http://wiki.hash.kr/index.php/%EB%AF%B8%EB%9D%BC%EC%9D%B4_%EB%B4%87%EB%84%B7)

[미라이 봇넷 분석](https://hacktagon.github.io/iot/persu/IoT_botnet_analysis)

### 🤔 봇넷 설계에 대한 생각
봇넷을 효율적으로 할려면 **다중 서버 네트워크 토폴로지**도 좋은 것 같지만 추적을  
회피하기 위해서는 **계층식 네트워크 토폴로지**가 더 나은 것 같다.

참고로 미라이 봇넷은 미국 국방부 등 국가 관련 기구의 기기 일부 IP 주소를 가지고 있어서  
해당 기기는 감염시키지 않고 피했다고 한다.