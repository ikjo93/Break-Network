### CSRF(Cross-Site Request Forgery, 다른 사이트간의 요청 위조)
+ 신뢰할 수 있는 사용자를 가장하여 특정 웹 사이트에 (실제 해당 사용자가) 원치 않는 명령(비밀번호 변경 등)을 보내는 공격

### CSRF 예시 1
```
1. 해커가 어떤 사용자에게 특정 사이트 접속을 유도

2. 해당 사이트에는 사용자가 이용하고 있는 서비스 웹 사이트에 사용자 계정 정보 조회 API를 호출하는 script 로직이 담겨있음

3. 사용자가 해당 사이트에 접속하면 즉시 이 API가 호출되는데, 앞서 사용자가 가지고 있던 쿠키도 함께 전송이 됨

4. API를 요청받은 서버에서는 이 쿠키를 통해 해당 사용자를 인증한 후 해당 요청을 처리함

5. 해커 싱글벙글
```

### 예시 1에서의 문제점
+ Cross-origin 간 write와 read가 가능했기 때문 → 해결책 : SOP(Cross-origin 간 read 방지)
+ 브라우저가 Cross-Site에서도 사용자의 세션 쿠키 값을 그대로 붙여서 전송했기 때문 → 해결책 : SameSite 속성값 Lax 또는 Strict 설정

### CSRF 예시 2
```
1. 해커가 어떤 사용자에게 특정 사이트 접속을 유도

2. 해당 사이트에는 사용자가 이용하고 있는 서비스 웹 사이트에 비밀번호 변경 API를 호출하는 script 로직이 담겨있음

3. 사용자가 해당 사이트에 접속하면 즉시 이 API가 호출되는데, 앞서 사용자가 가지고 있던 쿠키도 함께 전송이 됨

4. API를 요청받은 서버에서는 이 쿠키를 통해 해당 사용자를 인증한 후 해당 요청을 처리함

5. 사용자 어리둥절(왜 내 계정의 비밀번호가 바뀌었지?)
```

### 예시 2에서의 문제점
+ Cross-origin 간 write가 가능했기 때문 → 이를 막을 방법은 없는듯(?)
+ 브라우저가 Cross-Site에서도 사용자의 세션 쿠키 값을 그대로 붙여서 전송했기 때문 → 해결책 : SameSite 속성값 Lax 또는 Strict 설정
