# vite 处理跨域

```js
export default defineConfig({
    server{ // 开发服务器中的配置
        proxy: { // 配置跨域解决方案
            "/api": {
                target: "https://www.baidu.com",
                changeOrigin: true,
                rewrite: (path) => path.replace(/^\/api/, '')
            }
        }
    }
})
```