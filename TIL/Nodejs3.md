## Nodejs

#### 프로젝트 작업 프로세스

- mkdir projectX
- cd projectX
- npm init
- npm i express
- package.json에 start 속성 넣기 //
- app.js 작성
- require('express') ... listen(port);
- public 폴더에 html, css, js, img ....
- localhost:port 확인
- client.js 작성
- $.post('url', send_param, callback);
- app.js 에서 app.post('/url', callback);
- app.js에 app.use(express.json());