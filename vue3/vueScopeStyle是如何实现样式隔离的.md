在 Vue 中，**Scoped Styles** 通过**自动生成并添加唯一的属性选择器**，实现**样式隔离**。这种机制确保**组件的样式仅作用于当前组件的 DOM，而不会影响全局或其他组件**。

------

## **🌟 Scoped Styles 实现原理**

当你在 `.vue` 文件中使用 `scoped` 关键字时：

```vue

<style scoped>
  .title {
    color: red;
  }
</style>

<template>
  <h1 class="title">Hello Vue!</h1>
</template>
```

Vue **不会直接生成 `.title { color: red; }`**，而是会**给组件的 HTML 元素和 CSS 规则加上一个特定的 `data-v-xxxx` 属性选择器**。

### **🔹 编译后的 HTML**

Vue **会给组件根元素及其子元素自动添加一个 `data-v-xxxx`（哈希值）属性**：

```ts

<h1 class="title" data-v-abcd1234> Hello Vue! </h1>
```

### **🔹 编译后的 CSS**

Vue **会将 `.title` 选择器编译成 `属性选择器` 的形式**：

```ts

.title[data-v-abcd1234] {
  color: red;
}
```

这样，样式就**只会作用在当前组件的 HTML 结构中，避免影响全局样式**。

------

## **🎯 Scoped Styles 的两种实现方式**

Vue 支持两种 `scoped` 作用方式：

### **方式 1️⃣：Attribute 选择器（默认模式）**

Vue 默认使用 **`[data-v-xxxx]` 属性选择器** 进行样式作用域限制。

#### **示例**

```vue

<style scoped>
  h1 {
    color: red;
  }
</style>
```

🔽 **编译后**

```

h1[data-v-abcd1234] {
  color: red;
}
```

- **优点**：简单高效，兼容性好。
- **缺点**：如果 CSS 规则很复杂，可能需要额外处理 `:deep()`、`:global()` 规则。

------

### **方式 2️⃣：`:slotted`（针对 `<slot>` 内容）**

如果一个组件内部使用了 `<slot>` 插槽，Scoped Styles 默认**不会作用于插槽内容**。

#### **问题**

```vue

<ParentComponent>
  <ChildComponent>
    <h2> 插槽内容 </h2>
  </ChildComponent>
</ParentComponent>
```

如果 `ChildComponent` 里有：

```vue

<style scoped>
  h2 {
    color: blue;
  }
</style>
```

但 `<h2>` 是 **插槽内容**，它属于 `ParentComponent`，所以它**不会被 `data-v-xxxx` 作用**，不会变蓝。

#### **解决方案**

使用 `::v-slotted`（Vue 3） 或 `::slotted`（Vue 2）：

```vue

<style scoped>
  ::v-slotted(h2) {
    color: blue;
  }
</style>
```

🔽 **编译后**

```vue

h2[data-v-abcd1234]::slotted {
  color: blue;
}
```

这样，Vue **就能让 Scoped Styles 作用于插槽内容**。

------

## **🔥 Scoped Styles 进阶用法**

### **1️⃣ 深度选择器 `:deep()`**

`scoped` 只会影响当前组件的直接 DOM，**子组件内部的元素不会被影响**。

#### **问题**

```vue

<style scoped>
  .wrapper div {
    color: green;
  }
</style>

<template>
  <div class="wrapper">
    <ChildComponent />
  </div>
</template>
```

🔽 **编译后**

```vue

.wrapper div[data-v-abcd1234] {
  color: green;
}
```

但是 `ChildComponent` 里的 `<div>` 没有 `data-v-abcd1234`，所以 **样式不会生效**。

#### **解决方案**

```vue

<style scoped>
  :deep(.wrapper div) {
    color: green;
  }
</style>
```

🔽 **编译后**

```vue

.wrapper div {
  color: green;
}
```

- **`deep()` 让 CSS 选择器作用到所有后代组件，而不会被 `data-v-xxxx` 限制。**
- **Vue 3 支持 `:deep()`，Vue 2 需要 `::v-deep()`。**

------

### **2️⃣ `:global()`（全局样式）**

有时候，我们希望在 `scoped` 样式中定义**全局可用的 CSS**。

#### **示例**

```vue

<style scoped>
  :global(.global-class) {
    font-size: 20px;
  }
</style>
```

🔽 **编译后**

```vue

.global-class {
  font-size: 20px;
}
```

- **`global()` 让样式直接应用于整个页面，而不会被 `scoped` 限制。**

------

## **🎯 总结**

| 功能              | 作用                                              |
| ----------------- | ------------------------------------------------- |
| **Scoped Styles** | 通过 `data-v-xxxx` 让样式**仅作用于当前组件**     |
| **默认模式**      | 使用 `属性选择器 [data-v-xxxx]` 限制 CSS 作用范围 |
| **`:deep()`**     | 作用于**子组件的内部元素**，突破 `scoped` 限制    |
| **`:slotted()`**  | 让 `scoped` 样式作用于 `<slot>` 插槽内容          |
| **`:global()`**   | 让 `scoped` 组件里的样式**作用于全局**            |

**Vue 通过 `scoped` 提供了灵活的样式隔离机制，让组件样式不会互相污染，但同时也提供了 `:deep()`、`:slotted()` 等方式来自由调整作用范围。** 🎨🚀