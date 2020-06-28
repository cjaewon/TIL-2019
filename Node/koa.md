> 수정 날짜 : 없음 / 작성 날짜 : (2020-06-28)

# Koa 사용법 정리

### 📦 koa-static

```js
app.use(serve(path.resolve(__dirname, '../dist')));
```
`__dirname + '../dist/` 하지말고 path.resolve 사용하자.  
**koa-static** 에서 인식 못함
