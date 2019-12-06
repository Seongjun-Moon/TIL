# Markdown 문법

## 1. 헤더 Headers

- "#" 을 통해 헤더 크기 조절 (1~6개까지)

``` 
# 가장 큰 제목 H1
## 그 다음 H2
### 그 다음 H3
#### 그 다음 H4
##### 그 다음 H5
###### 그 다음 H6
```



##   2. BlockQuote

이메일에서 사용하는 ">" 블럭인용문자를 이용.

```
> This is a blockquote
```



> This is a blockquote.
>
> > This is a second blockquote.
> >
> > > This is third blockquote.



이 안에 다른 마크다운 요소 포함 가능.

> ###  H3
>
> > - List
> >
> > > ```
> > > code
> > > ```



##  3. 목록

- ### 순서있는 목록(번호)

```
1. 첫번째
2. 두번째
3. 세번째
```



1. 첫번째
2. 두번째
3. 세번째



###### 현재까지는 어떤 번호를 입력해도 순서는 내림차순으로 정의

```
1. 첫번째
3. 세번째
2. 두번째
```

1. 첫번째
2. 세번째
3. 두번째



- ### 순서없는 목록(글머리 기호)

```
* 빨강
	* 녹색
		* 파랑

+ 빨강
	+ 녹색
		+파랑

- 빨강
	- 녹색
		- 파랑
		
세번째 목록 이후에는 네모 점으로 동일.
```

* 빨강
  * 녹색
    * 파랑



+ 빨강
  + 녹색
    + 파랑



- 빨강
  - 녹색
    - 파랑
    - 노랑
    - 보라



혼합하여 사용도 가능

```
* 첫번째
	- 두번째
		+ 세번째
			= 네번째
```

* 첫번째
  - 두번째
    - 세번째 = 네번째



## 4. 코드 '<pre><code></code></pre>'

4개의 공백 또는 하나의 탭으로 들여쓰기를 하면 변화되기 시작하여

들여쓰지 않은 행을 만날때까지 변환이 계속된다.

> 한줄 띄어쓰면 인식이 제대로 안되는 문제가 발생하기도 한다고 함.

```
This is a normal paragraph:

	This is a code block.
end code block.
```

적용하면,

<pre>This is a normal paragraph


	<code>This is a code block.</code>

end code block.</pre>





```
<pre>This is a normal paragraph:
	<code> This is a code block.</code>
</pre>end code block.
```

적용하면,

<pre>This is a normal paragraph:
    <code>This is a code block.</code>
</pre>end code block.



##### 예제1.

<pre>여기서부터 시작
    <code>빨
    주
    노
    초
    파
    남
    보</code>
</pre>여기가 끝.



## 5. 수평선 <hr/>

아래 줄은 모두 수평선을 만든다. 마크다운 문서를 미리보기로 출력할 때 페이지 나누기 용도로 많이 사용.

```
* * *
***
*****
- - - (얇은 선)
------------------------
<hr/> (폭이 넓은 선)
```

<hr/>

<hr/

<hr/>

<hr/>

---

---

---

***

***

*****

****

******

---









