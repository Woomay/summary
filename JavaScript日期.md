## JavaScript日期

### Date对象

Date对象实例表示单个时间点。尽管被命名为Date，也处理时间。

### 初始化Date对象

使用`new Date()` 来初始化Date对象。

UNIX时间戳：表示自1970年1月1日（UTC）以来的毫秒数。

### 4种方式创建新的Date对象

* 不传参数，创建一个表示‘现在’的Date对象。

  ```json
  var now = new Date()
  ```

* 传递number，表示从格林威治标准时间1970年1月1日的00:00开始的毫秒数

  ```json
  const timestamp = 1530826365000
  var now = new Date(timestamp)
  ```

* 传递一个字符串，代表一个日期

  ```json
  new Date('2018/7/22')
  new Date('July 22, 2018')
  new Date('July 22, 2018 07:22:13')
  new Date('2018-07-22 07:22:13')
  new Date('2018-07-22T07:22:13')
  new Date('25 March 2018')
  new Date('25 Mar 2018')
  ```

* 传递一组参数，它们代表日期的不同部分

  ```json
  var now = new Date(2018,8,22)
  ```

### 指定时区

初始化日期时，您可以传递时区，因此日期不会被假定为UTC，然后转换为您当地的时区。

您可以通过以+ HOURS格式添加时区来指定时区，或者通过添加括在括号中的时区名称来指定时区：

```json
new Date('July 22,2018 08:22:13 +0700')
new Date('July 22,2018 08:22:13 (CET)')
```

### 日期转换和格式设置

给定Date对象，有很多方法将从该日期生成一个字符串： 

```json
const date = new Date('July 22, 2018 07:22:13')

date.toString() // "Sun Jul 22 2018 07:22:13 GMT+0200 (Central European Summer Time)"
date.toTimeString() //"07:22:13 GMT+0200 (Central European Summer Time)"
date.toUTCString() //"Sun, 22 Jul 2018 05:22:13 GMT"
date.toDateString() //"Sun Jul 22 2018"
date.toISOString() //"2018-07-22T05:22:13.000Z" (ISO 8601 format)
date.toLocaleString() //"22/07/2018, 07:22:13"
date.toLocaleTimeString()    //"07:22:13"
date.getTime() //1532236933000
```

### Date对象的getter方法

Date对象提供了急着检查值的方法。

```json
const date = new Date('July 22, 2018 07:22:13')

date.getDate() //22
date.getDay() //0 (0 means sunday, 1 means monday..)
date.getFullYear() //2018
date.getMonth() //6 (starts from 0)
date.getHours() //7
date.getMinutes() //22
date.getSeconds() //13
date.getMilliseconds() //0 (not specified)
date.getTime() //1532236933000
date.getTimezoneOffset() //-120 (will vary depending on where you are and when you check - this is CET during the summer). Returns the timezone difference expressed in minutes
```

### 获取当前时间戳

`Date.now()`

