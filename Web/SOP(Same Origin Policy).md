### SOP(Same Origin Policy)
+ Same Origin ↔ Cross-Origin
+ 한 origin으로부터 로드된 document 또는 script가 다른 origin의 리소스와 상호작용 할 수 있는 방법을 제한하는 중요한 보안 메커니즘
+ document 내에서 외부 리소스들과 상호 작용할 때 외부 리소스의 orgin이 document의 origin과 다른 경우에 제한을 둠
  + XMLHttpRequest(), widnow.open, iframe 등
+ 로컬스토리지나 세션스토리지 등 origin 마다 하나씩 생성되는 웹 데이터베이스에 접근하는 것도 Same-Origin을 갖는 document나 script만 가능

<br/>

### Origin이란?
+ 브라우저가 document를 받을 때 document에는 항상 origin에 대한 정보가 담겨있음
  + js document.location 명령어로 확인 가능

<br/>

### 브라우저의 Origin 판단 기준
+ Scheme : http, https 등 프로토콜
+ Host : `www.naver.com` 등
+ Port : 80, 443 등

<br/>

### Cross-origin script API
+ iframe.content, window.parent, window.open, window.opener
  + Same-Origin : 상호간의 document에 자유롭게 접근 가능
  + Cross-Origin : 상호간의 document에 접근 불가, 매우 제한적인 객체에만 접근 가능
  + Cross-Origin 간 데이터 교환 시에는 window.postMessage 사용

<br/>

### Cross-origin network access
+ Cross-Origin인 경우에도 다른 서버에 자원을 요청(write)하는 것 자체는 가능
  + 이로 인해 CSRF 공격이 가능한 것
  + Preflight 요청일 때는 요청도 불가
+ 또한 Cross Origin인 경우에도 embedding 가능
  + <script src=""></script>
  + CSS : `<link rel="stylesheet" href="">`
  + `<img>, <video>, <audio>`
  + `<object>, <embed>`
  + @font-face
  + <iframe>
+ 하지만 Cross Origin인 경우 서버로부터 응답받은 리소스에 접근(read) 불가능
  + `Access to ~ at ~ from origin ~ has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.`
  + 서버가 허용한 도메인의 경우 Access-Control-Allow-Origin 헤더에 해당 도메인이 명시됨
+ Cross-origin간 통신이 필요한 경우는?
  + JSONP : 모든 origin 대상으로 SOP를 무력화(표준 X, 권장 X)
  + 허용할 origin만을 Access-Control-Allow-Origin 응답 헤더에 
  
<br/>

### 참고자료
+ `https://www.youtube.com/watch?v=6QV_JpabO7g&list=LL&index=1`


