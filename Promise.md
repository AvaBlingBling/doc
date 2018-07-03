# ES6的Promise对象

Promise是异步编程的一种解决方案，目的是为了解决回调地狱问题

## 1. 三种状态
pending -> resolved, 会执行resolve方法
pending -> rejected, 会执行reject方法

```
let promise = new Promise ( (resolve, reject) => {
    if ( success ) {
        resolve(a) // pending ——> resolved 参数将传递给对应的回调方法
    } else {
        reject(err) // pending ——> rejectd
    }
} )
```

## 2. then和catch方法（原型链方法）
then包含两个参数，分别是状态为resolved时的回调函数和状态rejected时的回调函数
```
promise.then(
    data => console.log('this is resolved status'),
    err => console.log('this is rejected status'),
)
```

catch捕获Promise的错误，同时也会捕获then中抛出的错误，所以习惯性写法如下：

```
promise.then(
    data => console.log('this is success callback')
).catch(
    err => console.log(err),
)
```

## 3. resolve和reject方法（构造函数方法）
返回resolved状态或者rejected状态的Promise对象

## 4. all和race方法（构造函数方法）
参数为Promise对象数组，如不是，会通过Promise.resolve()方法转换;返回执行所有resolve方法的传入参数组成的数组；
```
let p1 = new Promise ( (resolve, reject) => {
    if ( true ) {
        resolve(2345);
    } else {
        reject('err');
    }
} );
let p2 = 34;

let promise = Promise.all([p1,p2]);
promise.then( data => {
    console.log(data); // [2345, 34]
})
```
race与all不同，只要参数中有一个Promise对象改变了状态就会执行对应的回调函数