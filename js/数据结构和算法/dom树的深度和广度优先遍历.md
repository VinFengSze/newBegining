```ts
const tree = {
  value: "A",
  children: [
    {
      value: "B",
      children: [
        { value: "D", children: [] },
        { value: "E", children: [] }
      ]
    },
    {
      value: "C",
      children: [
        { value: "F", children: [] },
        { value: "G", children: [] }
      ]
    }
  ]
};

```

**树形结构可视化**：

```
        A
       / \
      B   C
     / \  / \
    D  E F   G
```

### **1. 深度优先遍历（DFS）**

#### **遍历顺序（前序：根 → 子节点）**

```
A → B → D → E → C → F → G
```

#### **显式栈实现（非递归）**

```ts
function dfsStack(root) {
  const stack = [root];
  while (stack.length) {
    const node = stack.pop();    // 从栈顶取出节点
    console.log(node.value);
    // 子节点逆序入栈（保证左到右遍历）
    for (let i = node.children.length - 1; i >= 0; i--) {
      stack.push(node.children[i]);
    }
  }
}
dfsStack(tree); // 输出：A → B → D → E → C → F → G
```

### **2. 广度优先遍历（BFS）**

#### **遍历顺序（按层级）**

```
A → B → C → D → E → F → G
```

#### **队列实现代码**

```ts
function bfs(root) {
  const queue = [root];
  while (queue.length) {
    const node = queue.shift();  // 从队头取出节点
    console.log(node.value);
    node.children.forEach(child => {
      queue.push(child);        // 子节点入队
    });
  }
}
bfs(tree); // 输出：A → B → C → D → E → F → G
```

