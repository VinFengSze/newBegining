### 如何实现真正的 1px 边框（解决移动端 Retina 屏显示过粗问题）

在移动端开发中，由于 Retina 等高分辨率屏幕的存在，CSS 的 `1px` 实际会渲染为 2px 甚至更粗。以下是几种主流解决方案：

#### 1. **使用 `transform: scale()` 缩放（推荐）**

```css
.border-1px {
  position: relative;
}
.border-1px::after {
  content: "";
  position: absolute;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 1px;
  background: #000;
  transform: scaleY(0.5);
  transform-origin: 0 0;
}
```

**优点**：兼容性好，实现简单
**缺点**：需要伪元素，可能影响布局

#### 2. **使用 `viewport` 动态缩放（推荐）**

通过 JavaScript 动态修改 viewport 的 initial-scale：



```js
const scale = 1 / window.devicePixelRatio;
document.querySelector('meta[name="viewport"]').content = `width=device-width, initial-scale=${scale}, maximum-scale=${scale}, minimum-scale=${scale}`;
```

**优点**：全局生效，无需修改 CSS
**缺点**：会影响页面布局，需要重新计算 rem/px

#### 3. **使用 `border-image`**



```css
.border-1px {
  border-bottom: 1px solid transparent;
  border-image: url('data:image/png;base64,...') 2 2 stretch;
}
```

**优点**：可实现复杂边框
**缺点**：不灵活，修改麻烦

#### 4. **使用 `box-shadow` 模拟**

```css
.border-1px {
  box-shadow: 0 0.5px 0 0 #000;
}
```

**优点**：代码简单
**缺点**：颜色和位置控制不精确

#### 5. **使用 SVG 方案**

```css
.border-1px {
  background: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="1"><rect width="100%" height="0.5" fill="black"/></svg>') repeat-x bottom;
}
```

**优点**：矢量清晰
**缺点**：兼容性稍差

#### 6. **使用 `postcss-write-svg`（构建工具方案）**

通过 PostCSS 插件自动生成 SVG 边框：



```css
@svg 1px-border {
  height: 1px;
  @rect {
    fill: var(--color, black);
    width: 100%;
    height: 50%;
  }
}
.border-1px {
  border-bottom: 1px solid transparent;
  border-image: svg(1px-border param(--color #000)) 2 2 stretch;
}
```

#### 最佳实践建议：

1. **普通项目**：使用 `transform: scale()` 方案（方案1）
2. **需要精确控制**：使用 viewport 缩放（方案2）
3. **复杂边框**：使用 SVG 方案（方案5）

这些方案都能有效解决 Retina 屏下 1px 显示过粗的问题，开发者可根据项目需求选择最适合的方案。