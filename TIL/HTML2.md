## HTML

Web Server : httpd

Web Container : CGI engine

Web Context :  My Application scope

Web Application : Web Site + CGI Servlet



- 회원가입 화면 만들기

Dynamic Web Project 생성 (TestWeb3)

---

index.html 생성

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>첫 페이지</title>
</head>
<body>
	<a href="memberInsert.html">회원가입</a>
</body>
</html>
```

---

memberInsert.html 생성

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 가입</h1>
	<hr><hr>
	<form action="main" method="post">
		<input type="hidden" name="sign" value="memberInsert">
		<fieldset>
			<legend>인적 사항</legend>
			이름 : <input name="name" value="ID 입력 요망">
			주민번호 : <input name="ssn1" >-<input name="ssn2" >
			<br>
			국적 : <select name="country">
					<option value="korea">내국인(대한민국)</option>
					<option value="foreign">외국인</option>
				</select>
			<br>
			취득 자격증 : <select name="license" multiple size="1">
						<option value="L1">정보처리기사1급</option>
						<option value="L2">보안기사1급</option>
					</select>
		</fieldset>
		
		<input type="submit" value="회원가입" />
	</form>
</body>
</html>
```

MainServelet 생성

```
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h1>회원 가입</h1>
	<hr><hr>
	<form action="main" method="post">
		<input type="hidden" name="sign" value="memberInsert">
		<fieldset>
			<legend>인적 사항</legend>
			이름 : <input name="name" value="ID 입력 요망">
			주민번호 : <input name="ssn1" >-<input name="ssn2" >
			<br>
			국적 : <select name="country">
					<option value="korea">내국인(대한민국)</option>
					<option value="foreign">외국인</option>
				</select>
			<br>
			취득 자격증 : <select name="license" multiple size="1">
						<option value="L1">정보처리기사1급</option>
						<option value="L2">보안기사1급</option>
					</select>
		</fieldset>
		
		<input type="submit" value="회원가입" />
	</form>
</body>
</html>
```

tel type은 별도의 pattern 입력 해줘야함

```
<fieldset>
			<legend>기타</legend>
			이메일 : <input type="email" placeholder="xxx@xx.xx.xx" name="mail">
			웹주소 : <input type="url" placeholder="http://xxx.xxx.xxx.xxx.xxxx" name="address" />
			연락처 : <input type="tel" pattern="[0-9]{2,3}-[0-9]{3,4}-[0-9][4]" placeholder="0**-999-9999" />
		</fieldset>
```



```
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>첫 페이지</title>
	<link rel="stylesheet" type="text/css" href="style01.css">
	<style>
		@import"style01.css";
	</style>
</head>
```



## CSS3 스타일 시트

- 기본형식

> 선택자 {스타일속성1: 값; 스타일속성2: 값; }

```
h1, h3 { color: white; backgroung-color: black; }
```

