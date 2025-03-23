## 一、项目初始化

```ts
pnpm init // 初始化package.json
pnpm install vite -D // 安装vite
```

### 1、package.json

```ts
 "scripts": {
    "dev": "vite",
    "build": "vite build"
  },
```

#### 1.1 支持.vue结尾文件

```ts
pnpm add @vitejs/plugin-vue -D
```

新建vite.config.ts

```ts
import { defineConfig, UserConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

// vite 默认只会编译ts,不会检测ts
export default defineConfig(() => {
  const config: UserConfig = {
    plugins: [
        vue(),
    ],
  }
  return config
});
```

#### 1.2 vite 默认只会编译ts,不会检测ts

```ts
pnpm add typescript vue-tsc -D
```

新建tsconfig.json

#### 1.3 eslint支持

```ts
npx eslint --init
```

#### 1.4 prettier支持

```ts
pnpm add prettier eslint-plugin-prettier @vue/eslint-config-prettier -D
```

