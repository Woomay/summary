## reduce用法总结

### 基础语法

```js
array.reduce(function(total,currentValue,currentIndex,arr),initialValue)
// total: 必需，初始值。或者计算结束后返回值。
// currentValue: 必需，当前的元素。
// currentIndex: 可选，当前元素的索引。
// arr: 可选。当前元素所属的数组对象。
// initialValue: 可选。传递给函数的初始值，相当于total的初始值。
```

### 常见用法

* 数组求和

  ```js
  const arr = [1,2,3,4,5];
  var arr1 = arr.reduce((total,num) => {return total + num},0) // -----15,initialValue默认为0，可以设定初始值
  <!--对象数组求和-->
  var res = [
      {name: 'jhon',age: 18},
      {name: 'pipi',age: 24},
      {name: 'apple',age: 25}
  ]
  const sum = res.reduce((prev,cur) => prev+cur.age,0) //------67
  const sum1 = res.reduce((prev,cur) => prev + cur.age,-7) //------60
  ```

* 数组最大值

  ```js
  const arr = [11,14,8,22];
  const maxNum = arr.reduce((pre,cur) => {return pre>cur ? pre : cur}) //-----22
  ```

### 进阶用法

* 数组对象中的用法

  ```js
  //比如生成"皮皮、小帅比和大帅比"
  const objArr = [{name: '皮皮'},{name: '小帅比'},{name: '大帅比'}];
  const res = objArr.reduce((pre,cur,index,arr) => {
      if (index === 0) {
          return `${cur.name}`;
  	} else if (index === (arr.length -1)) {
          return `${pre}和${cur.name}`
  	} else {
          return `${pre}、${cur.name}`
  	}
  },'')
  ```

* 数组转数组

  ```js
  //按照一定的规则转换数组
  var a = [1,2,3,4];
  var newA = a.reduce((prev,cur) => {
      prev.push(cur * cur);
      return prev;
  },[]) //-----[1,4,9,16]
  ```

* 求字符串中字母出现的次数

  ```js
  const str = 'sdfffddsfsfsgghht';
  const res = str.split('').reduce((prev,cur) => {
      prev[cur] ? prev[cur]++ : prev[cur] = 1;
      return prev
  },{}) //-----{d:3,f:5,g:2,h:2,s:4,t:1}
  ```

* 数组转对象

  ```js
  var objArr = [{name: '皮皮',id: 1},{name: '皮酱子',id:2}];
  var obj = objArr.reduce((prev,cur) => {
      prev[cur.id] = cur;
      return prev;
  },{}) //-----{{name: '皮皮'，id:1},{name:'皮酱子'，id: 2}}
  ```

### 高级用法

* 多维叠加执行操作

  ```js
  <!-- 各科成绩占比重不一样， 求结果 -->
  var result = [
    { subject: 'math', score: 88 },
    { subject: 'chinese', score: 95 },
    { subject: 'english', score: 80 }
  ];
  var dis = {
      math: 0.5,
      chinese: 0.3,
      english: 0.2
  };
  var res = result.reduce((prev, cur) => dis[cur.subject] * cur.score + prev, 0);
  
  <!-- 加大难度， 商品对应不同国家汇率不同，求总价格 -->
  var prices = [{price: 23}, {price: 45}, {price: 56}];
  var rates = {
    us: '6.5',
    eu: '7.5',
  };
  var initialState = {usTotal:0, euTotal: 0};
  var res = prices.reduce((prev1, cur1) => Object.keys(rates).reduce((prev2, cur2) => {
    console.log(prev1, cur1, prev2, cur2);
    prev1[`${cur2}Total`] += cur1.price * rates[cur2];
    return prev1;
  }, {}), initialState);
  
  var manageReducers = function() {
    return function(state, item) {
      return Object.keys(rates).reduce((nextState, key) => {
          state[`${key}Total`] += item.price * rates[key];
          return state;
        }, {});
    }
  };
  var res1= prices.reduce(manageReducers(), initialState);
  ```

* 对象数组去重

  ```js
  const hash = {};
    chatlists = chatlists.reduce((obj, next: Object) => {
      const hashId = `${next.topic}_${next.stream_id}`;
      if (!hash[hashId]) {
        hash[`${next.topic}_${next.stream_id}`] = true;
        obj.push(next);
      }
      return obj;
    }, []);
  ```

* 扁平一个多维数组

  ```js
  var arr = [[1,2,3],[4,5,6],[7,8,9]]
  var res = arr.reduce((x,y)=>x.concat(y),[])
  ```

* 一维数组变二维

  ```js
  const data = [
      { title: '个人_增加', value: '1112' },
      { title: '个人_删除', value: '1112' },
      { title: '我_增加', value: '1112' },
      { title: '我_删除', value: '1112' }
  ]
  //数据如上，怎么把每一项title里_前面的数据当初数组的title。如果前一半字符相同，就往children数组里添加一项.
  const test = data.reduce((array, item) => {
      const title = item.title.split('_')[0]
      const action = item.title.split('_')[1]
      let index = array.findIndex(n => n.title === title)
      if (index > -1) {
          array[index].children.push({ action, value: item.value })
          return array
      } else {
          return [...array, { title, children: [{ action, value: item.value }] }]
      }
  }, [])
  //test
  const data = [
      {
          title: '个人',
          children: [
              { action: '增加', value: '1112' },
              { action: '删除', value: '1112' }
          ]
      },
      {
          title: '我',
          children: [
              { action: '增加', value: '1112' },
              { action: '删除', value: '1112' }
          ]
      },
  ]
  ```
