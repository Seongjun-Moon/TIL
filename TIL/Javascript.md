### Javascript



![image-20200113131744318](C:\Users\student\AppData\Roaming\Typora\typora-user-images\image-20200113131744318.png)



var 보다는 let으로 통일해서 쓰기



- 확인 button 누르면 점수 입력하는 prompt 띄우고 점수값에 따라 합/불 출력

```javascript
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<script>
	function a() {
    	var score=prompt("점수는?");
    	
    	if (score>=80) {
    		document.write("합격");
    	}
    	else {
    		document.write("불합격");
    	}
	}
	
    </script>
    
   <button onclick="a()"> 확인 </button>
    

</body>
</html>
```

