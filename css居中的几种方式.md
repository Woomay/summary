## css居中的几种方式

* 水平居中的`margin:0 auto`

  ```css
  <style>
  	.box{
          width: 300px;
          height: 300px;
          border: 3px solid red;
  	}
      img{
          display: block;
          width: 100px;
          height: 100px;
          margin: 0 auto;
      }
  </style>
  <!--html-->
  <body>
  	<div class="box">
  		<img src="example.jpg" alt="example" />
  	</box>
  </body>
  
  ```

  

* 水平居中`text-align:center`

* 水平垂直居中（一）定位`position:`需要定位的元素的margin减去宽高的一半

  ```css
   <style>
          *{
              padding: 0;
              margin: 0;
          }
          .box{
              width: 300px;
              height: 300px;
              background:#e9dfc7; 
              border:1px solid red;
              position: relative;
          }
          img{
              width: 100px;
              height: 150px;
              position: absolute;
              top: 50%;
              left: 50%;
              margin-top: -75px;
              margin-left: -50px;
          }
      </style>
  <!--html -->
  <body>
      <div class="box" >
         <img src="example.jpg" alt="example" />
      </div>
  </body>
  ```

* 水平垂直居中（二）定位和`margin: auto` （不受宽高限制）

  ```html
   <style>
          *{
              padding: 0;
              margin: 0;
          }
          .box{
              width: 300px;
              height: 300px;
              background:#e9dfc7; 
              border:1px solid red;
              position: relative;
  
          }
          img{
              width: 100px;
              height: 100px;
              position: absolute;
              top: 0;
              left: 0;
              right: 0;
              bottom: 0;
              margin: auto;
          }
      </style>
  <!--html -->
  <body>
      <div class="box" >
          <img src="example.jpg" alt="example" />
      </div>
  </body>
  ```

* 水平垂直居中（三）绝对定位和`transfrom`

  ```html
  <style>
      *{
          padding: 0;
          margin: 0;
      }
      .box{
          width: 300px;
          height: 300px;
          background:#e9dfc7; 
          border:1px solid red;
          position: relative;
  
      }
      img{
          width: 100px;
          height: 100px;
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%,-50%);
      }
  </style>
  <!--html -->
  <body>
      <div class="box" >
          <img src="example.jpg" alt="example" />
      </div>
  </body>
  ```

* 水平垂直居中（四）`display: table-cell` (变成表格样式,在父元素上设置)

  ```html
  <style>
      .box{
              width: 300px;
              height: 300px;
              background:#e9dfc7; 
              border:1px solid red;
              display: table-cell;
              vertical-align: middle;
              text-align: center;
          }
          img{
              width: 100px;
              height: 150px;
              /*margin: 0 auto;*/  这个也行
          }
  </style>
  <!--html -->
  <body>
      <div class="box" >
          <img src="example.jpg" alt="example" />
      </div>
  </body>
  ```

* 水平垂直居中（五）`flexbox` 居中

  ```html
  <style>
      .box{
              width: 300px;
              height: 300px;
              background:#e9dfc7; 
              border:1px solid red;
              display: flex;
              justify-content: center;
              align-items:center;
          }
          img{
              width: 150px;
              height: 100px;
          }
  </style>
  <!--html -->
  <body>
      <div class="box" >
          <img src="example.jpg" alt="example" />
      </div>
  </body>
  ```

  