```ts
			const obj = {
				name: 'swf',
				get aliname() {
					return 'ali' + this.name;
				},
			}
			const proxy = new Proxy(obj, {
				get(target, key) {
					console.log('get', key);
					return target[key];
				},
				set(target, key, value) {
					console.log('set', key, value);
					target[key] = value;
					return true;
				}
			})
			effect(() => {
				console.log(proxy.aliname); // 这里收集依赖的时候，若不用Reflect, 访问aliname 触发get aliname()方法，this指向的是obj,而不是prox对象。导致依赖没有收集。我去修改proxy.name 不会触发effect回调
			})
			proxy.name = 'sss';
```

