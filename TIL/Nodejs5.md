## Nodejs

- index.html

```
function a(product){
    const send_param={product};
        $.post('basket',send_param,function(returnData){
            alert(returnData.message);
            $('#basket_div').html(returnData.message);
    });
}

$(document).ready(function(){  
    $('#pro_btn').click(function(){
        a("pro");
    });

    $('#basic_btn').click(function(){
        //alert();
        a("basic");
    });

    $(document).on('click','#logout_btn',function(){
        $.get('logout',function(returnData){
            alert(returnData.message);
            $('#login_div').show();
            $('#logout_div').hide();
        });
    });  

    $('#login_btn').click(function(){
        const email=$('#login_email').val();
        //alert(email);
        const send_param={email};
        $.post('login',send_param,function(returnData){
            alert(returnData.message);              
            $('#logout_div').show();                
            $('#login_div').hide();
        });
    });

    $('#contact_btn').click(function (){
        const name=$('#name').val();
        const email=$('#email').val();
        const comments=$('#comments').val();
        //alert(name+":"+email+":"+comments);
        const send_param={name,email,comments};
        $.post('contact',send_param,(returnData)=>{
            alert(returnData.message);
            $('#name').val('');
            $('#email').val('');
            $('#comments').val('');
        });
    });

    
});
```

- client.js

```
function a(product){
    const send_param={product};
        $.post('basket',send_param,function(returnData){
            alert(returnData.message);
            $('#basket_div').html(returnData.message);
    });
}

$(document).ready(function(){  
    $('#pro_btn').click(function(){
        a("pro");
    });

    $('#basic_btn').click(function(){
        //alert();
        a("basic");
    });

    $(document).on('click','#logout_btn',function(){
        $.get('logout',function(returnData){
            alert(returnData.message);
            $('#login_div').show();
            $('#logout_div').hide();
        });
    });  

    $('#login_btn').click(function(){
        const email=$('#login_email').val();
        //alert(email);
        const send_param={email};
        $.post('login',send_param,function(returnData){
            alert(returnData.message);              
            $('#logout_div').show();                
            $('#login_div').hide();
        });
    });

    $('#contact_btn').click(function (){
        const name=$('#name').val();
        const email=$('#email').val();
        const comments=$('#comments').val();
        //alert(name+":"+email+":"+comments);
        const send_param={name,email,comments};
        $.post('contact',send_param,(returnData)=>{
            alert(returnData.message);
            $('#name').val('');
            $('#email').val('');
            $('#comments').val('');
        });
    });

    
});
```

- app.js

```
const express=require('express');
const path=require('path');
const app=express();
const session=require('express-session');
const loginRouter=require('./routes/login');
const logoutRouter=require('./routes/logout');
const {contactRouter, members}=require('./routes/contact');
const basketRouter=require('./routes/basket');

app.use(express.static(path.join(__dirname,'public')));
app.use(express.json());
app.use(express.urlencoded({extended:true}));

app.use(session({
    resave:false,
    saveUninitialized:true,
    secret: '미녀 강사 전은수',
    cookie : {
        httpOnly:true,
        secure:false
    }
}));


app.use('/basket',basketRouter);
app.use('/logout',logoutRouter);
app.use('/login',loginRouter);
app.use('/contact',contactRouter);


app.listen(3000,()=>{
    console.log("server ready...");
});
```

- login.js

```
const express=require('express');
const router=express.Router();
const {contactRouter, members}=require('./contact');

router.post('/',(req,res)=>{
    //console.log(req.body);
    let message;
    for(let i=0;i<members.length;i++){    
        if(members[i].email==req.body.email){
            message="login ok";
            req.session.email=req.body.email;
            const my_basket=[];
            req.session.basket=my_basket;
            break;
        }
    }
    if(!message){
        message="login fail";
    }
    res.json({message});
    
});


module.exports=router;
```

- contact.js

```
const express=require('express');
const contactRouter=express.Router();

const members=[
    { name: 'park', email: 'park@naver.com', comments: 'park' },
    { name: 'jes', email: 'javanism@hanmail.net', comments: 'a' },    
];

contactRouter.post('/', (req,res)=>{    
    members.push(req.body);
    console.log(members);
    res.json({message:"contact ok"});
});

module.exports={contactRouter, members};
```

- logout.js

```
const express=require('express');
const router=express.Router();

router.get('/',(req,res)=>{
    req.session.destroy(()=>{
        res.json({message:"logout ok"});
    });
});

module.exports=router;
```

- basket.js

```
const express=require('express');
const router=express.Router();

router.post('/',(req,res)=>{
    //console.log(req.body);
    if(req.session.email){
        const my_basket=req.session.basket;
        my_basket.push(req.body.product);
    }
    res.json({message:req.session.basket});
});

module.exports=router;
```

