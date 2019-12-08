#####  블록체인코드 예시

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
		
		/* 마지막 블록을  반환하는 함수를 정의*/
		Blockchain.prototype.getLastBlock = function() {
			return this.chain[this.chain.length - 1];
		}
		
		jscoin.createNewBlock("3333", 400, "4444");
		jscoin.createNewBlock("4444", 500, "5555");
		jscoin.createNewBlock("5555", 600, "6666");
		console.log(jscoin.getLastBlock());
		
		
		/* 트랜잭션을 정의하는 함수*/
		Blockchain.prototype.createNewTransaction = function(sender, recipient, amount) {
			const newTransaction = {
					sender : sender,
					recipient : recipient,
					amount : amount
			}
			
			this.pendingTransaction.push(newTransaction);
			
			return;
		}
		
		jscoin.createNewBlock("0000", 100, "1111");
		jscoin.createNewTransaction("aaa", "bbb", 1000);
		console.log(jscoin); // 대기 트랜잭션이 존재하는 것을 확인 
		
		jscoin.createNewBlock("1111", 200, "2222");
		console.log(jscoin); // 대기 트랜잭션이 사라진 것을 확인

		const sha256 = require('sha256');
		
		/* 블록 해쉬값을 구하는 함수 정의 */
		Blockchain.prototype.hashBlock = function(previousBlockHash, currentBlockDat, nonce) {
			const data = previousBlockHash + nonce.toString() + JSON.stringify(currentBlockData);
			const hash = sha256(data);
			
			return hash;
		}
		
		// 테스트 코드
		const previousBlockHash = "previousBlockHash";
		const currentBlockData = [
			{ sender : "a", recipient : "b", amount : 100},
			{ sender : "b", recipient : "d", amount : 200},
			{ sender : "e", recipinet : "f", amount : 300},
		];
		const nonce = 100;
		
		const hashBlock = jscoin.hashBlock(previousBlockHash, currentBlockData, nonce);
		console.log(hashBlock);



	</script>
</body>
</html>
```



> 'sha256' 을 실행하기 위해서는 nodejs을 다운받아 npm을 이용해 sha256 모듈을 추가해야함

```
설치 완료 후 명령어창을 실행해서 설치를 확인
C:\Users\myanj>node --version
v12.13.1 ⇐ 정상 설치 확인

작업 디렉터리 생성 및 초기화
C:\Users\myanj>cd C:\SecureCoding

C:\SecureCoding>mkdir blockchain

C:\SecureCoding>cd blockchain

C:\SecureCoding\blockchain>npm init

메모장을 이용해서 C:\SecureCoding\blockchain\jscoin.js 파일을 생성

명령어 창에서 아래와 같은 형식으로 실행을 테스트 
C:\SecureCoding\blockchain>node ./jscoin.js

npm을 이용해서 sha256 모듈을 추가
-- 다음과 같이 나와야 함 --
C:\SecureCoding\blockchain>npm install sha256 --save
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN blockchain@1.0.0 No description
npm WARN blockchain@1.0.0 No repository field.

+ sha256@0.2.0
added 3 packages and audited 3 packages in 0.539s
found 0 vulnerabilities

```



- BlockChain 정의 부분에 난이도 목표를 추가

```
/* 블록체인 객체 정의 */
function Blockchain() {
	this.difficulty = '0000'; // <-- 난이도 모굪
	this.chain = [];
	this.pendingTransactions = [];
}
```

- 난이도를 체크하는 함수

```
Blockchain.prototype.checkDifficuly = function(hash) {
	let head = hash.substring(0, this.difficulty);
	return (head,match(/0/g) || []).length == this.difficulty;
}
```



- 작업 증명 코드 및 테스트를 추가

```
/* 작업증명 코드를 추가*/
Blockchain.prototype.pow = function(previousBlockHash, currentBlockData) {
	let nonce = 0;
	let hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	while (hash,substring(0,4) !=this.difficulty) {
		nonce ++;
		hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	}
	return nonce;
}

// 테스트코드
const previousBlockHash = "previousBlockHash";
const currentBlockData = [
	{ sender : "a", recipient : "b", amount : 100 },
	{ sender : "c", recipient : "d", amount : 200 },
	{ sender : "e", recipient : "f", amount : 300 }
];

const nonce = jscoint.pow(previousBlockHash, currentBlockData);
console.log(nonce);

const hash = jscoin.hashBlock(previousBlockHash, currentBlockData, nonce);
console.log(hash);

```

nodejs 실행

```
C:\SecureCoding\blockchain>node ./jscoin.js
133272
00004860cbe802c8181eda2c8c91107463e101c62e6a39768b00b11df8fcdd3d

```



- 제너시스 블록 생성 코드를 추가

```
/* 블록체인 객체 정의 */
function Blockchain() {
	this.difficulty = '0000'; // <-- 난이도 목표
	this.chain = [];
	this.pendingTransactions = [];
	
	// 제너시스 블록을 생성
	this.createNewBlock(0, 0, 0);
}

	:
const jscoin = new Blockchain();

console.log(jscoin);  // <-- 제너시스 블록 생성을 확인
	:
```



- 난이도 목표를 숫자로 변경

```
/* 블록체인 객체 정의 */
function Blockchain() {
	this.difficulty = 4; // <-- 난이도 목표를 숫자로 변경
	this.chain = [];
	this.pendingTransactions = [];

	// 제너시스 블록을 생성
	this.createNewBlock(0, 0, 0);
}

           :

/* 난이도를 체크하는 함수 */
Blockchain.prototype.checkDifficulty = function(hash) {
	let head = hash.substring(0, this.difficulty);
	return (head.match(/0/g) || []).length == this.difficulty;
}


/* 작업증명 코드를 추가 */
Blockchain.prototype.pow = function(previousBlockHash, currentBlockData) {
	let nonce = 0;
	let hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	while (!this.checkDifficulty(hash)) {
		nonce ++;
		hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	}
	return nonce;
}

```



---

---

---

---

---



- 최종 코드

```
function Blockchain() {
	this.difficulty = 4; // <-- 난이도 목표
	this.chain = [];
	this.pendingTransactions = [];

	//제너시스 블록을 생성
	this.createNewBlock(0, 0, 0);

}

/* 블록체인 생성 함수 정의 */ 
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



/* 마지막 블록을 반환하는 함수를 정의 */ 
Blockchain.prototype.getLastBlock = function() {
	return this.chain[this.chain.length - 1];
}
		
/* 트랜잭션을 생성하는 함수를 정의 */
Blockchain.prototype.createNewTransaction = function(sender, recipient, amount) {
	const newTransaction = {
		sender : sender, 
		recipient : recipient, 
		amount : amount
	}
	
	this.pendingTransactions.push(newTransaction);
	
	return;
}


jscoin.createNewBlock("0000", 100, "1111");
		jscoin.createNewTransaction("aaa", "bbb", 1000);
		console.log(jscoin); // 대기 트랜잭션이 존재하는 것을 확인 
		
		jscoin.createNewBlock("1111", 200, "2222");
		console.log(jscoin); // 대기 트랜잭션이 사라진 것을 확인

const sha256 = require('sha256');

/* 블록 해쉬값을 구하는 함수 정의 */ 
Blockchain.prototype.hashBlock = function(previousBlockHash, currentBlockData, nonce) {
	const data = previousBlockHash + nonce.toString() + JSON.stringify(currentBlockData);
	const hash = sha256(data);

	return hash;
}

/* // 테스트 코드 
const previousBlockHash = "previousBlockHash";
const currentBlockData = [
	{ sender : "a", recipient : "b", amount : 100 }, 
	{ sender : "c", recipient : "d", amount : 200 },
	{ sender : "e", recipient : "f", amount : 300 } 
];
const nonce = 100;

const hashBlock = jscoin.hashBlock(previousBlockHash, currentBlockData, nonce);
console.log(hashBlock); 해당 테스트 코드를 작업증명 테스트 코드와 같이 쓰면 오류*/


/* 난이를 체크하는 함수 */
Blockchain.prototype.checkDifficulty = function(hash) {
	let head = hash.substring(0, this.difficulty);
	return (head.match(/0/g) || []).length == this.difficulty;
}


/* 작업증명 코드를 추가 */
Blockchain.prototype.pow = function(previousBlockHash, currentBlockData) {
	let nonce = 0;
	let hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	while (!this.checkDifficulty(hash)) {
		nonce ++;
		hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	}
	return nonce;
}







/* 작업증명 코드를 추가 
Blockchain.prototype.pow = function(previousBlockHash, currentBlockData) {
	let nonce = 0;
	let hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	while (hash.substring(0, 4) != this.difficulty) {
		nonce ++;
		hash = this.hashBlock(previousBlockHash, currentBlockData, nonce);
	}
	return nonce;
} */





// 테스트 코드 
/*
const previousBlockHash = "previousBlockHash";
const currentBlockData = [
	{ sender : "a", recipient : "b", amount : 100 }, 
	{ sender : "c", recipient : "d", amount : 200 },
	{ sender : "e", recipient : "f", amount : 300 } 
];

const nonce = jscoin.pow(previousBlockHash, currentBlockData);
console.log(nonce);

const hash = jscoin.hashBlock(previousBlockHash, currentBlockData, nonce);
console.log(hash); */







```



