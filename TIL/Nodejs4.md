## Nodejs

- client.js

```
$(document).ready(function(){
    $('#hello_div').click(function(){
        let login_form='ID<input id="id"><br>';
        login_form += 'PW<input id="pw" type="password"><br>';
        login_form += '<input id="login_btn" type="button" value="login">';
        $('#login_div').html(login_form);
    });

    $(document).on('click','#login_btn',function(){
        const id=$('#id').val();
        const pw=$('#pw').val();
        const send_param={id,pw};
        
        $.post('login',send_param,function(returnData){
            alert(returnData.message);
            if(returnData.resultCode){
                let logout_form='<div id="logout_btn" class="btn btn-danger" >logout</div>';
                logout_form += '<br><hr>쇼핑하기<br>';                
                logout_form += '<input type="checkbox" name="product" value="apple">사과</input>';
                logout_form += '<input type="checkbox" name="product" value="orange">오렌지</input>';
                logout_form += '<input type="checkbox" name="product" value="banana">바나나</input>';
                logout_form += '<input type="button"  id="basket_btn" value="장바구니 넣기"><br>';
                logout_form += '<hr><input type="button"  id="basket_view_btn" value="장바구니 보기">';
                logout_form += '<br><div id="basket_view_div"></div>';
                $('#login_div').html(logout_form);
            }else{
                $('#id').val("");
                $('#pw').val("");
            }           
        });
    });
    
    $(document).on('click','#basket_btn',function(){
        
        const product=$('input:checkbox[name=product]:checked').val();
        const send_param={product};
        $.post('basket',send_param,function(returnData){
            alert(returnData.message);
        });
        
    });
    
    $(document).on('click','#basket_view_btn',function(){      
       
        const send_param={};
        $.post('basket_view',send_param,function(returnData){
           alert(returnData.message);            
        });
    });
    
    $(document).on('click','#logout_btn',function(){     
        const send_param={};
        $.post('logout',send_param,function(returnData){
           alert(returnData.message);     
           location.reload();
        });
    });
});
    
    
 ----
 체크박스 체크 후 장바구니에 담기게 해보기
    
    
    
```

- app.js

```
const express =require('express');
const path=require('path');
const session=require('express-session');

const app=express();

const user_data={id:"a", pw:"b"};

app.use(express.static(path.join(__dirname,'public')));
app.use(express.json());
app.use(express.urlencoded({extended:false}));

app.use(session({
    resave:false,
    saveUninitialized:true,
    secret:'미녀 강사 전은수',
    cookie: {
        httpOnly:true,
        secure:false
    }
}));

app.post('/login',(req,res)=>{
    console.log("login처리:"+req.headers.cookie);
    console.log(req.session);
    const id=req.body.id;
    const pw=req.body.pw;
    if( (id == user_data.id) && (pw == user_data.pw)){        
        req.session.logined_user_id=id;
        res.json({resultCode:1, message:`${id}님 로그인 되셨습니다.`});
    }else{
        res.json({resultCode:0, message:`다시 로그인하세요`});
    }    
});
app.post('/basket',(req,res)=>{
    console.log("basket처리:"+req.headers.cookie);
    console.log(req.session);
    
    const product=req.body.product;
    
    if( req.session.logined_user_id){//로그인 되어있는 사용자
        if(!req.session.basket){//장바구니가 없을 때
            req.session.basket=[];
        }
        req.session.basket.push(product);
        res.json({resultCode:1, message:`${req.session.logined_user_id}님의 장바구니에 ${product}가 담겼습니다.`});
    }else{
        res.json({resultCode:0, message:`로그인부터 하세요`});
    }    
});

app.post('/basket_view',(req,res)=>{
    console.log("basket_view 처리:"+req.headers.cookie);
    console.log(req.session);    
    
    if( req.session.logined_user_id){//로그인 되어있는 사용자
        let basket;
        if(req.session.basket){//장바구니가 있을 때
            basket=req.session.basket.join(',');
            res.json({resultCode:1, message:basket});
        }else{
            res.json({resultCode:0, message:`장바구니가 비었습니다`});
        }    
        
    }else{
        res.json({resultCode:0, message:`로그인부터 하세요`});
    }    
});

app.post('/logout',(req,res)=>{
    console.log("logout 처리:"+req.headers.cookie);
    console.log(req.session);    
    
    req.session.destroy(()=>{
        console.log("세션이 파기 되었습니다");
        res.json({resultCode:1, message:`로그아웃 되었습니다`});
    });
});


app.listen(3000,()=>{
    console.log("server ready...");
});
```

- index.html

```
<!DOCTYPE html>
<html>
    <head>

        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.16.0/umd/popper.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js"></script>

        <link rel="stylesheet", href="css/client.css">
        <script src="js/client.js"></script>
    </head>
    <body>
        <div id="hello_div" class="btn btn-success my_width">로그인 해볼까요?</div>
        <br>
        <div id="login_div"><img src='img/a.jpg' class="my_width"></div>
    </body>
</html>
```

