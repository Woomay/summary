## 携带cookie进行数据请求

前端进行数据请求有：普通的ajax(json)请求，jsop跨域请求，cors跨域请求，fetch请求...**PC端**这些请求方式中，普通的ajax(json)请求和jsop跨域请求是默认携带cookie的，而cors跨域请求和fetch请求默认是不携带cookie的。因此，当我们的请求需要携带cookie时，我们就要对cors跨域请求和fetch请求这两中请求方式进行特殊配置处理。对于做移动端的童鞋来说，要是能把项目运行在PC端中最好不过，对于调试过程中的BUG一目了然，所以做特殊处理后更有利于我们在PC端进行调试。

- fetch请求方式： `在请求头中配置：credentials: 'include'`

  ```ruby
  fetch(url,{
      method: 'POST',
      headers: {
      	"Content-Type": "application/x-www-form-urlencoded"    
      },
      credentials: 'include',
      body
  })
  	.then(res => return res.json();)
  	.then(data => { //----请求成功 })
  	.catch(e => { //----报错 })
  ```

- cors跨域请求：`在请求头中配置：xhrFields:{ withCredentials: true },crossDomain: true`

  ```ruby
  $.ajax({
      type: 'post',
      url: url,
      dataType: 'json',
      xhrFields:{
      	withCredentials: true    
      },
      crossDomain: true,
      data: {},
      success: function(data) {
          //--------
      },
      error: function(data) {
          //--------
  	}
  })
  ```

  ​