> 수정 날짜 (없음) / 작성 날짜 : (2020-05-6일)

# Node.js Cluster

### ❓ 사용 이유

`Node.js (javascript)` 는 싱글 스레드 언어여서 한개의 코어만 사용할 수 있는데  
지금같이 **32코어 64코어가** 날아다니는 상황에서 코어 한개만 쓴다는 것 굉장히 비효율적이다.

그 해결책은 **노드 프로세스를 복제** 시키면 된다.  
즉 CPU 코어가 32개라면 32개의 노드 프로세스를 생성해서 상황에 맞게 로드 밸런싱 해준다.

로드 밸런싱 및 노드 프로세스 생성을 도와주는 것이 cluster 라는 노드 내장 모듈이다.

### 📖 예제

밑에 코드는 cpu 수에 맞게 프로세서를 실행하는 코드이다

```js
const cluster = require('cluster');
const http = require('http');
const { length: CpuCount } = require('os').cpus();

if (cluster.isMaster) {
  for (let i = 0; i < CpuCount; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`[ERROR] Worker Died | pid = ${process.pid}`);
  });
} else {
  const server = http.createServer((req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
  });

  server.listen(3000);

  console.log(`[INFO] Created Worker | pid = ${process.pid}`);
}
```

그럼 인제 오류가 생겨도 절때 죽지 않는 좀비 cluster를 만들어보자  
방법은 cluster가 오류 때문에 종료되면 새로운 프로세스를 바로 만들면 된다.

```js
const cluster = require('cluster');
const http = require('http');
const { length: CpuCount } = require('os').cpus();

if (cluster.isMaster) {
  for (let i = 0; i < CpuCount; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`[ERROR] Worker Died | pid = ${process.pid}`);
    cluster.fork();
  });
} else {
  const server = http.createServer((req, res) => {
    res.writeHead(200);
    res.end('hello world\n');
  });

  server.listen(3000);

  console.log(`[INFO] Created Worker | pid = ${process.pid}`);
}
```

### 🚧 설계

app.js 에서는 서버를 리턴하고 server.js에서는 cluster로  
서버를 실행시키면 좋을 것 같다.

```js
// app.js
const koa = require('koa');

const app = new Koa();

app.use(ctx => {
  ctx.body = '<h1>Hello World</h1>';
});

module.exports = app;
```

```js
// server.js
const cluster = require('cluster');
const http = require('http');
const { length: CpuCount } = require('os').cpus();

const app = require('./app');

if (cluster.isMaster) {
  for (let i = 0; i < CpuCount; i++) {
    cluster.fork();
  }

  cluster.on('exit', (worker, code, signal) => {
    console.log(`[ERROR] Worker Died | pid = ${process.pid}`);
    cluster.fork();
  });
} else {
  app.listen(3000);

  console.log(`[INFO] Created Worker | pid = ${process.pid}`);
}
```

이렇게 koa랑 server.js를 사용해 cluster를 구현해보았다.

만약 Docker을 쓴다면 conatiner 두개를 만들어
cpu / 2로 클러스터링하면 나중에 무중단 배포하기 쉬울 것 같다 