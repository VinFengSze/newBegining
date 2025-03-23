### **Vue2 与 Vue3 生命周期对比表**

| Vue2 生命周期钩子         | Vue3 生命周期钩子（组合式API） | Vue3 生命周期钩子（选项式API） | 说明                                                         |
| :------------------------ | :----------------------------- | :----------------------------- | :----------------------------------------------------------- |
| `beforeCreate`            | **无**（由 `setup()` 替代）    | `beforeCreate`（保留但少用）   | 组件实例初始化前，组合式API中代码直接写在 `setup()` 开头。   |
| `created`                 | **无**（由 `setup()` 替代）    | `created`（保留但少用）        | 组件实例初始化后，组合式API中代码直接写在 `setup()` 中。     |
| `beforeMount`             | `onBeforeMount`                | `beforeMount`                  | 组件挂载到DOM前触发。                                        |
| `mounted`                 | `onMounted`                    | `mounted`                      | 组件挂载到DOM后触发。                                        |
| `beforeUpdate`            | `onBeforeUpdate`               | `beforeUpdate`                 | 数据变化导致DOM更新前触发。                                  |
| `updated`                 | `onUpdated`                    | `updated`                      | 数据变化导致DOM更新后触发。                                  |
| `beforeDestroy`           | `onBeforeUnmount`              | `beforeUnmount`（推荐新名称）  | 组件卸载前触发，Vue3选项式API支持旧名但建议改用新名。        |
| `destroyed`               | `onUnmounted`                  | `unmounted`（推荐新名称）      | 组件卸载后触发，Vue3选项式API支持旧名但建议改用新名。        |
| `activated`               | `onActivated`                  | `activated`                    | 被 `<keep-alive>` 缓存的组件激活时触发。                     |
| `deactivated`             | `onDeactivated`                | `deactivated`                  | 被 `<keep-alive>` 缓存的组件停用时触发。                     |
| `errorCaptured`           | `onErrorCaptured`              | `errorCaptured`                | 捕获子孙组件错误时触发。                                     |
| **无**                    | `onRenderTracked`（调试用）    | **无**                         | **Vue3新增**，用于跟踪渲染依赖（仅在开发模式下生效）。       |
| **无**                    | `onRenderTriggered`（调试用）  | **无**                         | **Vue3新增**，用于追踪触发渲染的依赖（仅在开发模式下生效）。 |
| `serverPrefetch`（SSR用） | `onServerPrefetch`（SSR用）    | `serverPrefetch`               | 服务器端渲染时预取数据（需手动处理Promise）。                |

------

### **核心变化说明**

1. **`beforeCreate` 和 `created`**

   - **Vue3组合式API**：被 `setup()` 函数替代，直接在此函数内编写初始化逻辑（无需显式钩子）。
   - **Vue3选项式API**：仍支持这两个钩子，但推荐将代码迁移到 `setup()` 中。

2. **销毁钩子重命名**

   - `beforeDestroy` → `onBeforeUnmount`（组合式API）或 `beforeUnmount`（选项式API）。
   - `destroyed` → `onUnmounted`（组合式API）或 `unmounted`（选项式API）。

3. **新增调试钩子**

   - `onRenderTracked` 和 `onRenderTriggered`：用于追踪渲染依赖和触发更新的依赖（开发模式专用）。

4. **组合式API的使用方式**
   需从 `vue` 中导入钩子函数，并在 `setup()` 中调用：

   javascript

   Copy

   ```
   import { onMounted, onUnmounted } from 'vue';
   
   export default {
     setup() {
       onMounted(() => {
         console.log('组件已挂载');
       });
       onUnmounted(() => {
         console.log('组件已卸载');
       });
     }
   };
   ```

5. **执行顺序与行为**

   - 生命周期顺序与Vue2一致，但组合式API的 `setup()` 在 `beforeCreate` 和 `created` 之间执行。
   - **选项式API**中仍可使用旧钩子名（如 `beforeDestroy`），但Vue3推荐使用新名称。

------

### **使用建议**

- **组合式API**：优先使用 `setup()` 替代 `beforeCreate`/`created`，并通过 `onXxx` 函数注册生命周期。
- **选项式API**：可沿用Vue2写法，但建议逐步迁移到新名称（如 `beforeUnmount` 替代 `beforeDestroy`）。
- **调试钩子**：仅在需要分析渲染性能时使用 `onRenderTracked` 和 `onRenderTriggered`。

------

通过对比，Vue3的生命周期更强调语义化（如“卸载”代替“销毁”）和组合式API的灵活性，同时保持对旧选项式API的兼容。