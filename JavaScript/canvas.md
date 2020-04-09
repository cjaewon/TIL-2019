# Canvas

웹에서 그래픽을 그리기 위해 사용되는 element 

### 🖌 기본 사용법
```js
// html에 canvas element가 있다고 가정
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

const width = canvas.width;
const height = canvas.height;

ctx.fillRect(0, 0, 10, 10);
```

### 🐭 마우스 이벤트 
```js
canvas.addEventListener('mousemove', e => {
  const mosX = e.clientX - this.canvas.offsetLeft;
  const mosY = e.clientY - this.canvas.offsetTop;

  ctx.fillRect(mosX, mosY, 10, 10);
}, false);
```

참고로 밑에 코드처럼 사용하면 다른 화면으로 넘어가거나 다른 창에 있을 때 마우스 위치가 이상하게 될 수 있다.
따라서 위에 코드를 사용해야 한다.
```js
const rect = canvas.getBoundingClientRect();

canvas.addEventListener('click', e => {
  ctx.fillRect(e.clientX - rect.x, e.clientY - rect.y, 10, 10);
});
```