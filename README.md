# express-wrap-async

## Usage

```js
import wrapAsync from 'express-wrap-async';
```


### Promise example
```js
function routePromise(req, res) {
  return new Promise((resolve) => {
    res.send({ ok: true });
    resolve();
  });
}

router.get('/promise', wrapAsync(routePromise));

```

### Babel example:
```js
const fakeWait = () => new Promise((resolve) => setTimeout(resolve, 100));

async function asyncRoute(req, res) {
  await fakeWait();
  res.send({ ok: true });
}
router.get('/async', wrapAsync(asyncRoute));
```

### Multiple middlewares
```js
const middlewares = [
  async(req, res, next) => {
    await fakeWait();
    next();
  },
  (req, res, next) => {
    // non async middleware will also work
    next();
  },
  asyncRoute,
];

router.get('/middlewares', wrapAsync(middlewares));
```


### Error handling
```js
async function errorRoute(req, res) {
  await fakeWait();
  throw new Error('unexpected error');
}
router.get('/standard', wrapAsync(standardRoute));

app.use((err, req, res, next) => {
  res.status(500);
  res.json({
    error: err.message,
  });
});

```



MIT License

Copyright (c) 2016 Łukasz Sentkiewicz