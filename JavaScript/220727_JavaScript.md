# 항해99 8기 3주차 <Node.js 입문> TIL_20220727 #
## 주특기 입문 주차 팀 과제 ##
**우리가 브라우저에서 구매한 도메인 주소를 입력하면 만들어 놓은 aws EC2 서버까지 전달이 되어서 해당 서버에게 요청을 보낼 수 있습니다. 
이 과정이 상세하게 어떻게 진행되는지 그림으로 한번 그려볼까요?**

![dns 동작과정](https://user-images.githubusercontent.com/109029407/181509266-05044b28-7e68-445d-a25b-3883212846b0.png)

- 웹사이트에 접속할 때 외우기 어려운 IP 주소 대신 도메인 이름을 사용한다. 도메인 이름을 사용했을 때 입력한 도메인을 실제 네트워크상에서 사용하는 IP주소로 바꾸고 해당 IP주소로 접속하는 과정 시스템을 **DNS(도메인 네임 시스템)** 이라고 한다.

- 상위 기관에서 인증된 기관에게 도메인을 생성하거나 IP주소로 변경할 수 있는 ‘권한’을 부여한다. DNS는 상위기관, 하위기관과 같은 **‘계층 구조’** 를 가지는 **분산 데이터베이스 구조**를 가진다.

- **<DNS 동작 방식 예시>**
1. 웹 브라우저는 해결사 서버에게 요청(www.naver.com의 IP 주소를 알려주세요!)
2. 해결사 서버는 최상위 기관에서 관리하는 네임 서버에 요청(.com이라는 도메인 있나요?)
3. 최상위 기관 네임 서버는 응답(.com 네임 서버로 가보세요~)
4. 해결사 서버는 .com 네임 서버에게 요청(naver.com 있나요?)
5. .com 네임 서버는 응답( 가비아로 가세요~)
6. 해결사 서버는 가비아 네임서버에게 요청하고 가비아 네임서버 응답(12.345.678.900으로 가세요!)
7. 해결사 서버가 웹브라우저에 전달
---
**new 연산자와  생성자 함수란 무엇인가?**

new 연산자는 객체를 생성하고 생성자 함수를 호출해 객체의 속성(프로퍼티를) 초기화한다.

new 연산자는 생성자 함수 앞에 붙여서 쓰이는데 생성자 함수는 첫 글자가 대문자로 시작한다. 

( ex) Date )   new 연산자는 생성자 함수를  호출해서 빈 객체를 this안에 할당한다. 속성을 사용하기 위해서는 this 안에 속성을 설정하고, 값을 할당하면 된다.

만약 여기서 생성자 함수에 인자를 받아 속성 값으로 할당할 수도 있고, 어떤 프로그래밍(코딩)을 해두면 그에 맞게 인자 값을 가공해서 받을 수 있다.
