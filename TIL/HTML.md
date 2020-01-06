## HTML

- apache tomcat

startup.bat 실행시 `지정된 파일을 찾을 수 없습니다` 라고 출력 될 시

conf -> server.xml 실행 후 해당부분 편집 저장 후 다시 startup.bat 실행

```
    <!-- Define a SSL HTTP/1.1 Connector on port 8443
         This connector uses the JSSE configuration, when using APR, the 
         connector should be using the OpenSSL style configuration
         described in the APR documentation -->
<!-- # 주석 추가
    <Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
               maxThreads="150" scheme="https" secure="true"
               clientAuth="false" sslProtocol="TLS" 
		   keystoreFile="${user.home}/.keystore" keystorePass="test0000"/>
    --> #주석 추가
 
 => HTTPS는 쓰지 않겠다는 말
```



#### - HTML 기본문서 작성 해보기

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=EUC-KR">
<title>Insert title here</title>
</head>
<body>

	<a href="#pos1">video</a><br>
<form>
		<input type="submit"><br>
		<input type="checkbox"><br>
	</form>
	<p> &lt; , &gt; HTML은 웹 문서를<br> 작성하는<br>  표준 &nbsp; 언어입니다</p>
	<pre>
	HTML은 웹 문서를
	작성하는
	표준 언어입니다.
	</pre>
프로야구팀 리스트
<ul>
	<li>두산 베어스</li>
	<li>LG 트윈스</li>
	<li>롯데 자이언츠</li>
	<li>삼성 라이언스</li>
</ul>
<hr>
프로축구팀 리스트
<ul type="circle">
	<li>FC 서울</li>
	<li>인천 유나이티드</li>
	<li>전북 현대</li>
	<li>포항 스틸러스</li>
</ul>
프로야구팀 리스트
<ol>
	<li>두산 베어스</li>
	<li>LG 트윈스</li>
	<li>롯데 자이언츠</li>
	<li>삼성 라이언스</li>
</ol>
<hr>
프로축구팀 리스트
<ol type="A">
	<li>FC 서울</li>
	<li>인천 유나이티드</li>
	<li>전북 현대</li>
	<li>포항 스틸러스</li>
</ol>
<table border="1">
	<tr><th>a</th><th>b</th><th>c</th></tr>
	<tr><td rowspan="2">a</td><td colspan="2">b</td></tr>
	<tr><td>b</td><td>c</td></tr>	
</table>
<img src="img/tomcat.png" width="100" alt="tomcat"/>logo <br>
<img src="" alt="tomcat"/>logo <br>
<audio src="music/music.mp3" controls loop volume=0.6/></audio><br>

<a id="pos1"></a>
<video src"vidoe.mp4" controls autoplay width="300" height="200"></video>

<div>a</div>	<div>a</div>
<span>b</span>	<span>b</span>
</body>
</html>
```

```
<pre></pre>
줄바꿈, 띄어쓰기를 별도의 명령어 없이도 가능
<form></form>
```

