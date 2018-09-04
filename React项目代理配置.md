## React项目代理配置

* 使用`create-react-app` 创建的项目，在`package.json`文件中进行如下配置：

  ```jsx
  "proxy": {
      "/api/*": {
          "target": "http://www.example.com",
        	"changeOrigin": true
      }
  }
  ```

  表示`api`下面的所有请求都将转发到`http://www.example.com`下面.

* 使用`http-proxy-middleware` 解决API资源请求跨域问题，`http-proxy-middleware` 一般不需要自行安装，在安装webpack的过程中，会自动依赖安装到`node_modules`文件夹下，如果没有，那就请自行安装：

  ```js
  npm install --save-dev http-proxy-middleware
  ```

   React开发分为以下两种情况：

  1. 前端部署了nodejs服务器，才用app.listen()启动前端服务器，那么只需要在js文件中添加以下代码即可（假如你的前端服务器js文件叫做server.js）：

     ```jsx
     //导入proxy
     var proxy = require('http-proxy-middleware')
     
     //context可以是单个字符串，也可以是多个字符串数组，分别对应你需要代理的api，星号（*）表示匹配当前路径下面所有的api
     const context = [`/login`,`/admin/*`]
     
     //options可选的配置参数可自行查看readme.md文档，通常只需要配置target，也就是api所属域名。
     const options = {
         target: 'http://www.example.com',
         changeOrigin: true
     }
     
     //讲options对象用proxy封装起来，作为参数传递
     const apiProxy = proxy(options)
     
     //现在只需要执行这一行代码，当需要跨域访问api资源时候，便可以成功访问；
     app.use(context,apiProxy)
     ```

  2. 前端没有node服务器，但是用webpack的devServer来启动前端项目。

     在`webpack.config.js` 里面添加proxy配置：

     ```jsx
     var proxy = require('http-proxy-middleware')
     
     const context = [`/login`,`/admin/*`]
     
     module.exports = {
         devServer: {
             host: 'localhost',
             port: '3000',
             proxy: [
                 {
                     context: context,
                     target: 'http://www.example.com',
                     changeOrigin: true,
                     secure: false
                 }
             ]
         }
     }
     ```

     

