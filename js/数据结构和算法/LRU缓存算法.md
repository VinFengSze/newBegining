```ts
class LRUCache {
  constructor(capacity) {
    this.capacity = capacity; // 最大容量
    this.cache = new Map();   // 使用 Map 保持有序
  }

  get(key) {
    if (!this.cache.has(key)) {
      return -1; // 不存在
    }

    // 先取出，再重新放入，目的是更新“最近使用”
    const value = this.cache.get(key);
    this.cache.delete(key);      // 删除原位置
    this.cache.set(key, value);  // 重新插入，放到最后（表示最近使用）
    return value;
  }

  put(key, value) {
    if (this.cache.has(key)) {
      this.cache.delete(key); // 删除旧的（为了更新顺序）
    } else if (this.cache.size >= this.capacity) {
      // 淘汰最久未使用的：Map 的第一个元素
      const oldestKey = this.cache.keys().next().value;
      this.cache.delete(oldestKey);
    }

    this.cache.set(key, value); // 插入新值
  }
}

```

## ✅ 使用示例

```ts

const lru = new LRUCache(3);

lru.put(1, 'A');
lru.put(2, 'B');
lru.put(3, 'C');

console.log(lru.get(2)); // B，访问了 key=2，顺序更新

lru.put(4, 'D'); // 插入新值，会淘汰最早的 key=1

console.log(lru.get(1)); // -1，key=1 被淘汰
console.log(lru.get(3)); // C
console.log(lru.get(4)); // D
```

------

## ✅ 内部机制简图（Map 顺序）

```ts

// 初始插入：
put(1, A) -> [1]
put(2, B) -> [1, 2]
put(3, C) -> [1, 2, 3]

// get(2) -> [1, 3, 2]（2 被访问，移到末尾）
// put(4, D) -> 淘汰 1 -> [3, 2, 4]
```