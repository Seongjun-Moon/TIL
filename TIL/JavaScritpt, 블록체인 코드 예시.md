#####  JavaScritpt, 블록체인 코드 예시

- Ex1.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>JScoin</h1>
	<script>
		/* JavaScript에서 객체를 정의하는 방법 */
		/* Case 1 */
		function User(name, age) {
				this.name = name;
				this.age = age;
		}
		
		var user1 = new User("이십대", 20);
		var user2 = new User("삼십대", 30);
		var user3 = new User("사십대", 40);
		
		console.log(user1);
		console.log(user2);
		console.log(user3);
		
		/* Case2 */
		class UserClass {
				constructor(name, age) {
						this.name = name;
						this.age = age;
				}
		}
		
		var user10 = new UserClass("이십대", 20);
		var user20 = new UserClass("삼십대", 30);
		var user30 = new UserClass("사십대", 40);
		
		console.log(user10);
		console.log(user20);
		console.log(user30);
		
		/* JavaScript 프로토타입 객체 */
			User.prototype.domain = "test.com";
			console.log(user1.domain);
			console.log(user2.domain);
			console.log(user3.domain);
			
			console.log(user10.domain); // undefined
			
			User.prototype.getEmail = function() {
					return this.name + "@" + this.domain;
			}
			console.log(user1.getEmail());
			console.log(user2.getEmail());
			console.log(user3.getEmail());
			
	</script>
</body>
</html>


```



---

---

---

---



- Ex2.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>JScoin</h1>
	<script>
		/* 블록체인 객체 정의 */
		function Blockchain() {
			this.chain = [];
			this.pendingTransaction = [];
		}
		
		/* 블록체인 생성 함수 */
		Blockchain.prototype.createNewBlock = function(previousBlockHash, nonce, hash) {
			const newBlock = {
				index : this.chain.length + 1,
				timestamp : Date.now(), 
				transaction : this.pendingTransactions, 
				nonce : nonce, 
				hash : hash, 
				previousBlockHash : previousBlockHash
			};
			
			this.pendingTransactions = [];
			this.chain.push(newBlock);
			
			return newBlock;
		}
		



	</script>
</body>
</html>
```



---

---

---

---



- Ex3

```
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>JScoin</h1>
	<script>
		/* 블록체인 객체 정의 */
		function Blockchain() {
			this.chain = [];
			this.pendingTransaction = [];
		}
		
		/* 블록체인 생성 함수 */
		Blockchain.prototype.createNewBlock = function(previousBlockHash, nonce, hash) {
			const newBlock = {
				index : this.chain.length + 1,
				timestamp : Date.now(), 
				transaction : this.pendingTransactions, 
				nonce : nonce, 
				hash : hash, 
				previousBlockHash : previousBlockHash
			};
			
			this.pendingTransactions = [];
			this.chain.push(newBlock);
			
			return newBlock;
		}
		
		
		


	</script>
</body>
</html>
```



---

---

---

---



- Ex4.

```
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Insert title here</title>
</head>
<body>
	<h1>JScoin</h1>
	<script>
		/* 블록체인 객체 정의 */
		function Blockchain() {
			this.chain = [];
			this.pendingTransaction = [];
		}
		
		/* 블록체인 생성 함수 */
		Blockchain.prototype.createNewBlock = function(previousBlockHash, nonce, hash) {
			const newBlock = {
				index : this.chain.length + 1,
				timestamp : Date.now(), 
				transaction : this.pendingTransactions, 
				nonce : nonce, 
				hash : hash, 
				previousBlockHash : previousBlockHash
			};
			
			this.pendingTransactions = [];
			this.chain.push(newBlock);
			
			return newBlock;
		}
		
		
		const jscoin = new Blockchain();
		console.log(jscoin);
		
		
		// 새로운 블록을 생성
		jscoin.createNewBlock("0000", 100, "1111");
		jscoin.createNewBlock("1111", 200, "2222");
		jscoin.createNewBlock("2222", 300, "3333");
		console.log(jscoin);
		
		/* 마지막 블록을 반환하는 함수를 정의 */
		Blockchain.prototype.getLastBlock = function() {
			return this.chain[this.chain.length - 1];
		}
	
		jscoin.createNewBlock("3333", 400, "4444");
		jscoin.createNewBlock("4444", 500, "5555");
		jscoin.createNewBlock("5555", 600, "6666");
		console.log(jscoin.getLastBlock());
		
		/* 트랜잭션을 정의하는 함수 */
		Blockchain.prototype.createNewTransaction = function(sender, recipient, amount) {
			const newTransaction = {
					sender : sender,
					recipient : recipient,
					amount : amount
			}
			
			this.pendingTransaction.push(newTransaction);
			
			return;
		}
		
		jscoin.createNewTransaction("aaa", "bbb", 1000);
		jscoin.createNewBlock("0000", 100, "1111");
		console.log(jscoin); // 대기 트랜잭션이 존재하는 것을 확인
		
		jscoin.createNewBlock("1111", 200, "2222");
		console.log(jscoin); // 대기 트랜잭션이 사라진 것을 확인
		



	</script>
</body>
</html>
```

