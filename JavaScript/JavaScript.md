# JavaScript

## JavaScript基础

### JavaScript变量声明

**let**

```js
//无法作用域提升
alert(a)  //报错
let a 
//声明范围为块作用域
if(a){
    let b = a //只在本块起作用 无法暴露到外部
}
//不可重复声明
let b = 1
let b = 2 //报错 
//声明不会成为window的属性
alert(window.b) // undefined
```

**var**

```js
//具有声明提升  
alert(a)  //undefined
var a 
//声明范围是函数作用域
function a(){
    var b = 1  //外部无法访问
}
//允许重复声明
var a = 1
var a = 2
//声明会成为window的属性
alert(window.a) // 2
```

**const**

```js
//定义常量 不可修改 一定要赋初始值
const c = 1
c = 2 //报错
const d //报错
//不可重复声明
//声明范围为块作用域
const b = 1
if(b){
    const b = 2 //只在本块起作用 无法暴露到外部
}
alert(b) //1
//对于数组和对象元素的修改，不算作对常量的修改，不会报错
const A = ['a','a1']
A[0] = 'a0'
A.push('a2')
A = ['a0','a1','a2']
```



### DataType

1. 基本数据类型（值）

   ```js
   String  'string'
   Number   666
   Boolean  true false 
   null  null
   undefined  undefined
   ```

2. 对象数据类型（引用）

   ```js
   Object
   Array
   Function
   ```

3. 数据类型判断

   - typeof

     可以判断string number function undefied

     不可以判断null和array(被认定为是object)

     ```js
     typeof  //返回数据类型字符串表达  
     typeof a  //'undefined' 'number' 'string' 'object' 'function'
     typeof a === 'string' //true false
     a = null 
     typeof a   //'object'
     ```

   - instanceof

     判断对象具体类型

     ```js
     instanceof  //返回true false
     a instanceof Object/Array/Function
     ```

   - ===

     ```js
     ===
     //可判断string number function undefied null array
     ```

   - Array.isArray()

     ```js
     //判断是否为数组对象
     Array.isArray([1,2])
     ```

### 数据类型转化

**数字转字符串**

```js
String(value)
value.toString()
```

**字符串转数字**

Number() 参数只能是数字，否则返回NaN。

整型 ParseInt(value) 将参数变成整数类型，如果不能转，返回NaN。

浮点型ParseFloat(value) 将参数截取数值部分，如果不能截取返回NaN。

```js
var a='12a';
var b='12.3a5';
var c='a12';
var d='1.32';
Number(a/b/c/d)  //NaN NaN NaN 1.32
parseInt(a/b/c/d) //12 12 NaN 1
parseFolat(a/b/c/d)  // 12 12.3 NaN 1.32
```

**变布尔值**

```js
Boolean(value)
```



### 模板字符串

```js
let name = "Qin Shiwen"
let str = `hello
	${name}`  //可换行
```



## JavaScript数组

#### 数组检测

```js
arr instance of Array
Array.isArray(arr)
```

#### 数组迭代器

```js
keys()     //数组索引
values()   //数组元素
entries()  //索引+元素
Array.from(a.entries())
[0, 1]
[1, 5]
[2, 3]
[3, 4]
```

#### 数组复制

**copyWithin**：浅复制部分内容

```js
c = [1,2,3,4,5,6,7]
//索引0开始的内容复制到索引4开始的位置
c.copyWithin(4)  // [1, 2, 3, 4, 1, 2, 3]
//索引从3开始到5之前的内容复制到到2开始的位置
c.copyWithin(2,3,5) //  [1, 2, 4, 5, 5, 6, 7]
```

#### 数组填充

```js
arr = [0,1,2,3,4,5]
//填充整个数组
arr.fill(5)
//填充数组index>=3的
arr.fill(0,3)   //[0, 1, 2, 0, 0, 0]
//填充数组4>index>=3
arr.fill(0,3,4) //[0, 1, 2, 0, 4, 5]
```

#### 转化字符串

```js
arr = [0,1,2,3,4,5]
arr.toString()  // '0,1,3,4,4,5'
arr.join('-')  //'0-1-2-3-4-5'
```

#### 栈与队列方法

```js
push()  //后入
pop()   //后出
shift()  //前出
unshift()  //前入
```

#### 数组排序

```js
reverse()	// 到排序
sort()	//正排序
//从小到大
arr((a,b)->a-b)
//从大到小
arr((a,b)->b-a)
```

#### Concat

1. 创建当前数组副本加上参数后返回新数组。
2. 复制数组。

```js
arr = [3,4,5,1,6,7,8]
let arr = arr.concat([9,0])	//arr2 = [3,4,5,1,6,7,8,9,0]
let arr2 = arr.concat()
```

将 `Symbol.isConcatSpreadable` 属性设置为 `false`，以指示该对象在 `concat()` 方法中不可展开。

```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const arr3 = [7, 8, 9];
arr2[Symbol.isConcatSpreadable] = false;
const concatenatedArray = arr1.concat(arr2, arr3);
console.log(concatenatedArray); // 输出: [1, 2, 3, [4, 5, 6], 7, 8, 9]
```



#### Splice

参数：将改变的元素下标，删除数量，要插入的值

```js
arr = [3,4,5,1,6,7,8]
//删除前两个元素
arr.splice(0,2)  //[5,1,6,7,8] 
//替换：把索引为0的元素替换成1
arr.splice(0,1,1) //[1,4,5,1,6,7,8]
//插入：在索引为0的下标插入元素1和2
arr.splice(0,0,1,2)  //[1,2,3,4,5,1,6,7,8]
```



#### 数组搜索与位置

**indexOf( ) lastIndexOf( )**  若找不到则返回-1

```js
arr = [3,4,5,1,6,7,8]
//从前往后搜素值为4的下标
arr.indexOf(4) //1
//从前往后搜素值为4的下标
arr.lastIndexOf(4)
```

**includes** 是否包含该元素

```js
arr.includes(4) //true
```

**find** 返回第一个匹配元素。**findIndex**返回第一个匹配元素下标

```js
arr = [3,4,5,1,6,7,8]
arr.find((element,index,arra)=> element<5 ) //3
arr.findIndex((element,index,arra)=> element<5 )  //0
```

#### 遍历迭代

**every** 判断每一项是否符合要求，若全部符合则返回true。

```js
arr = [3,4,5,1,6,7,8]
arr.every((item,index)=>item>3)  // false
```

**some** 判断是否存在任意一项是否符合要求，若全部符合则返回true。

```js
arr.some((item,index)=>item>3)  // true
```

**filter** 过滤出符合要求的元素

```js
result = arr.filter((item,index,array) => item>3)  
//[4, 5, 6, 7, 8]
```

**forEach** 遍历

```js
arr = [3,4,5,1,6,7,8]
result = arr.forEach((item,index,array) => {
    //执行操作
})
```

**map** 把结果构成数组

```js
arr = [3,4,5,1,6,7,8]
result = arr.map((item,index,array) => item*2) //[6, 8, 10, 2, 12, 14, 16]
```



#### 归并reduce

```js
arr.reduce((prev,cur)=>{
	return prev+cur
},0)  //设置初始值
```



#### 数组拍平flat

```js
arr = [[1,2],3,4,5,[6,[7]]]
arr.flat()  //  [1,2,3,4,5,6,[7]]
arr.flat(2)  // [1,2,3,4,5,6,7]
```



## JavaScript对象

### JavaScript函数

#### This

`this`用于访问当前执行函数的对象，可以在函数内部使用它来引用该对象的属性和方法。

```js
const obj = {
  name: 'John',
  sayHello: function() {
    console.log('Hello, ' + this.name);
  }
};
obj.sayHello(); // 输出: Hello, John
```

使用`call()`和`apply()`方法指定`this`：`call()`和`apply()`是函数对象的方法，可以用来显式指定函数在调用时的`this`值。

```js
function sayHello() {
  console.log('Hello, ' + this.name);
}

const obj1 = { name: 'John' };
const obj2 = { name: 'Jane' };

sayHello.call(obj1); // 输出: Hello, John
sayHello.apply(obj2); // 输出: Hello, Jane
```

箭头函数中的`this`：箭头函数的`this`绑定在函数定义时，而不是运行时。它会捕获函数所在的上下文的`this`值。

```js
const obj = {
  name: 'John',
  sayHello: () => {
    console.log('Hello, ' + this.name);
  }
};
obj.sayHello(); // 输出: Hello, undefined
```



#### rest参数

```js
function fn(a,b,...args){ //rest参数：...args
    //会把args当成数组，可filter\some\every等等
}
```



#### async & await

await只能在async函数中

```js
// async函数返回值为promise对象
async function fn(){
	//等待获取fetchdata返回值
	let res = await fetchdata()
	return res
} 
```

处理Ajax请求

```js
function sendAjax(){
    try {
        let req = new Promise((res,rej)=>{
            const xml = XMLHttpRequest()
            xml.open("POST","URL")
            xml.send()
            xml.onreadystatechange = function(){
                if(xml.readystate === 4){
                    if(xml.status>=200 && xml.status<300){
                        resolve(xml.response)
                    }else{
                        reject(xml.status)
                    }
                }
            }
        })
        req.then((value)=>{
            return value
        })
    } catch (err){
        console.log(err)
    }
}

async function main(){
    const res = await sendAjax()
    return res
}
```



### Promise

封装ajax请求

```js
const p = new Promise((resole,reject)=>{
	//实例
	const xhr = new XMLHttpRequest()
	//准备请求接口
	xhr.open("GET","http://172.26.1.174:5001/")
	//发送请求
	xhr.send()
	//绑定事件，处理响应结果
	xhr.onreadystatechange = function(){
		if(xhr.readyState === 4){
			if(xhr.status>=200 && xhr.status<300){
				resole(xhr.response)
			}else{
				reject(xhr.status)
			}
		}
	} 
})
//处理返回的数据
p.then((value)=>{
	document.body.innerHTML = value
},(reason)=>{
	console.log(reason)
})
```

then方法可以链式调用，嵌套异步任务

```js
p.then((value)=>{},(reason)=>{}).then().then()...
```

串联多个异步任务。

```js
const p = new Promise((resole,reject)=>{
	//实例
	const xhr = new XMLHttpRequest()
	//准备请求接口
	xhr.open("GET","http://172.26.1.174:5001/")
	//发送请求
	xhr.send()
	//绑定事件，处理响应结果
	xhr.onreadystatechange = function(){
		if(xhr.readyState === 4){
			if(xhr.status>=200 && xhr.status<300){
				resole(xhr.response)
			}else{
				reject(xhr.status)
			}
		}
	} 
})

p.then((value)=>{
    return new Promise((resole,reject)=>{
        //再做请求
        let data = ...
        resole([value,data]) //封装好先前请求的数据和本次请求完成的数据
    })
}).then((value)=>{
	console.log(value)   //获取两次请求的结果     
}) 
```

catch方法

```js
p.catch((reason)=>{
	console.log(reason)  //可直接报错
})
```



### Set

- Set建立，属性与方法

  ```js
  let s = new Set()
  //可实现去重
  let s2 = new Set([1,1,2,2,3,3])  // 1 2 3
  //元素个数
  s2.size() // 3
  //增加
  s2.add('4') // 1 2 3 4
  //删除
  s2.delete('1')  //2 3 4
  //检测 
  s2.has('2') //true
  ```

- Set交集、并集、差集

  ```js
  //交集
  let arr1 = [1,2,3,4,5]
  let arr2 = [2,4,6,8]
  let result = [...new Ser(arr1)].filter(item => new Set(arr2).has(item)
  //并集
  let result = [...new Set([...arr1,...arr2])]
  //差集
  let result = [...new Ser(arr1)].filter(item => !new Set(arr2).has(item)  //1 3 5
  ```

  

### Map

```js
let m = new Map()
//插入key:value
m.set('name','QinShiwen')  //{ name:QinShiwen }
m.size() //大小
m.delete('name')  //删除
m.get('name')	//获取
m.clear()	//清空
//遍历
for(let v of m){
    ...
}
```



### 对象

#### 迭代器

```js
const person = {
	name:'Qin Shiwen'
	age:22
}
Object.keys(person)	//获取键
Object.values(person)	//获取值
Object.entries(person)	//返回数组[[key,value]...]
//方便创建map
const m = new Map(Object.entries(person))
```

#### 解构赋值

```js
let A = ['a','b','c','d']
let [a,b,c,d] = A
alert(a)  //'a'

const Person = {
	name:'Qin Shiwen',
	age:22,
	show:function(){
		alert('1')
	}
}
let {name,age,show} = Person
```



#### 对象合并

```js
const O1 = {name:''}
const O2 = {Age:22}
const O = [...O1,...O2] //{ name: age: }
```



### 宏任务与微任务



## 练习题

### 直角三角形

```js
请补全JavaScript代码，要求在页面上渲染出一个直角三角形，三角形换行要求使用"br"实现。三角形如下：
*
**
***
```

```js
var triangle = document.querySelector('.triangle');
// 补全代码
let str = ""
for(let i=0;i<3;i++){
    str = str + `*`.repeat(i+1) + `<br />`
}
triangle.innerHTML = str
```



### 拍平数组