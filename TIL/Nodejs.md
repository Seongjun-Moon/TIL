## Node.js

- 싱글 스레드 / 멀티 스레드

싱글스레드 : 하나의 스레드가 처리되어야 다음 스레드가 처리됨.

멀티스레드 :  여러 스레드를 한번에 처리

![싱글 스레드 블로킹 모델 이미지 검색결과"](https://img1.daumcdn.net/thumb/R800x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FFuINi%2FbtqvBpB5kDn%2FGXWs7S4b9w77UEbFKzdK4K%2Fimg.png)싱글 스레드, 블로킹 모델

---



- 블로킹 / 논블로킹

블로킹 : 1대1 매칭 (ex. 직원1 : 손님1) => 손님이 적어질시 노는 점원이 늘어나게 됨 (비용 낭비)

논블로킹 : 1대 다 매칭(ex. 직원1: 손님 多)



![싱글 스레드 논블로킹 모델 이미지 검색결과"](https://mblogthumb-phinf.pstatic.net/MjAxODExMDhfMTc0/MDAxNTQxNjE4MzczNDQ2.f_XnAPvF81kqwKPV-FmF7aNwl7Bz5YaTOm6oo-wvF8Mg.apfkaeY-cpQZS3dIeAh4eC1S8IeLRpTQ6EPtF1kdC9Yg.PNG.hhw1990/image.png?type=w800)

---





- Callback function

```
console.log("시작");
setTimeout(function(){
    console.log("hello"); setTimeout(function() {
        console.log("world"); setTimeout(function() {
            console.log("끝");
        },0);
    },1000);
},2000);
-------
시작
(2초후) hello
(1초후) world
끝

```



- 프로미스

```
Callback hell을 극복하고자 만들어짐

	function a() {
    	return new Promise((resolve,reject)=>{
        	setTimeout(()=>{
            	resolve("hello");
            },2000);
        });
    }
    
    function b() {
    	return new Promise((resolve,reject)=>{
        	setTimeout(()=>{
            	resolve("world");
            },1000);
        });
    }
    
    
    console.log('시작');
    a()
    .then((result)=>{
    	console.log(result);
        return b();
    })
    .then((result)=>{
    	console.log(result);
        console.log('끝');
    })
    .catch((error)=>{
    	console.error(error);
    });
```

- async/await
```
function a() {
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve("hello");
        },2000);
    });
}

function b() {
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve("world");
        },1000);
    });
}


async function HelloWorld () {
    console.log("시작");
    const result1=await a();
    console.log(result1);
    const result2=await b();
    console.log(result2);
    console.log("끝");
}


HelloWorld();


```
