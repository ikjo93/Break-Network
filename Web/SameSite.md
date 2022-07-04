### SameSite 판단 기준
+ Public suffix(com, co.kr, org, net, github.io 등) 기준 한 단계 하위 도메인까지 확인해서 이게 같으면 SameSite라고 판단
  + example. `https://www.naver.com:443/path/index.html`
  + 판단 내용 : naver.com

### SameSite 속성
+ 쿠키에 부여하는 속성 값으로 None, Lax, Strict가 있다.
+ None
  + Same Site건 Cross Site건 모든 문맥에서 쿠키를 전송할 수 있음
  + 만일 SameSite 속성 값이 None인 경우 쿠키에 Secure 속성을 설정해줄 필요가 있다.
+ Strict
  + 무조건 SameSite인 경우에만 쿠키를 전송할 수 없음
  + HTTP 목적지의 사이트와 현재 주소 표시창에 있는 사이트가 동일한 경우에만 쿠키를 전송함
+ Lax
  + default 속성(원래는 None이 default 였는데 최근 바뀜)
  + 기본적으로는 Strict 속성과 같음(Cross Site 간 쿠키 전송 X)
    + Cross Site 간 subrequest(img, iframe 등 src 요청) 시 쿠키 전송 X
  + **단, top-level-navagation일 때 쿠키 전송**
    + a 태그 href 속성이나 documnet.location을 통해 특정 사이트에 접속하는 경우 쿠키 전송

<br/>

### SameSite Lax 에시
```
현재 document.location.origin = "https://ikjo.com"
<img src="https://hello.com/1.png"/> → 쿠키 전송 X

<a href="https://hello.com">Link</a> → 쿠키 전송 O

<iframe id="if" src="/hello"></iframe> → 쿠키 전송 X (top level이 다르닌까, 최상단 document에서 네비게이션 할 때만 쿠키 전송)

<script>
document.getElementById("if").contentDocument.location.href = "https://hello.com"
document.location.href="https://hello.com" -> 쿠키 전송 O
</script>
```

<br/>

### SameSite의 이점
+ Privacy and Transparency
  + 사용자에게 쿠키값이 어떻게 사용되어지는지 말할 수 있음
+ Scurity benefit
  + CSRF 방지(Cross Origin 정보 누출 방지)
  + SameSite 기본 설정값 Lax로 변경 → Cross Site간 통신시에는 반드시 HTTPS만 허용

<br/>

### 크롬 브라우저의 SameSite 정책(2020. 2 ~ )
+ 쿠키의 SameSite 속성을 Lax를 기본 값으로 설정
+ SameStie 속성이 None일 경우 반드시 Secure 설정 필요
+ 마이크로소프트의 사이드 이펙트 우려로 FORM 태그를 이용한 POST 요청의 경우는 예외 처리(Cross Site 간 쿠키 전송 가능)
  + 이로 인해 CSRF 완전 방지 어려움
+ 당초 일괄 적용하려고 했으나 COVID-19로 인해 잠깐 롤백한 이후 당해연도 여름에 재배포

<br/>

### 오늘날 SameSite 사용법
+ 우선 해당 쿠키가 Same Site에서만 필요한건지 Cross Site에서도 필요한건지 판단
+ Same Site(first-party context)인 경우
  + SameSite의 속성을 Lax 또는 Strict 명확히 설정
  + Create/Update를 위한 API를 GET 요청으로 하지 않을 것
  + 왜냐하면, `location.href` 또는 `a 태그 href`으로 어떤 요청 전송 시 쿠키가 전송될 수 있음 → CSRF 우려
+ Cross Stie(third-party context)인 경우
  + SameStie 속성을 None으로 설정한 후 쿠키 Secure 설정
  + CSRF 방지 위한 개별적인 로직 필요

### 참고자료
+ `https://www.youtube.com/watch?v=Q3YuKipzPbs`
+ `https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie/SameSite`
