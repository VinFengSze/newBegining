# 代码执行顺序解析

这段代码展示了 JavaScript 事件循环中同步任务、微任务(microtask)和宏任务(macrotask)的执行顺序。以下是详细的输出解释：

```ts
async function async1() {
	console.log('async1 start');
	await async2();
	console.log('asnyc1 end');
}
async function async2() {
	console.log('async2');
}
console.log('script start');
setTimeout(() => {
	console.log('setTimeOut');
}, 0);
async1();
new Promise(function (reslove) {
	console.log('promise1');
	reslove();
}).then(function () {
	console.log('promise2');
})
console.log('script end');
```





## 完整输出顺序

```ts
script start
async1 start
async2
promise1
script end
asnyc1 end
promise2
setTimeOut
```

## 分步执行过程

### 1. 同步代码执行阶段

1. `console.log('script start')` - 同步代码，立即执行
2. `setTimeout` - 将回调加入**宏任务队列**（等待执行）
3. 调用 `async1()`
    - `console.log('async1 start')` - 同步执行
    - `await async2()` 调用 `async2()`
        - `console.log('async2')` - 同步执行
    - `await` 将 `async1` 函数剩余部分包装为微任务
4. `new Promise` 执行
    - `console.log('promise1')` - 同步执行（executor函数同步执行）
    - `reslove()` 将 then 回调加入**微任务队列**
5. `console.log('script end')` - 同步代码，立即执行

### 2. 微任务执行阶段（当前调用栈清空后）

1. `await async2()` 后的代码（`console.log('asnyc1 end')`）
2. `promise.then` 的回调（`console.log('promise2')`）

### 3. 宏任务执行阶段（所有微任务完成后）

1. `setTimeout` 的回调（`console.log('setTimeOut')`）

## 关键点说明

1. **执行顺序优先级**：

    - 同步代码 > 微任务 > 宏任务

2. **await 的隐式 Promise**：

    ```ts
    await async2();
    // 等价于
    Promise.resolve(async2()).then(() => { /* 剩余代码 */ });
    ```

3. **微任务队列顺序**：

    - 先加入队列的先执行（`async1 end` 比 `promise2` 先加入微任务队列）

4. **事件循环阶段**：

    ```
    [当前调用栈] → [微任务队列] → [渲染] → [宏任务队列]
    ```