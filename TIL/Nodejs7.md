## Nodejs

- 게시판, 장바구니 추가

```
Client.js
===========

// 게시판 부분
$('#board_read_text').click(function(){
        window.open("/board/read_form", "_blank", "toolbar=yes,scrollbars=yes,resizable=yes,top=100,left=100,width=700,height=600");
    });

    $('#board_write_btn').click(function(){
        const board_title=$('#board_title').val();
        const board_content=$('#board_content').val();
        //alert(board_title+":"+board_content);
        const send_param={board_title,board_content};
        $.post('/board/write',send_param,(returnData)=>{
            alert(returnData.message);
            location.href="/board/read_form";
        });
    });

    $('#board_write_text').click(function(){
        location.href="/board/write_form";
    });
 
 
function board_read_content(bo_no){
    const send_param={bo_no};
    $.post('/board/read_content',send_param,function(returnData){
        let one_content_html='<table border="1" width="650" class="table" >';
        one_content_html += '<tr><td>글 번호</td><td>'+bo_no+'</td></tr>';
        one_content_html += '<tr><td>작성자</td><td>'+returnData.name+'</td></tr>';
        one_content_html += '<tr><td>글 제목</td><td>'+returnData.title+'</td></tr>';        
        one_content_html += '<tr><td>글 내용</td><td>'+returnData.content+'</td></tr>';
        one_content_html += '</table>';
        $('#one_content').html(one_content_html);
    });
}

// 장바구니 부분
 $('#product_btn').click(function() {
        let product_div= "<h2> 발렌시아가 스피드러너 <h2><br>";
        product_div+= '<div><img src=./image/speedrunner.jpg><div>';
        product_div+= "품명 <input id='product', value='스피드러너'><br>";
        product_div+= "Size <input id='size' ><br> ";
        product_div+= "수량 <input id='quantity' ><br>";
        product_div+= "<input type='button', id='basket_btn', value='담기'>";
        $('#content').html(product_div);
    });

    $(document).on('click','#basket_btn',function(){
        
        const quantity=$('#quantity').val();
        const product=$('#product').val();
        const size=$('#size').val();
        /* alert(quantity+":"+product+":"+size); */
        const send_param={product, quantity, size};
        $.post('/basket',send_param,function(returnData){
            alert(returnData.message);
            location.reload();
        });
    });

    $('#purchase').click(function() {
        location.href="/purchase";
    })
```

```
board.js


const con=require('./mysql_con');
const express=require('express');
const router=express.Router();

router.post('/read_content',(req,res,next)=>{  
    con.query(`SELECT * FROM board where bo_no=${req.body.bo_no}`, 
         (err, result)=> {
            if (err)
                console.log(err); 
            con.query(`UPDATE board SET hit=${result[0].hit+1} WHERE bo_no=${req.body.bo_no}`,(err)=>{
                if (err){
                    console.log(err); 
                }else{
                    if(result[0]){
                        res.json({name:result[0].name,title:result[0].title,content:result[0].content});
                    }else{
                      res.json({message:"글 내용 보기 실패"});
                    }
                }
            });     
    }); //end query
    
});

router.get('/read_form',(req,res,next)=>{  
    con.query(`SELECT * FROM board order by bo_no desc`, 
         (err, result)=> {
          if (err) console.log(err); 
          console.log(result.length);       
        res.render('board_read_form',{title:"글읽기 화면",result});
    }); //end query
    
});
router.post('/write',(req,res,next)=>{
    if(req.session.name){//글쓰기 처리

            const sql=`INSERT INTO board (name,title,content) VALUES ( '${req.session.name}','${req.body.board_title}','${req.body.board_content}')`;
            con.query(sql,(err,result)=>{
                if(err){
                    console.error(err);
                    res.json({message:"글 등록 실패"});
                }else{
                    console.log('board insert ok');
                    res.json({message:"글 등록 성공"});
                }
            });

    }else{
        res.json({message:"로그인부터 하세요"});
    }
});

router.get('/write_form',(req,res,next)=>{  
    res.render('board_write_form',{title:"글쓰기 화면"});
    
});

module.exports=router;
```

```
basket.js
=========


const con = require('./mysql_con');
const express=require('express');
const router=express.Router();

router.post('/',(req,res,next)=>{       

        console.log("Connected!");        
        
        if(req.session.name){//basket insert 처리      

          var sql = `INSERT INTO basket (m_no,product,quantity,size) VALUES (${req.session.m_no}, '${req.body.product}',${req.body.quantity}, ${req.body.size})`;
        
          con.query(sql, (err, result) => {
            console.log(req.session);
            if (err) {
              console.log("basket insert fail",err);
              res.json({message:"장바구니 처리 실패"});
            }else{
              console.log("1 record inserted");
              res.json({message:`${req.session.name}님의 장바구니에 담겼습니다`});
            }
          });//end query

        }else{//login 권고
          res.json({message:"로그인부터 하세요"});
        }       
        
 
}); //end post

module.exports=router;
```

```
basket.js
=========

 $('#product_btn').click(function() {
        let product_div= "<h2> 발렌시아가 스피드러너 <h2><br>";
        product_div+= '<div><img src=./image/speedrunner.jpg><div>';
        product_div+= "품명 <input id='product', value='스피드러너'><br>";
        product_div+= "Size <input id='size' ><br> ";
        product_div+= "수량 <input id='quantity' ><br>";
        product_div+= "<input type='button', id='basket_btn', value='담기'>";
        $('#content').html(product_div);
    });

    $(document).on('click','#basket_btn',function(){
        
        const quantity=$('#quantity').val();
        const product=$('#product').val();
        const size=$('#size').val();
        /* alert(quantity+":"+product+":"+size); */
        const send_param={product, quantity, size};
        $.post('/basket',send_param,function(returnData){
            alert(returnData.message);
            location.reload();
        });
    });

    $('#purchase').click(function() {
        location.href="/purchase";
    })
```

