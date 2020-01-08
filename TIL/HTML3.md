## HTML

Servelet

> Java 코드 안에 HTML 코드

JSP

> HTML코드 안에 Java 코드가 들어가게 뒤바꿈



Main servelet은 controller 역할을 하고 JSP는 view 역할

-> Model2 Architecture

```
index.html 생성 -> memberInsert.html 생성 -> MainServelt 생성 -> memberInsert_resolt.jsp 생성

이후 더 간편하게 바꾸려면
index.html 파일 내용을
index.jsp, title.jsp, menu.jsp, content.jsp, banner.jsp, copyright.jsp 파일로 나눈뒤
index.html 파일은 templet.jsp로 변경
---
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

	<jsp:include page="title.jsp"></jsp:include>
	<jsp:include page="menu.jsp"></jsp:include>
	<jsp:include page="content.jsp"></jsp:include>
	<jsp:include page="banner.jsp"></jsp:include>
	<jsp:include page="copyright.jsp"></jsp:include>
---

---
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>첫 페이지</title>
	<style type="text/css">
	 @import "css/table.css"
	</style>
</head>
<body>
	<table border="1">
		<tr id="title"><td colspan="3">title</td></tr> //title.jsp
		<tr>
			<td class="c1">
				<a href="memberInsert.html">회원가입</a>
			</td> //menu.jsp
			<td>content</td> //content.jsp
			<td class="c1">banner</td> 
		</tr> //banner.jsp
		<tr id="copyright"><td colspan="3">copyright</td></tr>
	</table>
</body>
</html> //copyright.jsp
---

이후 memberInsert.html도 jsp파일로 변경하면서 내용들 변경
---
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:include page="title.jsp"></jsp:include>
<jsp:include page="menu.jsp"></jsp:include>
			<td>
				<h3>회원가입</h3>
				<form action="main" method="post">
					<input type="hidden" name="sign" value="memberInsert" />
					ID<input name="id"><br>
					PW<input name="pw" type="password"><br>
					NAME<input name="name"><br>
					<input type="submit" value="회원가입">
				</form>
			</td>
	
<jsp:include page="banner.jsp"></jsp:include>
<jsp:include page="copyright.jsp"></jsp:include>
---

memberInsert_resolt.jsp 내용도 추가 변경
---
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<jsp:include page="title.jsp"></jsp:include>
<jsp:include page="menu.jsp"></jsp:include>

			<td>${name}님 가입되셨습니다.</td>


<jsp:include page="banner.jsp"></jsp:include>
<jsp:include page="copyright.jsp"></jsp:include>
---
```



 