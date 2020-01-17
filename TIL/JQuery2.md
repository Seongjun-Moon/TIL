## JQuery

- 로그인 창, 로그아웃 후 로그인 창

```html
index.html

<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
  <script src="./js/my.js"></script>
</head>
<body>
<div id="login_div"><button id="login_button">로그인하기</button></div>
</body>
</html>
---
// WebContent - WebINF - lib - json.simple 1.1.1.1.jar 포함되어 있어야함

```

```javascript
my.js

$(document).ready(function(){
	
	let login_form="<form id='login_form'>";
	login_form+="ID<input id='id_val'><br>"
	login_form+="PW<input type='password' id='pw_val'><br>"
	login_form+="<input type='button' id='login_send_button' value='login'>"
	

	
	
	$(document).on("click", "#logout_button", function(){
		$('#login_div').html(login_form);
	})
	
	
	// <div id="login_div"><button id="login_button">로그인하기</button></div>
	////////////////
	$('#login_button').click(function(){
		//alert();
		let login_form="<form id='login_form'>";
		login_form+="ID<input id='id_val'><br>"
		login_form+="PW<input type='password' id='pw_val'><br>"
		login_form+="<input type='button' id='login_send_button' value='login'>"
		$('#login_div').html(login_form);
	});
	///////////////
	$(document).on("click", "#login_send_button", function(){
		const id_val=$('#id_val').val();
		const pw_val=$('#pw_val').val();
		// alert(id_val+":"+pw_val);
		const send_data_temp={
				sign: "login",
				id : id_val,
				pw : pw_val
		};
		const send_data=JSON.stringify(send_data_temp);
		$.post("main", send_data, function(returnData, status){
			if(returnData.resultCode){
				$('#login_div').html("<button id='logout_button'>로그아웃</button>");
			}else{
			alert(returnData.message);
			}
		});
	});
});

```

```java
MainServlet

package controller;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.json.simple.JSONObject;
import org.json.simple.JSONValue;


public class MainServlet extends HttpServlet {
	
	
	protected void process(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("application/json;charset=utf-8");
		PrintWriter out=response.getWriter();
		
		
		BufferedReader br=request.getReader();
		JSONObject obj=(JSONObject)JSONValue.parse(br);
		String sign=(String)obj.get("sign");
		
		if(sign!=null){
			if(sign.equals("login")){
				String id=(String)obj.get("id");
				String pw=(String)obj.get("pw");
				// DB 확인 ... 처리
				obj=new JSONObject();
				boolean flag=true;
				if(flag){
					obj.put("resultCode", true);
					obj.put("message", id+"님 환영합니다.");
				}else{
					obj.put("resultCode", false);
					obj.put("message", "다시 로그인하세요.");
				}
			}
		}else{ //해킹대응
			
		}
		out.print(obj);
	} // end process
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		process(request, response);
	}

	
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		process(request, response);
	}

}

```

