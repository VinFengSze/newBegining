## 1、CommonJs

### **1.1 背景**

- CommonJS 是 Node.js 的默认模块系统，诞生于 2009 年。
- 它的设计目标是让 JavaScript 能够在服务器端（如 Node.js）实现模块化。
- CommonJS 的核心思想是**同步加载模块**，适合服务器端环境。

### **1.2 语法**

#### **导出模块**

使用 `module.exports` 或 `exports` 导出模块。

- **导出单个值：**

  ```javascript
  // math.js
  function add(a, b) {
      return a + b;
  }
  module.exports = add;
  ```

- **导出多个值：**

  ```javascript
  // math.js
  function add(a, b) {
      return a + b;
  }
  function subtract(a, b) {
      return a - b;
  }
  module.exports = { add, subtract };
  ```

  或者：

  ```javascript
  // math.js
  exports.add = function(a, b) {
      return a + b;
  };
  exports.subtract = function(a, b) {
      return a - b;
  };
  ```

#### **导入模块**

使用 `require` 导入模块。

- **导入单个值：**

  ```javascript
  // main.js
  const add = require("./math.js");
  console.log(add(1, 2)); // 3
  ```

- **导入多个值：**

  ```javascript
  // main.js
  const math = require("./math.js");
  console.log(math.add(1, 2)); // 3
  console.log(math.subtract(5, 3)); // 2
  ```

------

### **1.3 运行机制**

- **同步加载**：CommonJS 模块是同步加载的，模块文件在运行时被读取并执行。
- **缓存机制**：模块在第一次加载后会被缓存，后续的 `require` 调用会直接返回缓存的结果。
- **动态加载**：模块的依赖关系是在运行时确定的，`require` 可以在代码的任何地方调用。

### **1.4 适用场景**

- **Node.js 环境**：CommonJS 是 Node.js 的默认模块系统，适合服务器端开发。
- **同步加载**：适合模块文件较小、加载速度较快的场景。

------

## 2、EsModule

### **2.1 背景**

- ES Module 是 JavaScript 的官方模块系统，于 2015 年随 ES6（ECMAScript 2015）引入。
- 它的设计目标是让 JavaScript 在浏览器和服务器端都能实现模块化。
- ES Module 的核心思想是**静态加载模块**，适合现代 JavaScript 开发。

------

### **2.2 语法**

#### **导出模块**

使用 `export` 导出模块。

- **导出单个值：**

  ```javascript
  // math.js
  export function add(a, b) {
      return a + b;
  }
  ```

- **导出多个值：**

  ```javascript
  // math.js
  export function add(a, b) {
      return a + b;
  }
  export function subtract(a, b) {
      return a - b;
  }
  ```

- **默认导出：**

  ```javascript
  // math.js
  export default function add(a, b) {
      return a + b;
  }
  ```

#### **导入模块**

使用 `import` 导入模块。

- **导入单个值：**

  ```javascript
  // main.js
  import { add } from "./math.js";
  console.log(add(1, 2)); // 3
  ```

- **导入多个值：**

  ```javascript
  // main.js
  import { add, subtract } from "./math.js";
  console.log(add(1, 2)); // 3
  console.log(subtract(5, 3)); // 2
  ```

- **导入默认导出：**

  ```javascript
  // main.js
  import add from "./math.js";
  console.log(add(1, 2)); // 3
  ```

- **导入整个模块：**

  ```javascript
  // main.js
  import * as math from "./math.js";
  console.log(math.add(1, 2)); // 3
  console.log(math.subtract(5, 3)); // 2
  ```

------

### **2.3 运行机制**

- **静态加载**：ES Module 是静态加载的，模块的依赖关系在代码运行前就已经确定。
- **异步加载**：ES Module 支持异步加载，适合浏览器环境。
- **严格模式**：ES Module 默认启用严格模式（`"use strict"`）。

------

### **2.4 适用场景**

- **浏览器环境**：ES Module 是浏览器的原生模块系统，适合现代前端开发。
- **静态加载**：适合模块化清晰、依赖关系明确的场景。
- **跨平台**：ES Module 可以在浏览器和 Node.js 中使用。

------

## **3. CommonJS 和 ES Module 的区别**

| **特性**     | **CommonJS**                                       | **ES Module**                         |
| :----------- | :------------------------------------------------- | :------------------------------------ |
| **加载方式** | 同步加载                                           | 静态加载（支持异步）                  |
| **语法**     | `require` / `module.exports`                       | `import` / `export`                   |
| **运行环境** | Node.js                                            | 浏览器和 Node.js（现代版本）          |
| **模块解析** | 运行时解析                                         | 编译时解析                            |
| **严格模式** | 默认不启用                                         | 默认启用                              |
| **缓存机制** | 模块加载后会被缓存                                 | 模块加载后会被缓存                    |
| **动态加载** | 支持动态加载（`require` 可以在代码的任何地方调用） | 不支持动态加载（`import` 必须在顶层） |

------

### **3.1 如何在 Node.js 中使用 ES Module**

Node.js 从 **v12** 开始支持 ES Module，但需要满足以下条件之一：

1. 文件扩展名为 `.mjs`。
2. 在 `package.json` 中设置 `"type": "module"`。

#### **示例：**

```javascript
// math.mjs
export function add(a, b) {
    return a + b;
}

// main.mjs
import { add } from "./math.mjs";
console.log(add(1, 2)); // 3
```

------

### **3.2 总结**

- **CommonJS**：
  - 适用于 Node.js 环境。
  - 同步加载，适合服务器端开发。
  - 语法：`require` / `module.exports`。
- **ES Module**：
  - 适用于浏览器和现代 Node.js 环境。
  - 静态加载，适合现代 JavaScript 开发。
  - 语法：`import` / `export`。

## 4、CommonJS中module.exports 和 exports的区别

### 4.**1 `module.exports` 和 `exports` 的关系**

- **`module.exports`** 是模块系统的实际导出对象。
- **`exports`** 是 `module.exports` 的一个引用（可以理解为一个快捷方式）。

默认情况下，`exports` 和 `module.exports` 指向同一个对象：



```javascript
console.log(exports === module.exports); // true
```

------

### 4.**2 使用方式**

#### 4.**2.1 使用 `exports`**

- `exports` 是一个对象，你可以给它添加属性来导出多个值。

- 示例：

  ```javascript
  // math.js
  exports.add = function(a, b) {
      return a + b;
  };
  exports.subtract = function(a, b) {
      return a - b;
  };
  ```

  导入：

  ```javascript
  // main.js
  const math = require("./math.js");
  console.log(math.add(1, 2)); // 3
  console.log(math.subtract(5, 3)); // 2
  ```

#### **4.2.2 使用 `module.exports`**

- `module.exports` 可以直接赋值，用于导出一个单一的值（如函数、类、对象等）。

- 示例：

  

  ```javascript
  // math.js
  module.exports = function add(a, b) {
      return a + b;
  };
  ```

  导入：

  ```javascript
  // main.js
  const add = require("./math.js");
  console.log(add(1, 2)); // 3
  ```

------

### 4.**3 关键区别**

#### 4.**3.1 赋值行为**

- **`exports`**：只能通过添加属性的方式导出值（如 `exports.add = ...`），不能直接赋值。

  - 如果直接赋值，会断开 `exports` 和 `module.exports` 的引用关系，导致导出失败。

  - 错误示例：

    ```java
    // math.js
    exports = function add(a, b) {
        return a + b;
    };
    ```

    ```javascript
    // main.js
    const math = require("./math.js");
    console.log(math); // {} （空对象）
    ```

- **`module.exports`**：可以直接赋值，用于导出一个单一的值。

  - 示例：

    ```javascript
    // math.js
    module.exports = function add(a, b) {
        return a + b;
    };
    ```

    

    ```javascript
    // main.js
    const add = require("./math.js");
    console.log(add(1, 2)); // 3
    ```

#### 4.**3.2 导出结果**

- **`exports`**：导出一个对象，对象中包含添加的属性。
- **`module.exports`**：可以导出任意类型的值（函数、对象、类、基本类型等）。

------

### 4.**4 实际使用中的建议**

- **如果需要导出多个值**，使用 `exports`：

  ```javascript
  exports.add = function(a, b) {
      return a + b;
  };
  exports.subtract = function(a, b) {
      return a - b;
  };
  ```

- **如果需要导出一个单一的值**，使用 `module.exports`：

  ```javascript
  module.exports = function add(a, b) {
      return a + b;
  };
  ```

- **不要混用**：避免同时使用 `exports` 和 `module.exports`，否则可能会导致意外的导出结果。

------

### **4.5 示例对比**

#### 4.**5.1 使用 `exports`**

```javascript
// math.js
exports.add = function(a, b) {
    return a + b;
};
exports.subtract = function(a, b) {
    return a - b;
};

// main.js
const math = require("./math.js");
console.log(math.add(1, 2)); // 3
console.log(math.subtract(5, 3)); // 2
```

#### **4.5.2 使用 `module.exports`**

```javascript
// math.js
module.exports = function add(a, b) {
    return a + b;
};

// main.js
const add = require("./math.js");
console.log(add(1, 2)); // 3
```

#### **4.5.3 错误示例**

```javascript
// math.js
exports = function add(a, b) {
    return a + b;
};

// main.js
const math = require("./math.js");
console.log(math); // {} （空对象）
```

------

### 4.**6 总结**

| **特性**     | `exports`                         | `module.exports`             |
| :----------- | :-------------------------------- | :--------------------------- |
| **类型**     | 对象（默认指向 `module.exports`） | 任意类型（函数、对象、类等） |
| **赋值行为** | 只能添加属性，不能直接赋值        | 可以直接赋值                 |
| **导出结果** | 导出一个对象                      | 导出任意类型的值             |
| **适用场景** | 导出多个值                        | 导出一个单一的值             |

- **`exports`** 是 `module.exports` 的快捷方式，适合导出多个值。
- **`module.exports`** 是实际的导出对象，适合导出一个单一的值。
- 不要直接对 `exports` 赋值，否则会断开它与 `module.exports` 的引用关系。

## **5. JavaScript 的引用机制**

在 JavaScript 中，变量可以分为两种类型：

1. **基本类型**（如 `number`、`string`、`boolean` 等）：直接存储值。
2. **引用类型**（如 `object`、`array`、`function` 等）：存储的是内存地址（引用）。

当你对一个引用类型的变量赋值时，实际上是修改了它的引用（内存地址），而不是修改它指向的对象本身。

------

### **示例：引用类型的行为**

```javascript
let obj1 = { name: "Alice" };
let obj2 = obj1; // obj2 和 obj1 指向同一个对象

obj2.name = "Bob";
console.log(obj1.name); // "Bob" （obj1 也被修改了）

obj2 = { name: "Charlie" }; // obj2 指向一个新的对象
console.log(obj1.name); // "Bob" （obj1 没有被修改）
```

在这个例子中：

- `obj2 = obj1` 让 `obj2` 和 `obj1` 指向同一个对象。
- 修改 `obj2.name` 会影响到 `obj1`，因为它们指向同一个对象。
- 但 `obj2 = { name: "Charlie" }` 让 `obj2` 指向一个新的对象，而 `obj1` 仍然指向原来的对象。

------

## **6. CommonJS 模块系统的设计**

在 CommonJS 中，每个模块都有一个 `module` 对象，`module.exports` 是这个模块的实际导出对象。`exports` 是 `module.exports` 的一个引用（快捷方式）。

### **初始状态**



```javascript
// 模块初始化时
module.exports = {};
exports = module.exports; // exports 是 module.exports 的引用
```

此时，`exports` 和 `module.exports` 指向同一个空对象。

------

### **6.1 使用 `exports` 添加属性**

当你通过 `exports` 添加属性时，实际上是在修改 `module.exports` 指向的对象：

```javascript
exports.add = function(a, b) {
    return a + b;
};
```

等价于：

```javascript
module.exports.add = function(a, b) {
    return a + b;
};
```

此时，`exports` 和 `module.exports` 仍然指向同一个对象。

------

### **6.2 直接对 `exports` 赋值**

如果你直接对 `exports` 赋值，会断开 `exports` 和 `module.exports` 的引用关系：



```javascript
exports = function add(a, b) {
    return a + b;
};
```

此时：

- `exports` 指向一个新的函数。
- `module.exports` 仍然指向原来的空对象。

因此，模块导出的仍然是 `module.exports`（空对象），而不是 `exports` 指向的函数。

------

### **6.3 直接对 `module.exports` 赋值**

如果你直接对 `module.exports` 赋值，会修改模块的实际导出对象：



```javascript
module.exports = function add(a, b) {
    return a + b;
};
```

此时：

- `module.exports` 指向一个新的函数。
- `exports` 仍然指向原来的空对象（但已经不重要了，因为模块导出的是 `module.exports`）。

------

## **7. 这样设计的目的是什么？**

### **7.1 提供灵活性**

- **`exports`**：提供了一个快捷方式，方便开发者通过添加属性的方式导出多个值。
- **`module.exports`**：提供了直接导出一个单一值的能力（如函数、类、对象等）。

这种设计让开发者可以根据需要选择不同的导出方式。

------

### **7.2 保持一致性**

- 通过 `exports` 添加属性的方式，可以保持模块导出的对象结构一致。
- 通过 `module.exports` 直接赋值的方式，可以灵活地导出任意类型的值。

------

### **7.3 避免混淆**

- 如果 `exports` 和 `module.exports` 的行为完全一致，可能会导致开发者混淆它们的用途。
- 通过限制 `exports` 的行为（只能添加属性，不能直接赋值），可以更清晰地表达模块的导出意图。

------

## 7.**4 总结**

| **行为**     | `exports`                          | `module.exports`                 |
| :----------- | :--------------------------------- | :------------------------------- |
| **初始值**   | 指向 `module.exports` 的引用       | 空对象 `{}`                      |
| **添加属性** | 修改 `module.exports` 指向的对象   | 修改 `module.exports` 指向的对象 |
| **直接赋值** | 断开与 `module.exports` 的引用关系 | 修改模块的实际导出对象           |
| **设计目的** | 提供快捷方式，方便导出多个值       | 提供灵活性，支持导出任意类型的值 |

### **关键点**

- `exports` 是 `module.exports` 的引用，用于方便地添加属性。
- 直接对 `exports` 赋值会断开它与 `module.exports` 的引用关系。
- 直接对 `module.exports` 赋值会修改模块的实际导出对象。

## 8、declare module在commonJS的用法

### **8.1 `export =` 对应 CommonJS 的 `module.exports`**

### **CommonJS 中的 `module.exports`**

在 CommonJS 中，`module.exports` 用于导出一个单一的值（如函数、类、对象等）。例如：



```
// my-commonjs-module.js
function myFunction() {
    console.log("Hello from CommonJS!");
}

module.exports = myFunction; // 导出一个函数
```

### **TypeScript 中的 `export =`**

在 TypeScript 中，`export =` 用于描述 CommonJS 模块导出一个单一的值。例如：

```
// my-commonjs-module.d.ts
declare module "my-commonjs-module" {
    function myFunction(): void;
    export = myFunction; // 表示模块导出一个函数
}
```

#### **对应关系**

- `module.exports = myFunction`（CommonJS）对应 `export = myFunction`（TypeScript）。
- 这种写法表示模块导出一个单一的值。

#### **导入方式**

typescript

Copy

```javascript
import myFunction from "my-commonjs-module";

myFunction(); // 正确
```

------

### 8**.2 `export const` 或 `export function` 对应 CommonJS 的 `exports`**

### **CommonJS 中的 `exports`**

在 CommonJS 中，`exports` 用于导出一个对象，对象中可以包含多个属性（如函数、变量等）。例如：

```javascript
// my-commonjs-module.js
function myFunction() {
    console.log("Hello from CommonJS!");
}

const myVariable = "Hello!";

exports.myFunction = myFunction; // 导出一个函数
exports.myVariable = myVariable; // 导出一个变量
```

或者：

```javascript
// my-commonjs-module.js
function myFunction() {
    console.log("Hello from CommonJS!");
}

const myVariable = "Hello!";

module.exports = {
    myFunction,
    myVariable,
};
```

### **TypeScript 中的 `export const` 或 `export function`**

在 TypeScript 中，`export const` 或 `export function` 用于描述 CommonJS 模块导出一个对象，对象中包含多个属性。例如：



```javascript
// my-commonjs-module.d.ts
declare module "my-commonjs-module" {
    export function myFunction(): void; // 导出一个函数
    export const myVariable: string;   // 导出一个变量
}
```

#### **对应关系**

- `exports.myFunction = myFunction` 或 `module.exports = { myFunction, myVariable }`（CommonJS）对应 `export function myFunction` 和 `export const myVariable`（TypeScript）。
- 这种写法表示模块导出一个对象，对象中包含多个属性。

#### **导入方式**

```javascript
import { myFunction, myVariable } from "my-commonjs-module";

myFunction(); // 正确
console.log(myVariable); // 正确
```

或者：



```javascript
import * as myModule from "my-commonjs-module";

myModule.myFunction(); // 正确
console.log(myModule.myVariable); // 正确
```

------

### 8.**3 总结对比**

| **CommonJS 导出方式**                                        | **TypeScript 声明方式**                                      | **导入方式**                                                 |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| `module.exports = myFunction`                                | `declare module "module" { function myFunction(): void; export = myFunction; }` | `import myFunction from "module";`                           |
| `exports.myFunction = ...` 或 `module.exports = { myFunction, myVariable }` | `declare module "module" { export function myFunction(): void; export const myVariable: string; }` | `import { myFunction, myVariable } from "module";` 或 `import * as myModule from "module";` |

### **关键点**

- **`export =`** 对应 CommonJS 的 `module.exports = ...`，表示导出一个单一的值。
- **`export const` 或 `export function`** 对应 CommonJS 的 `exports.myFunction = ...` 或 `module.exports = { ... }`，表示导出一个对象，对象中包含多个属性。

------

### 8.**4 最终总结**

- **`export =`**：用于描述 CommonJS 模块导出一个单一的值（如函数、类、对象等）。
- **`export const` 或 `export function`**：用于描述 CommonJS 模块导出一个对象，对象中包含多个属性。

通过这种对应关系，TypeScript 能够正确地理解 CommonJS 模块的导出方式，并确保类型检查和导入语法的正确性。