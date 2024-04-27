# 手机号运营商归属地查询

## 开发环境准备

### 前端

`WebStorm` , `Vue3` , `Element-Plus` 

首先安装 `Vue` 

```bash
npm create vue@latest
```

注意选择“`Add TypeScript` ”时选择“`Yes`”

进入到项目目录

```bash
cd <proj-name>
npm install
```

然后安装 `Element-Plus` 

```bash
npm install element-plus --save
```

选择完整导入，在 `main.js` 中复制以下内容

```tsx
// main.ts
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from './App.vue'

const app = createApp(App)

app.use(ElementPlus)
app.mount('#app')
```

最后运行项目

```bash
npm run dev
```

### 后端

创建项目时 Starter 选择 `Spring Web`和 `Lombok` 

在`build.gradle`中添加以下依赖：

```groovy
implementation group: 'com.baidubce', name: 'api-explorer-sdk', version: '1.0.4.1'
implementation group: 'com.alibaba.fastjson2', name: 'fastjson2', version: '2.0.49'
```

## 运行环境

