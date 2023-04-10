# ejs02文档

## 基本用法

### 方式一，根据字符串渲染

```js
let template = ejs.compile(str, options);
template(data);
// => 输出渲染后的 HTML 字符串
```

### 方式二，根据字符串渲染

```js
ejs.render(str, data, options);
// => 输出渲染后的 HTML 字符串
```

### 方式三，根据ejs文件渲染

```j
ejs.renderFile(filename, data, options, function(err, str){
    // str => 输出渲染后的 HTML 字符串
});
```

注意：此功能需要在node环境中才能正常使用。

## 参数

- `cache` 缓存编译后的函数，需要指定 `filename`
- `filename` 被 `cache` 参数用做键值，同时也用于 include 语句
- `context` 函数执行时的上下文环境
- `compileDebug` 当值为 `false` 时不编译调试语句
- `client` 返回独立的编译后的函数
- `delimiter` 放在角括号中的字符，用于标记标签的开与闭
- `debug` 将生成的函数体输出
- `_with` 是否使用 `with() {}` 结构。如果值为 `false`，所有局部数据将存储在 `locals` 对象上。
- `localsName` 如果不使用 `with` ，localsName 将作为存储局部变量的对象的名称。默认名称是 `locals`
- `rmWhitespace` 删除所有可安全删除的空白字符，包括开始与结尾处的空格。对于所有标签来说，它提供了一个更安全版本的 `-%>` 标签（在一行的中间并不会剔除标签后面的换行符)。
- `escape` 为 `<%=` 结构设置对应的转义（escape）函数。它被用于输出结果以及在生成的客户端函数中通过 `.toString()` 输出。(默认转义 XML)。
- `outputFunctionName` 设置为代表函数名的字符串（例如 `'echo'` 或 `'print'`）时，将输出脚本标签之间应该输出的内容。
- `async` 当值为 `true` 时，EJS 将使用异步函数进行渲染。（依赖于 JS 运行环境对 async/await 是否支持）

## 标签含义

- `<%` '脚本' 标签，用于流程控制，无输出。
- `<%_` 删除其前面的空格符
- `<%=` 输出数据到模板（输出是转义 HTML 标签）
- `<%-` 输出非转义的数据到模板
- `<%#` 注释标签，不执行、不输出内容
- `<%%` 输出字符串 '<%'
- `%>` 一般结束标签
- `-%>` 删除紧随其后的换行符
- `_%>` 将结束标签后面的空格符删除

## 包含（include）

通过 `include` 指令将相对于模板路径中的模板片段包含进来。(需要提供 'filename' 参数。) 例如，如果存在 "./views/users.ejs" 和 "./views/user/show.ejs" 两个模板文件，你可以通过 `<%- include('user/show'); %>` 代码包含后者。

你可能需要能够输出原始内容的标签 (`<%-`) 用于 include 指令，避免对输出的 HTML 代码做转义处理。

```markup
<ul>
  <% users.forEach(function(user){ %>
    <%- include('user/show', {user: user}); %>
  <% }); %>
</ul>
```

## 自定义分隔符

可针对单个模板或全局使用自定义分隔符：

```javascript
let ejs = require('ejs'),
    users = ['geddy', 'neil', 'alex'];

// 单个模板文件
ejs.render('<?= users.join(" | "); ?>', {users: users},
    {delimiter: '?'});
// => 'geddy | neil | alex'

// 全局
ejs.delimiter = '$';
ejs.render('<$= users.join(" | "); $>', {users: users});
// => 'geddy | neil | alex'
```

## 缓存

EJS 附带了一个基本的进程内缓存，用于缓在渲染模板过程中所生成的临时 JavaScript 函数。 通过 Node 的 `lru-cache` 库可以很容易地加入 LRU 缓存：

```javascript
let ejs = require('ejs'),
    LRU = require('lru-cache');
ejs.cache = LRU(100); // 具有 100 条内容限制的 LRU 缓存
```

如果要清除 EJS 缓存，调用 `ejs.clearCache` 即可。如果你正在使用的是 LRU 缓存并且需要设置不同的限额，则只需要将 `ejs.cache` 重置为 一个新的 LRU 实例即可。

## 自定义文件加载器

默认的文件加载器是 `fs.readFileSync`，如果你想要的自定义它, 设置`ejs.fileLoader` 即可。

```javascript
let ejs = require('ejs');
let myFileLoader = function (filePath) {
  return 'myFileLoader: ' + fs.readFileSync(filePath);
};

ejs.fileLoader = myFileLoad;
```

使用此功能，您可以在读取模板之前对其进行预处理。

## 布局（Layouts）

EJS 并未对块（blocks）提供专门的支持，但是可以通过 包含页眉和页脚来实现布局，如下所示：

```javascript
<%- include('header'); -%>
<h1>
  Title
</h1>
<p>
  My page
</p>
<%- include('footer'); -%>
```
