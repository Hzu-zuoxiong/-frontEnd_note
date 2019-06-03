# `Vue-cli 3.0` 创建项目

## 安装

### 卸载旧版本

若已安装旧版本的 vue-cli(1.x 或 2.x)，需要先卸载

```
npm uninstall vue-cli -g
```

### Node 版本要求

vue-cli3.0 需要 Node.js 8.9 或更高的版本，可以通过命令行查询你的 Node 版本

```
node -v
```

### 安装 vue-cli3.0

```
npm install -g @vue/cli

// 查询vue-cli版本
vue --version
```

## 零配置启动/打包一个.vue 文件

### 安装扩展

```
npm install -g @vue/cli-service-global
```

安装扩展后，可以新建一个.vue 文件，然后启动：

```
// 启动服务
vue serve App.vue

// 打包出生产环境的包并用来部署
vue build App.vue
```

**只需要一个.vue 文件，就能迅速启动一个服务，服务支持 ES6 语法和热更新**

在服务启动的时候，会生成一个 node_modules 包，打包的时候会生成一个 dist 文件夹

![](/assets/Vue/initProject/build.png)

## 创建一个项目

### 1. 命令行创建：

```
vue create hello-world
```

hello-world 是文件夹名，如果不存在该文件夹则会自动创建

### 2. 选择模板：

- 一开始只有两个选项：default(默认配置)和 Manually select features(手动配置)。默认配置只有 babel 和 eslint，其他的都需要自己另外配置。
- 在每次选择手动配置后，会询问是否保存配置，即 koro 选项，以后再创建项目的时候只需选择原先的配置即可。

![](/assets/Vue/initProject/selectMode.png)

### 3. 选择配置：

根据项目需要选择配置，空格键是选中与取消，a 键是全选，i 键是反向选择

```
( ) TypeScript                                 // 支持使用 TypeScript 书写源码
( ) Progressive Web App (PWA) Support          // PWA 支持
( ) Router                                     // 支持 vue-router
( ) Vuex                                       // 支持 vuex
( ) CSS Pre-processors                         // 支持 CSS 预处理器。
( ) Linter / Formatter                         // 支持代码风格检查和格式化。
( ) Unit Testing                               // 支持单元测试。
( ) E2E Testing
```

### 4. 选择 CSS 预处理器

如果选择了 CSS 预处理器选择，则会选择这个：

```
// 选择CSS预处理器（默认支持PostCSS，Autoprefixer和CSS模块）：
> SCSS/SASS
  LESS
  Stylus
```

### 5. 是否使用路由的 history 模式

- 建议选 no,这样打包出来丢到服务器可以直接使用，
- 选 yes 的话需要服务器再配置

### 6. 选择 Eslint 代码验证规则：

```
> ESLint with error prevention only
  ESLint + Airbnb config
  ESLint + Standard config
  ESLint + Prettier
```

### 7. 选择什么时候进行代码规则检测：

建议选保存就检测

```
>( ) Lint on save // 保存就检测
 ( ) Lint and fix on commit // fix和commit时候检查
```

### 8. 选择 E2E 测试：

```
❯ Cypress (Chrome only)
  Nightwatch (Selenium-based)
```

### 9. 把 babel, postcss, eslint 这些配置文件放哪

通常选择独立位置，让 package.json 干净些

```
> In dedicated config files // 独立文件放置
  In package.json // 放package.json里
```

### 10. 是否保存配置

```
Save this as a preset for future projects? (Y/n)
Save preset as:  name // 然后你下次进入配置可以直接使用你这次的配置了
```

### 11. 新建项目目录

与 vue-cli 2.x 相比：

- 3.0 的 webpack 配置的目录不见了，没有了 build 和 config 这两个文件夹。
- static 文件夹改成 public 文件夹
- router 文件夹变成单个文件

![](/assets/Vue/initProject/directory.png)

若需要自定义 webpack 配置，需要在根目录下新建一个 vue.config.js 文件，详细配置看[官方文档](https://cli.vuejs.org/zh/config/#%E5%85%A8%E5%B1%80-cli-%E9%85%8D%E7%BD%AE)

```
// vue.config.js
module.exports = {
    // 配置
}
```

### 12. 启动项目

```
npm run serve  // 与之前的npm run dev不同
```

打开http://localhost:8080

![](/assets/Vue/initProject/initProject.PNG)

## 13. 使用图形化界面创建/管理/运行项目

启动图形化界面

```
vue ui
```

具体步骤就不赘述了。图形化界面很容易操作
