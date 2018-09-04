##常用JS方法总结：

- `Object`

  1.`Object.assign()` 将所有可枚举属性的值从一个或多个源对象复制到目标对象。返回目标对象。

  ```ruby
  const obj1 = {a:1,b:2,c:3};
  const obj2 = Object.assign({},obj1);    //obj2 = {a:1,b:2,c:3}
  ```

  2.`Object.keys()` 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 循环遍历该对象时返回的顺序一致 （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）。

  ```ruby
  传入对象,返回属性名：
      var obj = {'a':'123','b':'345'};
      console.info(Object.keys(obj)); 	//['a','b']
  传入字符串,返回索引：
      var str = 'ab1234';
      console.info(Object.keys(obj));		//[0,1,2,3,4,5]
  传入构造函数，返回空数组或者属性名
  	function Pasta(name, age, gender) {
              this.name = name;
              this.age = age;
              this.gender = gender;
              this.toString = function () {
                      return (this.name + ", " + this.age + ", " + this.gender);
              }
      }

      console.log(Object.keys(Pasta)); //console: []

      var spaghetti = new Pasta("Tom", 20, "male");
      console.log(Object.keys(spaghetti)); //console: ["name", "age", "gender", "toString"]
  数组,返回索引：
  	var arr = ["a", "b", "c"];
      console.log(Object.keys(arr)); // console: ["0", "1", "2"]

  ```

  ​

- `Array`

- `Collection`

- 内置方法

