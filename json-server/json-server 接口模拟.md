# json-server 接口模拟

`json-server` 是一款小巧的接口模拟工具，一分钟内就能搭建一套 Restful 风格的 API，尤其适合前端接口测试使用。

只需指定一个 `json` 文件作为 `api` 的数据源即可。

**注意：** `json-server`分为 [v0](https://www.npmjs.com/package/json-server/v/0.17.4) 和 v1两个版本，建议使用 v0 版本（稳定版）。下述内容基于 v0 版本。

---

## 基本使用

### 环境依赖

安装 Node.js 环境。

### 操作步骤

1. 安装 `json-server`

```shell
npm install -g json-server
```

2. 创建一个`db.json`文件包含一些数据

```json
{
    "users": [
        {"id": 1, "name": "小明", "age": 20},
        {"id": 2, "name": "梨花", "age": 3}
    ],
    "curses": [
        {"id": 1001, "title":"高数"},
        {"id": 4003, "title":"英语", "课时": 24}
    ]
}
```

3. 启动`json-server`服务

```shell
# 快速创建
json-server db.json

# 配置参数
json-server db.json --watch --port 3000 
```

4. 获取数据
-  在浏览器访问`localhost:3000`，即可进入服务器，访问`db.json`中的所有数据。
  
  - 输入`localhost:3000/users`可以查看users中的所有的用户。
  
  - 输入`localhost:3000/users/1`可以查看users中id为1的用户。

- 在项目中使用 POST、PUT、PATCH 或 DELETE 请求，获取或修改`db.json`中的数据。

## json-server 参数

| 参数                   | 说明                         |                                                          |
| -------------------- | -------------------------- | -------------------------------------------------------- |
| --watch              | 开启监听，当 db.json改变时，会自动更新服务器 |                                                          |
| --port 3000          | 设置接口为3000                  |                                                          |
| --host 192.168.0.105 | 设置服务器的host为192.168.0.105   | 默认为localhost                                             |
| --cors               | 允许跨域                       |                                                          |
| --static ./public    | 指定静态资源目录为 public           | 可以通过地址192.168.0.105:3000/01.png，来获取 public 文件夹中的01.png文件 |


