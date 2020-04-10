> 수정 날짜 (2020-04-08) / 작성 날짜 : (2020-04-08)

# WebSocket

> " 웹소켓(WebSocket)은 하나의 TCP 접속에 전이중 통신 채널을 제공하는 컴퓨터 통신 프로토콜이다. 웹소켓 프로토콜은 2011년 IETF에 의해 RFC 6455로 표준화되었으며 웹 IDL의 웹소켓 API는 W3C에 의해 표준화됨 " 

- 위키백과  

**즉 기존 http와 다르게 웹 소켓은 양방향 통신이 가능하고 빠른 것이 특징이다.**

### 🕸️ 웹소켓 사용
Node.js에서는 [ws](https://github.com/websockets/ws) 모듈을 사용한다.
```js
const ws = require('ws');

const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws) => {
  ws.on('message', (message) => {
    console.log(message);
  });

  ws.send('hello-world');
});

```

Node.js 에서 웹 소켓 연결
```js
const WebSocket = require('ws');

const ws = new WebSocket('ws://www.host.com/path');

ws.on('open', () => {
  ws.send('hello-world-by-client');
});

ws.on('message', (message) => {
  console.log(message);
});
```

express랑 같이 사용
```js
const http = require('http');
const express = require('express');
const ws = require('ws');

const app = express();
const server = http.createServer(app);

const wss = new ws.Server({
  server,
});

app.use(express.static(__dirname + '/public'));

wss.on('connection', (ws, req) => {
  const id = req.headers['sec-websocket-key'];

  ws.on('message', (data) => {
    console.log(id, data);
  });

  ws.on('close', () => {
    console.log(id, 'close');
  });
});

server.listen(3000, () => {
  console.log('start web server');
});
```


### 🕸️ 웹소켓 설계
일단 `event`라는 폴더를 만든 후 index.js에서 on message 이벤트를 받는다.

```
server.js
- event
  - index.js
```

```js
// server.js
const http = require('http');
const express = require('express');
const ws = require('ws');

const event = require('./event');

const app = express();
const server = http.createServer(app);

const wss = new ws.Server({
  server,
});

app.use(express.static(__dirname + '/public'));

wss.on('connection', (ws, req) => {
  const id = req.headers['sec-websocket-key'];
  // id를 넘겨주는 대신 ws.id = id;를 해도 된다.
  ws.on('message', data => event(ws, id, data));
});

server.listen(3000, () => {
  console.log('start web server');
});
```

```js
// event/index.js
const bind = (name, data) => {
  return JSON.stringify({
    name,
    data,
  });
};

module.exports = (ws, id, socketData) => {
  const response = JSON.parse(socketData);

  const name = response.name; // 이벤트 이름
  const data = response.data; // 데이터

  switch (name) {
    case 'join': { // 스코프 때문에 대괄호를 사용한다.
      const body = { a: '123', b: '234' };
      ws.send(bind('hello', body));

      break;
    }
  }
};
```

### socket.io 쓰지 않는 이유
구버전 브라우저는 웹 소켓을 지원 안 해서 socket.io 라는 모듈을 사용해 각종 방법으로  
웹소켓 처럼 구현 가능하다

하지만 현대 브라우저는 웹 소켓을 다 지원하고 구버전 브라우저는 사라지는 추세여서 socket.io를 쓸 이유도  
사라지고 있다

또한 socket.io는 websocket 보다 느리고 클러스터링 할때 redis (전에 연결 했던 클러스터랑 연결하기 위해서) 를 사용해주어야 하는 단점이 있다.