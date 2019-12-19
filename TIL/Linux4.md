###  Linux4

- 학습 로드맵



- 로그인화면 만들어보기(JAVA) - WinClient

eclipse

JAVA EE

2myweb

> NEW -> HTML File -> in WebContent -> index.html로 생성

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Insert title here</title>
</head>
<body>
	<h1>Duke's shopping mall</h1>
	<hr>
	<br>
	<form action="main">
		ID<input type="text" name="id"><br>
		PW<input type="password" name="pw"><br>
		<input type="submit" value="로그인" >
	</form>
</body>
</html>
```

myweb -> Run as -> 1. Run on server -> Tomcat 8.0 



---



myweb -> New -> Servlet -> Java package(web.controller) -> Class name(MainServlet) -> Next -> URL mappings -> Edit -> /main -> Finish

```
package web.controller;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


public class MainServlet extends HttpServlet {


	

	

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		String id=request.getParameter("id");
		String pw=request.getParameter("pw");
		
		System.out.println(id+":"+pw);
		//DB 연동 Business
		
		response.setContentType("text/plain;charset=utf-8");
		PrintWriter out=response.getWriter();
		out.println("<h1>"+id+":"+pw+"</h1>");
	}
			
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
	}
```

로그인 화면 

---



Mariadb db 연동

- mysql 다운로드

  > https://dev.mysql.com/downloads/file/?id=489462

- eclipse

Data Source Explorer -> Database Connections -> New -> Mysql -> New driver Definition -> driver 5.1 -> Jar List도 5.1로  -> Properties 작성 -> Test Connection 되면 연동



---

- Server에서  Web Browser를 통해 접속해보기

Winclient ip주소:8080/myweb

---

- MainServlet 내용 변경 후 다시 접속해보기

```
package web.controller;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class MainServlet extends HttpServlet {
    
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            request.setCharacterEncoding("utf-8");
            String id=request.getParameter("id");
            String pw=request.getParameter("pw");
            
            System.out.println(id+":"+pw);
            //DB 연동 Biz
        
            Class.forName("com.mysql.jdbc.Driver");
            Connection con=DriverManager.getConnection("jdbc:mysql://192.168.111.100/shopping_db","winuser","4321");
            Statement stmt=con.createStatement();
            ResultSet rs=stmt.executeQuery("select * from customer where id='"+id+"'");
            String name=null;
            if(rs.next()) {
                name=rs.getString("name");
            }
            
            response.setContentType("text/html;charset=utf-8");
            PrintWriter out=response.getWriter();        
            out.println("<h1>");
            
            if(name != null) {
                out.println(name+"님 환영합니다");
            }
            
            out.println("</h1>");
        }catch(Exception e) {
            e.printStackTrace();
        }
    }
    
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
    }

}








```

##### shopping db는 11장 내용 참고

name님 환영합니다` 가 출력되어야함