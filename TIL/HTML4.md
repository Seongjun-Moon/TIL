## HTML

footer 하단 고정

```
- html, body의 height를 100%로 설정
- content의 height가 browser의 height보다 짧은 경우를 위해 div.wrap class에 min-height: 100% 설정
- footer는 position: absolute로 하여 bottom: 0에 고정
- footer를 content기준으로 위치를 잡기 위해 div.wrap class를 position: relative로 설정
- footer가 content(section)과 겹치지 않게 하기위해 div.wrap에 footer높이 만큼 padding-bottom을 지정
- padding-bottom에 지정한 값을 영역값으로 잡기 위해 box-sizing을 border-box로 지정
```

Bootstrap 이용해보기
https://www.w3schools.com/bootstrap/default.asp
https://www.w3schools.com/bootstrap4/default.asp
