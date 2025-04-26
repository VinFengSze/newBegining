```ts
const person = {
  name: "John",
  get aliName() {
    console.log(this);
    return this.name + " ali";
  },
};
const proxy = new Proxy(person, {
  get(target, key, receiver) {
    console.log("get", key);
    return target[key];
    // return Reflect.get(target, key, receiver);
  },
  set(target, key, value, receiver) {
    console.log("set", key, value);
    return Reflect.set(target, key, value, receiver);
  },
});
proxy.aliName;

```

**若用以上写法，当我访问proxy.aliName时，会触发get aliName函数。导致触发this.name访问，这个时候按道理也要触发proxy对象中的get方法。但实际结果却没有。******因为这个时候this指向person对象****
