## setTimeout函数经典闭包函数聊一聊
我们经常会遇到下面这样一道题,说出运行之后的输出结果：

```javascript
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
}
 
console.log(new Date, i); 
```

这段代码其实是输出了5个‘5’的，循环体里面的四个，外加外面的一个。这其实是考察了对 JS 中同步和异步代码的区别、变量作用域、闭包等概念的理解，若要正确的输出，可使用闭包解决：

```javascript
for (var i = 0; i < 5; i++) {
    (function(j){
      setTimeout(function() {
        console.log(new Date, j);
      }, 1000);
    })(i);
}
 
console.log(new Date, i); 
```

到现在可以正确的输出我们想要的结果了，对于了解es6的同学来说，es6里面有个“==let==”变量可以取代“==var=="变量，由于“==let==”作用域只在当前作用域内有效，所有我们有了更为简单的写法：

```javascript
for (let i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
}
 
console.log(new Date, i); 
```

这种写法固然好，循环体可以输出我们想要的结果，但是外面的“i”会报错，因为它所在的作用域里面并没有声明。

- ES6 解决方案

那么现在抛出一个新的问题：如果期望代码的输出变成 0 -> 1 -> 2 -> 3 -> 4 -> 5，并且要求原有的代码块中的循环和两处 console.log 不变，该怎么改造代码？

我们知道在ES6里面提供了新的Promise特性，使得异步做法简单优雅了许多，下面我们看下使用Promise后的解法：

```javascript
const tasks = [];
const output = (i) => new Promise((resolve) => {
  setTimeout(()=>{
        console.log(new Date(),i);
        resolve();  //这里要调用resolve方法，否则代码不会按照预期的运行
  },1000*i);        //定时器的时间逐步增加即可
});

for(var i = 0;i<5;i++){
      tasks.push(output(i));
}

Promise.all(tasks).then(()=>{
  setTimeout(()=>{
        console.log(new Date(),i);
  },1000);    //这里把时间设置为1秒即可
});

```

那么是不是到此就已经完美解决了呢，我们知道在ES7里面对对于异步操作提供了新的特性，下面我们看看在ES7里面是怎么解决的

- ES7解决方案

```javascript
const sleep = (time) => new Promise((resolve)=>{
      setTimeout(resolve,time);
});

//定义一个立即执行函数IIFE（Immediately Invoked Function Expression：声明即执行的函数表达式）
(async ()=>{
  for(var m = 0; m < 5; m++){
        await sleep(1000);
        console.log(new Date(),m);
  }

  await sleep(1000);
  console.log(new Date(),m);
})();
```


