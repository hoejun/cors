# cors
CORS 정책 

프론트엔드, 백엔드 개발을 하게되면 외부 API 서버에 통신을 하게된다.

그러면 단골로 만나는 에러 중 하나가 CORS 정책을 위반 관련 에러이다.

![image](https://user-images.githubusercontent.com/45154233/139275799-df91559b-138f-4c63-8a56-a021897cb642.png)

![image](https://user-images.githubusercontent.com/45154233/139275775-e316d8f6-f9fc-4937-a07d-728771bc41f3.png)
   




CORS : 교차 출처 리소스 공유(Cross-Origin Resource Sharing) 추가 HTTP 헤더를 사용하여,
한 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다.
   


교차 출처 요청의 예시 :

https://domain-a.com의 프론트엔드 JavaScript 코드가 XMLHttpRequest를 사용하여

https://domain-b.com/data.json을 요청하는 경우.....



보안 상의 이유로 브라우저는 스크립트에서 시작한 교차 출처 HTTP 요청을 제한한다. 

예를 들면, XMLHttpRequestdhk와 Fetch API는 동일 출처 정책을 따른다. 즉 이 API를 사용하는 웹 애플리케이션은 자신의 출처와 동일한 리소스만 불러올 수 있으며,

다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS 헤더를 포함한 응답을 반환해야한다.



요약

-CORS는 서로 다른 출처(Origin) 간에 리소스를 전달하는 방식을 제어하는 체제이며,

-CORS 요청이 가능하려면 서버에서 특정 헤더인 Access-Control-Allow-Origin과 함께 응답할 필요가 있다.

->클라이언트 포트3000번, 서버의 포트가 8000번일때 CORS 에러 메시지가 클라이언트 콘솔에 출력된다.



해결방법

1.상대방이 만든 프록시 서버 이용

->프록시 서버를 사용해서 우회하는 방법 : https://cors-anywhere.herokuapp.com/url(axios, ajax, fetch 등등 url 입력 부분 앞에 추가한다.)



2.직접 프록시 서버 구축하기

-CORS는 브라우저에 관련된 정책이기 때문에, 브라우저를 통하지 않고 서버 간 통신을 할 때는 이 정책이 적용되지 않는다.

react 클라이언트단 코드

![image](https://user-images.githubusercontent.com/45154233/139275700-56c2c24f-4ef8-4b34-9259-18e291ea2a8e.png)



Express를 활용한 서버단 코드

![image](https://user-images.githubusercontent.com/45154233/139275678-46701a6b-6e1d-486a-a485-0722874a2055.png)



3.클라이언트:http-proxy-middleware 사용

배포하고 나서는 동일한 출처에 요청을 하므로 CORS 에러가 발생하지 않지만, 배포하기 전 개발 단계가 문제

로컬 환경에서 React는 3000번 포트, Express는 5000번 포트

로컬 환경일 경우에 한정해서 이 방법으로 해결하자

![image](https://user-images.githubusercontent.com/45154233/139275603-53823b3b-6859-4222-88be-0158aa654ae0.png)


로컬 환경에서 3000포트 api로 시작되는 요청을 라이브러리가 5000포트 api로 프록싱 해주게된다.

->브라우저는 클라이언트와 서버의 출처가 다르지만 같은 것으로 받아들이게 되어 CORS 문제를 일으키지 않는다.



4.서버:Access-Control-Allow-Origin 헤더 세팅하기

서버에서 헤더를 세팅해주는 것이 가장 기본적인 CORS 해결 방법!!!!!!
![image](https://user-images.githubusercontent.com/45154233/139275559-38128798-5830-4901-963a-afa380f9f3af.png)



Access-Control-Allow-Origin:* 이런식으로 작성하지 마라.

*를 사용하면 모든 출처에서 요청을 허용하는 것이기 때문에 지양하는 것이 좋고, 허용하고자 하는 도메인을 꼭 작성해라.   



   
5.서버:CORS미들웨어 사용하기

Express로 구축한 경우 Node.js 미들웨어 중 하나인 CORS를 사용하여 쉽게 해결 가능
![image](https://user-images.githubusercontent.com/45154233/139275482-82d6cf2b-3744-4e5e-bce1-dcb0ca1f4dc0.png)

origin에 허용하고자 하는 도메인 추가, credentials을 true로 설정하면 response헤더에 추가가 된다.
app.use(cors()) 이런식으로 하게 되면 모든 출처에서 오는 요청을 허용하는 것이므로 지양해라..
