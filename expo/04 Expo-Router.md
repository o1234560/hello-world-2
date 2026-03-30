# Expo Router

## 1. 介绍

Expo Router 是一个用于 React Native 和网页应用的基于文件的路由。它允许你管理应用中屏幕之间的导航，使用户能够在应用的不同部分之间无缝切换，并在多个平台（Android、iOS 和网页）上使用相同的组件。

Expo Router 将来自网络的最佳文件系统路由概念带到通用应用中 —— 允许你的路由在每个平台上都能工作。当文件被添加到 app 目录时，该文件会自动成为你导航中的一个路由。

**当使用`npx create-expo-app@latest`命令创建项目时，回自动安装expo-router，直接在项目中使用即可。**

## 2. 手动安装

在expo项目中，手动添加exp-router。

### 2.1 安装依赖

```powershell
npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar
```

### 2.2 设置入口点

对于属性 `main`，在 package.json 中使用 `expo-router/entry` 作为其值。初始客户端文件是 https://expo.nodejs.cn/router/reference/src-directory/（如果不使用 src 目录，则为 [app/_layout.tsx](https://expo.nodejs.cn/router/basics/layout/#root-layout)）。

```json
# app.json 文件
{
  "main": "expo-router/entry"
}
```

### 2.3 修改项目配置

在你的应用配置中添加深度链接`scheme`并启用类型化路由：

```json
# app.json 文件
{
    "scheme":"your-app-scheme",
    "experiments":{
        "typeRoutes":true
    }
}
```

如果你正在为网页开发应用，请安装以下依赖：

```powershell
npx expo install react-native-web react-dom
```

然后将以下内容添加到应用配置中来启用Metro web支持：

```json
# app.json 文件
{
    "web":{
        "bundler":"metro"
    }
}
```

### 2.4 修改 babel.config.js

如果你的项目有一个 babel.config.js 文件，确保你将 `babel-preset-expo` 用作 `preset`。如果你不需要任何自定义的 Babel 配置，你可以完全删除该文件：

```js
// bable.config.js
module.exports = function(api) {
    api.cache(true);
    return {
        presets: ['babel-preset-expo'],
    };
};
```

### 2.5 配置路径别名

如果你正在使用 [`src` 目录](https://expo.nodejs.cn/router/reference/src-directory/)，请在你的 tsconfig.json 中添加路径别名，这样你就可以使用像 `@/components/button` 这样的简短导入路径，而不是相对路径：

```json
# tsconfig.json 文件
{
  "extends": "expo/tsconfig.base",
  "compilerOptions": {
    "strict": true,
    "paths": {
      "@/*": ["./src/*"]
    }
  },
  "include": ["**/*.ts", "**/*.tsx", ".expo/types/**/*.ts", "expo-env.d.ts"]
}
```

`@/*` 别名在上面的例子中映射到 src 目录。

### 2.6 清除 bundler 缓存

更新配置后，运行以下命令以清除打包器缓存：

```powershell
npx expo start --clear
```

## 3. 路由结构（文件结构）

expo-router，采用文件系统来实现路由。其中 **app** 文件夹中的`文件结构`即为`路由的结构`：

```
src
 ├─app
 │  ├─index.tsx
 │  ├─home.tsx
 │  ├─layout.tsx
 │  └─profile
 │      └─friends.tsx
 │ 
 ├─components
 │  ├─app-tabs.native.tsx
 │  ├─app-tabs.tsx
 │  ├─text-field.tsx
 │  └─toolbar.tsx
 │ 
 └─utils、hooks 等
```

- src/app/index.tsx 是初始路由，当你打开应用或导航到你的网络应用的根 URL 时，它将首先出现。
- src/app/home.tsx 是一个路径为 `/home` 的页面，所以你可以在浏览器中通过 `yourapp.com/home` 这样的 URL 访问它，或者在原生应用中通过 `yourapp://home` 访问它。
- src/app/_layout.tsx 是根布局。你之前可能放在 App.jsx 中的任何初始化代码都应该放在这里。
- src/app/profile/friends.tsx 是一个路由为 `/profile/friends` 的页面。
- src/components/app-tabs.native.tsx 和 src/components/app-tabs.tsx 是[特定平台](https://expo.nodejs.cn/router/advanced/platform-specific-modules/)的标签组件。.native.tsx 文件用于 Android 和 iOS，而 .tsx 文件用于网页。根布局导入这些文件以渲染标签导航器。
- src/components/text-field.tsx 和 src/components/toolbar.tsx 不在 src/app 目录中，因此它们不会被视为页面。它们不会有 URL，也不能成为导航操作的目标。然而，它们可以在 src/app 目录下的页面中作为组件使用。

## 4. 路由符号

### 4.1 简单符合/无符号

没有任何标记的常规文件和目录名称表示 *静态路由*。它们的 URL 会完全匹配文件树中的名称。

```
app
 ├─index.tsx
 ├─home.tsx
 ├─_layout.tsx
 └─profile
     └─friends.tsx
```

因此，位于 **profile** 文件夹中的 **friends.tsx** 的文件，其 **URL** 是 `/profile/firends`。

### 4.2 方括号

如果你在文件或目录名称中看到方括号，那你看到的是一个_动态路由_。路由的名称包含一个可以在渲染页面时使用的参数。该参数可以出现在目录名或文件名中。

```
app
 ├─index.tsx
 ├─[userName].tsx
 ├─_layout.tsx
 └─[productId]
     └─index.tsx
```

例如，一个名为 **[userName].tsx** 的文件可以匹配 `/evanbacon`、`/expo` 或其他用户名。然后，你可以在页面内使用 `useLocalSearchParams` 钩子访问该参数，用它来加载特定用户的数据。

### 4.3 圆括号

一个目录的名称被**圆括号**包围表示一个_路由组_。这些目录对于将路由分组在一起而不影响 URL 非常有用。

```
app
 ├─[userName].tsx
 ├─_layout.tsx
 └─(home)
     ├─settings.tsx
     └─index.tsx
```

例如，一个名为 **app/(home)/settings.tsx** 的文件，其 URL 将为 `/settings`，即使它不直接位于 **app** 目录中。

### 4.4 index.tsx 文件

就像在网页上一样，index.tsx 文件表示目录的默认路由。

```
app
 ├─[userName].tsx
 ├─_layout.tsx
 └─(home)
     ├─settings.tsx
     └─index.tsx
```

例如， **(home)/index.tsx** 文件，其 URL 将为 `/`, 实际上成为整个应用的默认路由。

### 4.5 _layout.tsx 文件

_layout.tsx 文件是特殊文件，它们本身不是页面，但用于定义目录内的一组路由如何相互关联。如果一个路由目录被安排为堆栈或选项卡形式，布局路由就是你通过使用堆栈导航器或选项卡导航器组件来定义这种关系的地方。

```
app
 ├─[userName].tsx
 ├─_layout.tsx
 └─(home)
     ├─settings.tsx
     └─index.tsx
```

布局路由会在其目录内的实际页面路由之前渲染。这意味着位于 **app** 目录中的 **_layout.tsx** 会在应用中的其他内容之前渲染。

### 4.6 加号

包含 `+` 的路由对 Expo Router 有特殊意义，并用于特定目的。

```
app
 ├─+not-found.tsx
 ├─+html.tsx
 ├─+middleware.tsx
 └─+native-intent.tsx
```

- [`+not-found`](https://expo.nodejs.cn/router/error-handling/#unmatched-routes)，用于捕获应用中不匹配任何路由的请求。
- [`+html`](https://expo.nodejs.cn/router/web/static-rendering/#root-html) 用于自定义应用在网页上使用的 HTML 样板代码。
- [`+native-intent`](https://expo.nodejs.cn/router/advanced/native-intent/) 用于处理指向应用的深度链接，这些链接不匹配特定的路由，例如由第三方服务生成的链接。
- [`+middleware`](https://expo.nodejs.cn/router/web/middleware/) 用于在路由渲染之前运行代码，允许你为每个请求执行诸如身份验证或重定向等任务。

<mark>警告：</mark> 一些路径名称，例如 `/assets`，是由 Metro 和 Expo Router 保留的。避免在路由中使用它们。完整列表请参见 [保留路径](https://expo.nodejs.cn/router/reference/reserved-paths/)。


