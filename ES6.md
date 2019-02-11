# 1. let和const
## ① let
let声明的变量只在let所在代码块内有效	
let只能声明变量一次，var可以重复声明	
在for循环中，let声明的变量只在当前轮循环有效，循环时如果使用let定义的变量，则每次循环时都相当于使用了不同的变量：	
*例：*		

	'use strict'

	for(var i = 0; i < 10; i++)
	{
		setTimeout(
			function()
			{
				console.log('i = ' + i);
			}
			)
	}

	for(let j = 0; j < 10; j++)
	{
		setTimeout(
			function()
			{
				console.log('j = ' + j);
			}
			)
	}
    //第一段程序输出10个10，第二段程序输出1,2,3,4,5,6,7,8,9；
    //这是因为i为全局变量，每次循环使用的都是这同一个全局变量，而j是局部变量，只在当前此次循环有用，每次循环相当于新的变量。
    //setTimeout函数每次执行相当于在主线程后添加事件，当前线程内执行完毕后，依次执行添加的事件。
---
## ② const
const声明只读变量，声明后变量值不允许再改变，因此必须在声明时进行初始化。	

---
**注：**let和const在使用时会绑定代码块，即从声明这些变量的代码块的开始到结束形成一个封闭区，在这个作用域内，如果在声明这些变量之前使用这些变量，就会报错。	
*例：*

	var PI = "a";
	if(true){
  	console.log(PI);  // ReferenceError: PI is not defined
  	const PI = "3.1415926";
	}
    
---

# 2.解构
## ① 数组模型的解构
	let [a,b,c] = [1,2,3];//a=1;b=2;c=3
    
    //可忽略
    let [a, ,b] = [1,2,3];//a=1;b=3
    
    //不完全解构
    let [a=1,b] = [];//a=1;b=undefined
    
    //剩余运算符
    let [a, ...b] = [1,2,3];//a=1;b=[2,3]
---
## ② 对象模型的解构
	let {foo,bar} = {foo:'aaa', bar:'bbb'};//foo='aaa';bar='bbb'
    let {baz:foo} = {baz:'ddd'};//foo='ddd'
---
# 3.原始数据类型Symbol
每个Symbol值都是独一无二的：	

	let sy = Symbol('kk');
    consloe.log(sy);
    console.log(typeof(sy));
    
    let sy1 = Symbol('kk');
    console.log(sy1);
    
    console.log(sy === sy1);
    
    /*
    输出如下： 
    Symbol(kk)
    symbol
    Symbol(kk)
    false
    */
    
由上可见，即使赋值相同，Symbol的返回值也不同。

---

## 可用于对象的属性名
	let sy = Symbol("key1");
 
	// 写法1
	let syObject = {};
	syObject[sy] = "kk";
	console.log(syObject);    // {Symbol(key1): "kk"}
 
	// 写法2
	let syObject = {
  	[sy]: "kk"
	};
	console.log(syObject);    // {Symbol(key1): "kk"}
 
	// 写法3
	let syObject = {};
	Object.defineProperty(syObject, sy, {value: "kk"});
	console.log(syObject);   // {Symbol(key1): "kk"}
    
**注：不知道为什么，输出时显示为空的大括号**

---

## Symbol.for()
首先在全局搜寻已登记的Symbol中是否具有以该字符串为名称的Symbol值，有，则返回该值；没有，则新建并返回以该字符串为名称的Symbol值，并登记。

	let yellow = Symbol("Yellow");
	let yellow1 = Symbol.for("Yellow");
	yellow === yellow1;      // false
 
	let yellow2 = Symbol.for("Yellow");
	yellow1 === yellow2;     // true
---

## Symbol.keyFor()
Symbol.keyFor() 返回一个已登记的 Symbol 类型值的 key ，用来检测该字符串参数作为名称的 Symbol 值是否已被登记。

	let yellow1 = Symbol.for("Yellow");
	Symbol.keyFor(yellow1);    // "Yellow"
 ---
 
 # 4.Map对象
 Map对象的键可以是任意值，object的键只能是字符串或者Symbol。	
 ## ① Map中的key
 	//key是字符串
    var myMap = new Map();
    var keyString = "a string";
    
    myMap.set(keyString, "和键'a string'关联的值");
    
    myMap.get(keyString); //"和键'a string'关联的值"
    myMap.get("a string"); //"和键'a string'关联的值"
    					   //因为keyString === 'a string'
                           
    //key是NaN
    var myMap = new Map();
    myMap.set(NaN, "not a number");
    
    myMap.get(NaN); // "not a number"
    
    var otherNaN = Number("foo");
    myMap.get(otherNaN); // "not a number"  
    //var otherNaN = Number("foo");
    //myMap.set(otsherNaN, "other number 'foo'");
	//myMap.get(otherNaN); // "other number 'foo'"
    //myMap.get(Number("foo")); // "other number 'foo'"
    
**虽然NaN和任何值甚至自己都不相等，但它作为Map的键来说是没有区别的**

---
## ② Map迭代
对Map进行遍历，两种最高级方式：for...of 和 forEach().	
	
    //for...of
    let myMap = new Map();
	myMap.set(0,"zero");
	myMap.set(1,"one");

	for(let [key,value] of myMap)
    {
    	console.log(key + "=" + value);
    } // 显示 0=zero, 1=one

	for(let [key,value] of myMap.entries())
	{
  		console.log(key + " = " + value);
	} // 显示 0=zero, 1=one
    //这个entires方法返回一个新的容器(Iterator)对象，它按插入顺序包含了Map对象中每个[key,value]数组

	for(let key of myMap.keys())
  	{
    	console.log(key);
  	} //显示 0, 1

	for(let value of myMap.values())
  	{
    	console.log(value);
  	} // 显示 zero, one
    
    
    //forEach()
    let myMap = new Map();
    myMap.forEach(function(value,key){
    	console.log(key + " = " + value);
    }) //显示 0=zero, 1=one
    //注意function后面的参数，前面是value，后面是key
    
---

## ③ Map对象的操作
### Map与数组(Array)的转换
	let kvArray = [["key1", "value1"], ["key2", "value2"]];
    
    let myMap = new Map(kvArray);
    //将二维键值对数组转化为Map对象
    
    let outArray = Array.from(myMap);
    //使用Array.from()函数，将Map对象转化为一个二维键值对数组
    
### Map的克隆

	let myMap1 = new Map([["key1", "value1"], ["key2", "value2"]]);

	let myMap2 = new Map(myMap1);
	for(let [key, value] of myMap2)
  	{
    	console.log(key + " = " + value);
  	} // 打印 key1 = value1, key2 = value2

	console.log(myMap1 === myMap2); // 打印 false,Map 对象构造函数生成实例，迭代出新的对象
    
### Map的合并

	let myMap1 = new Map([[1, 'one'], [2, 'two'], [3, 'three']]);
    let myMap2 = new Map([[1, 'uno'], [2, 'dos']]);
    
    let myMap3 = new Map([...myMap1, ...myMap2]);
    
    for(let [key,value] of myMap3)
  	{
    	console.log(key + " = " + value);
  	} // 打印 1 = uno, 2 = dos, 3 = three
    //融合时，当有键重复时，后面的键值覆盖前面的键值
    
# 5. Set对象
Set对象可存储不同类型的值，但存储的值是唯一的，不能重复。	
**几种特殊值：**	
* +0和-0是恒等的，不重复	
* undefined和undefined是恒等的，不重复	
* NaN和NaN不恒等，但Set中只能存一个，不重复		


	let mySet = new Set();
	mySet.add(1);
	mySet.add(2);
	mySet.add(5);
	mySet.add(5); // 打印 Set(3) {1, 2, 5}，5不重复
	console.log(mySet);
	mySet.add('some text');
	console.log(mySet);// 打印 Set(4) {1, 2, 5, "some text"}，可存储不同类型的值

	let o = {'a':1, 'b':2};
	mySet.add(o);
	mySet.add({'a':1, 'b':2});
	console.log(mySet); // 打印 Set(6) {1, 2, 5, "some text", {…}, …}, 对象之间即使值相同，引用不同也不恒等
    					//4: value: {a: 1, b: 2}; 5: value: {a: 1, b: 2}

## Set对象与数组的转换

	var mySet = new Set(['value1', 'value2', 'value3']);
    //注意初始化时若要同时赋几个值，是有中括号的，否则会将赋的字符串转化为单个字符

	var outArray = [...mySet]; // 利用[...Set]将Set对象转化为数组
	console.log(outArray); // 打印 (3) [“value1”, “value2”, “value3”]
    
## String转Set

	var mySet = new Set('hello');
    // 注：Set 中 toString 方法是不能将 Set 转换成 String
	console.log(mySet); // 打印 ‘h’ ‘e’ ‘l’ ‘o’
    
## Set对象的应用

	// 数组去重
    var mySet = new Set([1,2,3,4,4,5,5,6]);
	var outArray = [...mySet];
	console.log(outArray); // 打印 (6) [1, 2, 3, 4, 5, 6]

	var a = new Set([1,2,3]);
	var b = new Set([4,3,2]);

	// 求并集
	var union = new Set([...a, ...b]);
	console.log(union); // 打印 Set(4) {1, 2, 3, 4}

	// 求交集
	var intersect = new Set([...a].filter(x => b.has(x)));
	console.log(intersect); // 打印 Set(2) {2, 3}

	// 求a与b的差集
	var difference_a = new Set([...a].filter(x => !b.has(x)));
	console.log(difference_a); // 打印 Set(1) {1}

	// 求b与a的差集
	var difference_b = new Set([...b].filter(x => !a.has(x)));
	console.log(difference_b); // 打印 Set(1) {4}
   
---

# 6. Proxy和Reflect
Proxy 可以对目标对象的读取、函数调用等操作进行拦截，然后进行操作处理。它不直接操作对象，而是像代理模式，通过对象的代理对象进行操作，在进行这些操作时，可以添加一些需要的额外操作。	
Reflect 可以用于获取目标对象的行为，它与 Object 类似，但是更易读。它的方法与 Proxy 是对应的。

---
## ① Proxy
一个Proxy对象由两部分组成，target和handler。	
target即目标对象，handler是一个对象，声明了代理对象target的指定行为。

### 一般用法：
	let target = {
  	name : "Tom",
  	age : 25
	}

	let handler = {
  	get: function(target, key){
    	console.log('getting ' + key);
    	return target[key]; //注意此处不是target.key，而是target[key]
  	}, //注意此处要加逗号而不是分号
  	set: function(target, key, value){
  	console.log('setting ' + key);
  	target[key] = value;
  	}
	}

	let myProxy = new Proxy(target, handler);
	console.log(myProxy.name); // 打印 Tom；实际执行 handler.get
	myProxy.name = 'John'; // 实际执行 handler.set
	console.log(myProxy.name); // 打印 John

	//target 可以为空对象
	let targetEpt = {};
	let proxyEpt = new Proxy(targetEpt, handler);
	console.log(proxyEpt.name); // 打印 undefined；调用get方法，此时对象为空，没有name属性
	proxyEpt.name = 'John'; // 调用set方法，向对象添加了name属性
	console.log(targetEpt.name); // 打印John
    // 通过构造函数新建实例时，相当于对目标对象进行了浅拷贝，因此目标对象和代理对象会相互影响

	// handler 对象也可以为空，相当于不设置拦截操作，直接访问目标对象
	let targetEmpty = {};
	let proxyEmpty = new Proxy(targetEmpty, {});
	console.log(proxyEmpty.name); // 打印 undefined
	proxyEmpty.name = 'Jenny'; // 直接对目标对象进行操作
	console.log(targetEmpty.name); // 打印Jenny
    
---
### 实例方法：
**get(target, propKey, receiver)**	

	// 用于target对象上propKey的读取操作
	let exam ={
    	name: "Tom",
    	age: 24
	}
	let proxy = new Proxy(exam, {
  	get(target, propKey, receiver) {
    	console.log('Getting ' + propKey);
    	return target[propKey];
  	}
	})
	console.log(proxy.name); // 打印 Tom
    
    // get方法可以继承
    let obj = Object.create(proxy);
	console.log(obj.name); // 打印 Tom
    
---
**set(target, propKey, value, receiver)**	

	//用于拦截 target 对象上的 propKey 的赋值操作。如果目标对象自身的某个属性，不可写且不可配置，那么set方法将不起作用。
    let validator = {
    	set: function(obj, prop, value) {
        	if (prop === 'age') {
            	if (!Number.isInteger(value)) {
                	throw new TypeError('The age is not an integer');
            	}
            	if (value > 200) {
                	throw new RangeError('The age seems invalid');
            	}
        	}
        	// 对于满足条件的 age 属性以及其他属性，直接保存
        	obj[prop] = value;
    	}
	};
	let proxy= new Proxy({}, validator)
	proxy.age = 100;
	proxy.age           // 100
	proxy.age = 'oppps' // 报错 The age is not an integer
	proxy.age = 300     // 报错 The age seems invalid
    
    // 参数 receiver 表示原始操作行为所在对象，一般是 Proxy 实例本身
	const handler = {
    	set: function(obj, prop, value, receiver) {
            obj[prop] = receiver;
    	}
	};
	const proxy = new Proxy({}, handler);
	proxy.name= 'Tom';
	console.log(proxy); // 打印 Proxy {name: Proxy}
	console.log(proxy.name=== proxy); // 打印 true

---
**apply(target, ctx, args)**	

	// 用于拦截函数的调用、call 和 reply 操作。target 表示目标对象，ctx 表示目标对象上下文，args 表示目标对象的参数数组
    function sub(a, b){
    	return a - b;
	}
	let handler = {
    	apply: function(target, ctx, args){
        	console.log('handle apply');
        	return Reflect.apply(...arguments);
    	}
	}
	let proxy = new Proxy(sub, handler);
	console.log(proxy(2, 1));  // 打印 1
    
---
**has(target, propKey)**	

	//用于拦截 HasProperty 操作，即在判断 target 对象是否存在 propKey 属性时，会被这个方法拦截。此方法不判断一个属性是对象自身的属性，还是继承的属性
    let  handler = {
    	has: function(target, propKey){
        	console.log("handle has");
        	return propKey in target;
    	}
	}
	let exam = {name: "Tom"};
	let proxy = new Proxy(exam, handler);
	console.log('name' in proxy); // 打印 true
    //注意：此方法不拦截 for ... in 循环
    
---
**construct(target, args)**

	// 用于拦截new命令。返回值必须为对象
	let handler = {
    	construct: function(target, args, newTarget){
        	console.log("handle construct");
        	return Reflect.construct(target, args, newTarget);
    	} }
	function exam(name,age){
        		this.name = name;
  				this.age = age;
    		}

	let proxy = new Proxy(exam,handler)
	let stu = new proxy("Tom",25); // 相当于生成下面stu1对象的过程
	console.log(stu); // 打印 exam {name: "Tom", age: 25}

	let stu1 = new exam('John', 24);
	console.log(stu1); // 打印 exam {name: "John", age: 24}
    
---
**deleteProperty(target, propKey)**	
*用于拦截 delete 操作，如果这个方法抛出错误或者返回 false ，propKey 属性就无法被 delete 命令删除*

---
**defineProperty(target, propKey, propDesc)**

	// 用于拦截 Object.definePro若目标对象不可扩展，增加目标对象上不存在的属性会报错；若属性不可写或不可配置，则不能改变这些属性
    let handler = {
    	defineProperty: function(target, propKey, propDesc){
        	console.log("handle defineProperty");
      		//target[propKey] = propDesc;
        	return true;
    	}
	}
	let target = {}
	let proxy = new Proxy(target, handler)
	proxy.name = "Tom"
	// handle defineProperty
	console.log(target);
    // 应该显示{name: "Tom"}
**但不知道为什么显示为空：{}**
 
	// defineProperty 返回值为false，添加属性操作无效
	let handler1 = {
    	defineProperty: function(target, propKey, propDesc){
        	console.log("handle defineProperty");
      		//target[propKey] = propDesc;
        	return false;
    	}
	}
	let target1 = {}
	let proxy1 = new Proxy(target1, handler1)
	proxy1.name = "Jerry"
	console.log(target1); //显示为空{}
    
---
**getOwnPropertyDescriptor(target, propKey)**	

	// 用于拦截 Object.getOwnPropertyD() 返回值为属性描述对象或者 undefined 
	let handler = {
    	getOwnPropertyDescriptor: function(target, propKey){
        	return Object.getOwnPropertyDescriptor(target, propKey);
    	}
	}
	let target = {name: "Tom"};
	let proxy = new Proxy(target, handler);
	console.log(Object.getOwnPropertyDescriptor(proxy, 'name')); // 打印 {value: "Tom", writable: true, enumerable: true, configurable: true}
    console.log(Object.getOwnPropertyDescriptor(proxy, 'age')); // 打印 undefined
    
---
**getPrototypeOf(target)**	

	// 主要用于拦截获取对象原型的操作。包括以下操作：
    - Object.prototype._proto_
	- Object.prototype.isPrototypeOf()
	- Object.getPrototypeOf()
	- Reflect.getPrototypeOf()
	- instanceof

	// 代码如下：
    let exam = {name : 'Tom' , age : 25};
    let exam1 = {};
	let proxy = new Proxy({},{
    	getPrototypeOf: function(target){
        	return exam;
    	}
	})
    
    	let proxy1 = new Proxy({},{
    	getPrototypeOf: function(target){
        	return exam1;
    	}
	})
    
	console.log(Object.getPrototypeOf(proxy)); // 打印 {name: "Tom", age: 25}
    console.log(Object.getPrototypeOf(proxy1)); // 打印 {}
    
    // 注意，返回值必须是对象或者 null ，否则报错。另外，如果目标对象不可扩展（non-extensible），getPrototypeOf 方法必须返回目标对象的原型对象
    let proxy = new Proxy({},{
    	getPrototypeOf: function(target){
        	return true;
    	}
	})
	Object.getPrototypeOf(proxy); // 报错 Uncaught TypeError: 'getPrototypeOf' on proxy: trap returned neither object nor null
    
---
**isExtensible(target)**	

	// 用于拦截 Object.isExtensible 操作。
	// 该方法只能返回布尔值，否则返回值会被自动转为布尔值。
    let proxy = new Proxy({},{
    	isExtensible:function(target){
        	return true;
    	}
	})
	console.log(Object.isExtensible(proxy)); // 打印 true
    
    // 注意它的返回值必须与目标对象的isExtensible属性保持一致，否则会抛出错误:
    let proxy = new Proxy({},{
    	isExtensible:function(target){
        	return false;
    	}
	})
	Object.isExtensible(proxy); // 报错 Uncaught TypeError: 'isExtensible' on proxy: trap result does not reflect extensibility of proxy target (which is 'true') at Function.isExtensible 
    
---
**ownKeys(target)**	

	// 用于拦截对象自身属性的读取操作。主要包括以下操作：
    - Object.getOwnPropertyNames()
	- Object.getOwnPropertySymbols()
	- Object.keys()
	- or...in
	
    // 方法返回的数组成员，只能是字符串或 Symbol 值，否则会报错。
	// 若目标对象中含有不可配置的属性，则必须将这些属性在结果中返回，否则就会报错。
	// 若目标对象不可扩展，则必须全部返回且只能返回目标对象包含的所有属性，不能包含不存在的属性，否则也会报错。
    let proxy = new Proxy( {
  	name: "Tom",
  	age: 24
	}, {
    	ownKeys(target) {
        	return ['name'];
    	}
	});
	console.log(Object.keys(proxy)); // 打印 ["name"]
    
    /* 
    返回结果中，三类属性会被过滤：
          - 目标对象上没有的属性
          - 属性名为 Symbol 值的属性
          - 不可遍历的属性
     */
     let target = {
  	name: "Tom",
  	[Symbol.for('age')]: 24,
	};
	// 添加不可遍历属性 'gender'
	Object.defineProperty(target, 'gender', {
  	enumerable: false,
  	configurable: true,
  	writable: true,
  	value: 'male'
	});
	let handler = {
    	ownKeys(target) {
        	return ['name', 'parent', Symbol.for('age'), 'gender'];
    	}
	};
	let proxy = new Proxy(target, handler);
	console.log(Object.keys(proxy)); // 打印 ["name"]; 只有name属性被返回，其他被过滤了
    
---
**preventExtensions(target)**	

	// 拦截 Object.preventExtensions 操作。
	// 该方法必须返回一个布尔值，否则会自动转为布尔值。
    
    // 只有目标对象不可扩展时（即 Object.isExtensible(proxy) 为 false ），proxy.preventExtensions 才能返回 true ，否则会报错
    
    var proxy = new Proxy({}, {
  	preventExtensions: function(target) {
    	return true;
  	}
	});
	// 由于 proxy.preventExtensions 返回 true，此处也会返回 true，因此会报错
	console.log(Object.preventExtensions(proxy)); // 报错 Uncaught TypeError: 'preventExtensions' on proxy: trap returned truish but the proxy target is extensible at Function.preventExtensions 
#### *应该是由于target可扩展，即Object.isExtensible(proxy) 为true，所以冲突才报错吧，不太理解*

	// 修改如下：
	 var proxy = new Proxy({}, {
  	preventExtensions: function(target) {
        // 返回前先调用 Object.preventExtensions
    	console.log(Object.preventExtensions(target)); // 打印{}
    	return true;
  	}
	});
	console.log(Object.preventExtensions(proxy)); // 打印 Proxy {}
    
---
**setPrototypeOf**	

	// 主要用来拦截 Object.setPrototypeOf 方法。
	// 返回值必须为布尔值，否则会被自动转为布尔值。
	// 若目标对象不可扩展，setPrototypeOf 方法不得改变目标对象的原型。
    let proto = {}
	let proxy = new Proxy(function () {}, {
    	setPrototypeOf: function(target, proto) {
        	console.log("setPrototypeOf");
        	return true;
    	}
	});
	Object.setPrototypeOf(proxy, proto);
    // 结果中打印 setPrototypeOf
    
---
**Proxy.revocable()**	

	// 用于返回一个可取消的 Proxy 实例
    let {proxy, revoke} = Proxy.revocable({}, {});
	proxy.name = "Tom";
	revoke();
	proxy.name; // 报错 TypeError: Cannot perform 'get' on a proxy that has been revoked
    
---
## ② Reflect
ES6 中将 Object 的一些明显属于语言内部的方法移植到了 Reflect 对象上（当前某些方法会同时存在于 Object 和 Reflect 对象上），未来的新方法会只部署在 Reflect 对象上。	
Reflect 对象对某些方法的返回结果进行了修改，使其更合理。	
Reflect 对象使用函数的方式实现了 Object 的命令式操作。

### 静态方法
**Reflect.get(target, name, receiver)**	

	// 查找并返回 target 对象的 name 属性
    let exam = {
    	name: "Tom",
    	age: 24,
    	get info(){
        	return this.name + this.age;
    	}
	}
	console.log(Reflect.get(exam, 'name')); // 打印 Tom
	console.log(Reflect.get(exam, 'age'));  // 打印 24

	// 当 target 对象中存在 name 属性的 getter 方法， getter 方法的 this 会绑定 receiver
	let receiver = {
    	name: "Jerry",
    	age: 20
	}
	console.log(Reflect.get(exam, 'info', receiver)); // 打印 Jerry20； 
    //打印的结果为receiver对象中的内容而不是exam对象中的内容，这是因为getter方法的this绑定了receiver
 
	// 当 name 为不存在于 target 对象的属性时，返回 undefined
	console.log(Reflect.get(exam, 'birth')); // 打印 undefined
    
    // 当 target 不是对象时，会报错
	Reflect.get(1, 'name'); // 报错 Uncaught TypeError: Reflect.get called on non-object
    
---
**Reflect.set(target, name, value, receiver)**	

	// 将 target 的 name 属性设置为 value。返回值为 boolean ，true 表示修改成功，false 表示失败。当 target 为不存在的对象时，会报错
    let exam = {
    	name: "Tom",
    	age: 24,
    	set info(value){
        	return this.age = value;
    	}
	}
	console.log(exam.age); // 打印 24
	console.log(Reflect.set(exam, 'age', 25)); // 打印 true
	console.log(exam.age); // 打印 25
	console.log(Reflect.set(exam, 'color', 'blue')); // 打印 true
	console.log(exam.color); // 打印 blue
 
	// value 为空时会将 name 属性清除
	console.log(Reflect.set(exam, 'age', )); // 打印 true
	console.log(exam.age); // 打印 undefined
 
	// 当 target 对象中存在 name 属性 setter 方法时，setter 方法中的 this 会绑定 // receiver , 所以修改的实际上是 receiver 的属性,
	let receiver = {
    	age: 18
	}
	console.log(Reflect.set(exam, 'info', 1, receiver)); // 打印 true
	console.log(receiver.age); // 打印 1
	console.log(exam.age); // 打印 undefined；因为实际上修改的是receiver的属性，所以exam属性不变
 
	let receiver1 = {
    	name: 'oppps'
	}
	console.log(Reflect.set(exam, 'info', 1, receiver1)); // 打印 true
	console.log(receiver1.age); // 打印 1；以为setter方法只设置age属性
    
---
**Reflect.has(obj, name)**	

	// 是 name in obj 指令的函数化，用于查找 name 属性在 obj 对象中是否存在。返回值为 boolean。如果 obj 不是对象则会报错 TypeError
    let exam = {
    	name: "Tom",
    	age: 24
	}
	console.log(Reflect.has(exam, 'name')); // 打印 true
	console.log(Reflect.has(exam, 'color')); // 打印 false
    
---
**Reflect.deleteProperty(obj, property)**	

	// 是 delete obj[property] 的函数化，用于删除 obj 对象的 property 属性，返回值为 boolean。如果 obj 不是对象则会报错 TypeError	
    let exam = {
    	name: "Tom",
    	age: 24
	}
	Reflect.deleteProperty(exam , 'name'); // true
	console.log(exam); // 打印 {age: 24}
    
	// property 不存在时，也会返回 true
	Reflect.deleteProperty(exam , 'name'); // true
    
---
**Reflect.construct(obj, args)**	

	// 等同于 new target(...args)
    function exam(name){
    	this.name = name;
	}
	Reflect.construct(exam, ['Tom']); // exam {name: "Tom"}
    
---
**Reflect.getPrototypeOf(obj)**	

	// 用于读取 obj 的 _proto_ 属性。在 obj 不是对象时不会像 Object 一样把 obj 转为对象，而是会报错
    class Exam{}
	let obj = new Exam()
	console.log(Reflect.getPrototypeOf(obj) === Exam.prototype); // 打印 true
    
---
**Reflect.setPrototypeOf(obj, newProto)**	

	// 用于设置目标对象的 prototype
    let obj ={}
	Reflect.setPrototypeOf(obj, Array.prototype); // true;将obj对象prototype设置为Array
    
---
**Reflect.apply(func, thisArg, args)** 

	// 等同于 Function.prototype.apply.call(func, thisArg, args) 。func 表示目标函数；thisArg 表示目标函数绑定的 this 对象；args 表示目标函数调用时传入的参数列表，可以是数组或类似数组的对象。若目标函数无法调用，会抛出 TypeError
    Reflect.apply(Math.max, Math, [1, 3, 5, 3, 1]); // 5
    
---
**Reflect.defineProperty(target, propertyKey, attributes)**	

	// 用于为目标对象定义属性。如果 target 不是对象，会抛出错误
    let myDate= {}
	Reflect.defineProperty(MyDate, 'now', {
  	value: () => Date.now()
	}); // true
    
---
**Reflect.getOwnPropertyDescriptor(target, propertyKey)**	

	// 用于得到 target 对象的 propertyKey 属性的描述对象。在 target 不是对象时，会抛出错误表示参数非法，不会将非对象转换为对象
    var exam = {}
	Reflect.defineProperty(exam, 'name', {
  	value: true,
  	enumerable: false,
	})
	console.log(Reflect.getOwnPropertyDescriptor(exam, 'name'));
	// 打印 {value: true, writable: false, enumerable: false, configurable: false}
 
 
	// propertyKey 属性在 target 对象中不存在时，返回 undefined
	console.log(Reflect.getOwnPropertyDescriptor(exam, 'age')) // 打印 undefined
    
---
**Reflect.isExtensible(target)**	

	// 用于判断 target 对象是否可扩展。返回值为 boolean 。如果 target 参数不是对象，会抛出错误
    let exam = {};
	Reflect.isExtensible(exam); // true

---
**Reflect.preventExtensions(target)**	
 
 	// 用于让 target 对象变为不可扩展。如果 target 参数不是对象，会抛出错误
    let exam = {};
	Reflect.preventExtensions(exam);
	console.log(Reflect.isExtensible(exam)); // 打印false
    
---
**Reflect.ownKeys(target)**	

	// 用于返回 target 对象的所有属性，等同于 Object.getOwnPropertyNames 与Object.getOwnPropertySymbols 之和
    var exam = {
  	name: 1,
  	[Symbol.for('age')]: 4
	}
	console.log(Reflect.ownKeys(exam)); // 打印 (2) ["name", Symbol(age)]
    
---
### 组合使用 
Reflect 对象的方法与 Proxy 对象的方法是一一对应的。所以 Proxy 对象的方法可以通过调用 Reflect 对象的方法获取默认行为，然后进行额外操作。	

	let exam = {
    	name: "Tom",
    	age: 24
	}
	let handler = {
    	get: function(target, key){
        	console.log("getting "+key);
        	return Reflect.get(target,key);
    	}, //注意此处为逗号，而不是分号
    	set: function(target, key, value){
        	console.log("setting "+key+" to "+value)
        	Reflect.set(target, key, value);
    	}
	}
	let proxy = new Proxy(exam, handler);
	console.log(proxy.name); // 打印 Tom
	proxy.name = "Jerry"; // setting name to Jerry
	console.log(proxy.name); // 打印 Jerry
    
---
# 7. ES6字符串
## 字串的识别
ES6 之前判断字符串是否包含子串，用 indexOf 方法，ES6 新增了子串的识别方法：	
* includes()：返回布尔值，判断是否找到参数字符串。	
* startsWith()：返回布尔值，判断参数字符串是否在原字符串的头部。	
* endsWith()：返回布尔值，判断参数字符串是否在原字符串的尾部。	

以上三个方法都可以接受两个参数，需要搜索的字符串，和可选的搜索起始位置索引:

	let string = "apple,banana,orange";
	console.log(string.includes('banana')); // 打印 true
	console.log(string.startsWith('apple')); // 打印 true
	console.log(string.endsWith('apple')); // 打印 false
	console.log(string.startsWith('banana', 6)); // 打印 true
    
**注意：**	
* 这三个方法只返回布尔值，如果需要知道子串的位置，还是得用 indexOf 和 lastIndexOf 。	
* 这三个方法如果传入了正则表达式而不是字符串，会抛出错误。而 indexOf 和 lastIndexOf 这两个方法，它们会将正则表达式转换为字符串并搜索它。

---
## 字符串重复

	// repeat()：返回新的字符串，表示将字符串重复指定次数返回。	
    console.log('Hello LSS '.repeat(2)); // 打印 Hello LSS Hello LSS 
    
    // 如果参数是小数，向下取整
    console.log('Hello LSS '.repeat(3.2)); // 打印 Hello LSS Hello LSS Hello LSS 
    
    //如果参数是 0 至 -1 之间的小数，会进行取整运算，0 至 -1 之间的小数取整得到 -0 ，等同于 repeat 零次
    console.log('Hello LSS '.repeat(-0.5)); // 打印 ""
    
    // 如果参数是 NaN，等同于 repeat 零次
    console.log('Hello LSS '.repeat(NaN)); // 打印 ""
    
    // 如果参数是负数或者 Infinity ，会报错:
    console.log('Hello LSS '.repeat(-1));
    console.log('Hello LSS '.repeat(Infinity)); 
    // 报错 Uncaught RangeError: Invalid count value at String.repeat
    
    // 如果传入的参数是字符串，则会先将字符串转化为数字
    console.log('Hello LSS '.repeat('hh')); // 打印 ""
    console.log('Hello LSS '.repeat('2')); // 打印 Hello LSS Hello LSS
    
---
## 字符串补全	
以下两种方法：
* padStart：返回新的字符串，表示用参数字符串从头部补全原字符串。
* padEnd：返回新的字符串，表示用参数字符串从头部补全原字符串。


	//以上两个方法接受两个参数，第一个参数是指定生成的字符串的最小长度，第二个参数是用来补全的字符串。如果没有指定第二个参数，默认用空格填充。
	console.log("h".padStart(5,"o"));  // "ooooh"
	console.log("h".padEnd(5,"o"));    // "hoooo"
	console.log("h".padStart(5));      // "    h"
    
    // 如果指定的长度小于或者等于原字符串的长度，则返回原字符串:
    console.log("hello".padStart(5,"A"));  // "hello"
    console.log("hello".padStart(4,"A"));  // "hello"
    
    // 如果原字符串加上补全字符串长度大于指定长度，则截去超出位数的补全字符串:
    console.log("hello".padEnd(10,",world!"));  // "hello,worl"
    
    // 常用于补全位数：
    console.log("123".padStart(10,"0"));  // "0000000123"
    
---
## 模板字符串
模板字符串相当于加强版的字符串，用反引号 \` ,除了作为普通字符串，还可以用来定义多行字符串，还可以在字符串中加入变量和表达式。	

**基本用法：**	

	// 普通字符串
    let string = `Hello, \n LSS`;
	console.log(string); 
    // 打印 Hello,
    //       LSS
                         
    // 多行字符串:
    console.log(`Hello
	Lss`);
    // 打印 Hello
    //      LSS
    
    // 字符串插入变量和表达式
	//变量名写在 ${} 中，${} 中可以放入 JavaScript 表达式
    let name = "CKX";
	let L_name = "LSS";
	let str = `Hello, my name is ${name} and I like ${L_name}`;
	console.log(str);
    // 打印 Hello, my name is CKX and I like LSS
    
    // 字符串中调用函数
    function f(){return 'I like LSS';}

	let str1 = `Hello, my name is CKX and ${f()}`;
	console.log(str1);
    // 打印 Hello, my name is CKX and I like LSS
    
**注意要点:**	
模板字符串中的换行和空格都是会被保留的	

	innerHtml = `<ul>
  	<li>menu</li>
  	<li>mine</li>
	</ul>
	`;
	console.log(innerHtml);
	// 打印 
    <ul>
 	<li>menu</li>
 	<li>mine</li>
	</ul>

---
## 标签模板
标签模板，是一个函数的调用，其中调用的参数是模板字符串。	
	
    alert`Hello world!`;
	// 等价于
	alert('Hello world!');
    
当模板字符串中带有变量，会将模板字符串参数处理成多个参数。	

	function f(stringArr,...values){
 	let result = "";
 	for(let i=0;i<stringArr.length;i++){
  	result += stringArr[i];
  	if(values[i]){
   	result += values[i];
        	}
    	}
 	return result;
	}
	let name = 'Mike';
	let age = 27;
	console.log(f`My Name is ${name},I am ${age+1} years old next year.`);
	// 打印 My Name is Mike,I am 28 years old next year.
    
    /*
    f`My Name is ${name},I am ${age+1} years old next year.`;
	等价于
	f(['My Name is',',I am ',' years old next year.'],'Mike',28);
    */
    
---
# 8. ES6 数值
## 数值的表示
二进制表示法新写法: 前缀 0b 或 0B

	console.log(0b11 === 3); // true
	console.log(0B11 === 3); // true

八进制表示法新写法: 前缀 0o 或 0O 

	console.log(0o11 === 9); // true
	console.log(0O11 === 9); // true
---
### 常量	
</br>
**Number.EPSILON**	

    // Number.EPSILON 属性表示 1 与大于 1 的最小浮点数之间的差
	// 它的值接近于 2.2204460492503130808472633361816E-16，或者 2-52
    
    //测试数值是否在误差范围内:
    0.1 + 0.2 === 0.3; // false
	// 在误差范围内即视为相等
	equal = (Math.abs(0.1 - 0.3 + 0.2) < Number.EPSILON); // true
    
    // 属性特性
    writable：false
	enumerable：false
	configurable：false
---

**最大/最小安全整数**	
安全整数表示在 JavaScript 中能够精确表示的整数，安全整数的范围在 2 的 -53 次方到 2 的 53 次方之间（不包括两个端点），超过这个范围的整数无法精确表示。

	// 最大安全整数,安全整数范围的上限，即 2 的 53 次方减 1 
    Number.MAX_SAFE_INTEGER + 1 === 	Number.MAX_SAFE_INTEGER + 2; // true
	Number.MAX_SAFE_INTEGER === Number.MAX_SAFE_INTEGER + 1;     // false
	Number.MAX_SAFE_INTEGER - 1 === Number.MAX_SAFE_INTEGER - 2; // false
    
    // 最小安全整数,安全整数范围的下限，即 2 的 -53 次方减 1 
    Number.MIN_SAFE_INTEGER + 1 === Number.MIN_SAFE_INTEGER + 2; // false
	Number.MIN_SAFE_INTEGER === Number.MIN_SAFE_INTEGER - 1;     // false
	Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2; //false
    
    // 属性特性
    writable：false
	enumerable：false
	configurable：false
---

### 方法

**Number.isFinite()**	

	//用于检查一个数是否为有限的(finite)， 即不是Infinity
    console.log( Number.isFinite(1));   // true
	console.log( Number.isFinite(0.1)); // true
 
	// NaN 不是有限的
	console.log( Number.isFinite(NaN)); // false
 
	console.log( Number.isFinite(Infinity));  // false
	console.log( Number.isFinite(-Infinity)); // false
 
	// Number.isFinate 没有隐式的 Number() 类型转换，所有非数值都返回 false
	console.log( Number.isFinite('foo')); // false
	console.log( Number.isFinite('15'));  // false
	console.log( Number.isFinite(true));  // false
---
   
**Number.isNaN()**	

	//用于检查一个值是否为 NaN 。
	console.log(Number.isNaN(NaN));      // true
	console.log(Number.isNaN('true'/0)); // true
 
	// 在全局的 isNaN() 中，以下皆返回 true，因为在判断前会将非数值向数值转换
	// 而 Number.isNaN() 不存在隐式的 Number() 类型转换，非 NaN 全部返回 false
	console.log(Number.isNaN("NaN"));      // false
	console.log(Number.isNaN(undefined));  // false
	console.log(Number.isNaN({}));         // false
	console.log(Number.isNaN("true"));     // false
---

**Number.parseInt()**	

	// 用于将给定字符串转化为指定进制的整数
    // 不指定进制时默认为 10 进制
	Number.parseInt('12.34'); // 12
	Number.parseInt(12.34);   // 12
    
    //// 指定进制
	Number.parseInt('0011',2); // 3
 
	// 与全局的 parseInt() 函数是同一个函数
	Number.parseInt === parseInt; // true
---

**Number.parseFloat

	// 用于把一个字符串解析成浮点数
    Number.parseFloat('123.45')    // 123.45
	Number.parseFloat('123.45abc') // 123.45
 
	// 无法被解析成浮点数，则返回 NaN
	Number.parseFloat('abc') // NaN
 
	// 与全局的 parseFloat() 方法是同一个方法
	Number.parseFloat === parseFloat // true
---

**Number.isInteger()**	

	// 用于判断给定的参数是否为整数。
	console.log(Number.isInteger(0)); // true
    
	// JavaScript 内部，整数和浮点数采用的是同样的储存方法,因此 1 与 1.0 被视为相同的值
	console.log(Number.isInteger(1));   // true
	console.log(Number.isInteger(1.0)); // true 
	console.log(Number.isInteger(1.1));     // false
	console.log(Number.isInteger(Math.PI)); // false
 
	// NaN 和正负 Infinity 不是整数
	console.log(Number.isInteger(NaN));       // false
	console.log(Number.isInteger(Infinity));  // false
	console.log(Number.isInteger(-Infinity)); // false
 
	console.log(Number.isInteger("10"));  // false
	console.log(Number.isInteger(true));  // false
	console.log(Number.isInteger(false)); // false
	console.log(Number.isInteger([1]));   // false
 
	// 数值的精度超过 53 个二进制位时，由于第 54 位及后面的位被丢弃，会产生误判
	console.log(Number.isInteger(1.0000000000000001)) // true
 
	// 一个数值的绝对值小于 Number.MIN_VALUE（5E-324），即小于 JavaScript 能够分辨的最小值，会被自动转为 0，也会产生误判
	console.log(Number.isInteger(5E-324)); // false
	console.log(Number.isInteger(5E-325)); // true
---

**Number.isSafeInteger()**	

	// 用于判断数值是否在安全范围内
    console.log(Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1)); // false
    console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false
---

## Math对象的扩展
ES6 在 Math 对象上新增了 17 个数学相关的静态方法，这些方法只能在 Math 中调用。

**Math.cbrt()**	

	// 用于计算一个数的立方根
    console.log(Math.cbrt(1)); // 1
    console.log(Math.cbrt(0)); // 0
    console.log(Math.cbrt(-1)); // -1
    console.log(Math.cbrt(8)); // 2
    
    // 会对非数值进行转换
	Math.cbrt('1'); // 1
 
	// 非数值且无法转换为数值时返回 NaN
	Math.cbrt('hhh'); // NaN
---

**Math.imul()**	

	// 两个数以 32 位带符号整数形式相乘的结果，返回的也是一个 32 位的带符号整数
    // 大多数情况下，结果与 a * b 相同 
	Math.imul(1, 2);   // 2
    Math.imul(-2, 2);   // -4
 
	// 用于正确返回大数乘法结果中的低位数值
	Math.imul(0x7fffffff, 0x7fffffff); // 1
---

**Math.hypot()**	

	// 用于计算所有参数平方和的平方根
    Math.hypot(3, 4); // 5
    
    // 非数值会先被转换为数值后进行计算
	Math.hypot(1, 2, '3'); // 3.741657386773941
	Math.hypot(true);      // 1
	Math.hypot(false);     // 0
 
	// 空值会被转换为 0
	Math.hypot();   // 0
	Math.hypot([]); // 0
    // 注意，当括号内为大括号({})时，无法转换，返回NaN
 
	// 参数为 Infinity 或 -Infinity 返回 Infinity
	Math.hypot(Infinity); // Infinity
	Math.hypot(-Infinity); // Infinity
 
	// 参数中存在无法转换为数值的参数时返回 NaN
	Math.hypot(NaN);         // NaN
	Math.hypot(3, 4, 'foo'); // NaN
	Math.hypot({});          // NaN ， 无法转换
---

**Math.clz32()**	

	// 用于返回数字的32位无符号整数形式的前导 0 的个数
    Math.clz32(0); // 32
	Math.clz32(1); // 31
	Math.clz32(0b00100000000100000000000000000000); // 2
 
	// 当参数为小数时，只考虑整数部分
	Math.clz32(0.5); // 32
 
	// 对于空值或非数值，会转化为数值再进行计算
	Math.clz32('1');       // 31
	Math.clz32();          // 32
	Math.clz32([]);        // 32
	Math.clz32({});        // 32
	Math.clz32(NaN);       // 32
	Math.clz32(Infinity);  // 32
	Math.clz32(-Infinity); // 32
	Math.clz32(undefined); // 32
	Math.clz32('hhh');     // 32
---

**Math.trunc()**	

	// 用于返回数字的整数部分
    Math.trunc(12.3); // 12
	Math.trunc(12);   // 12
    Math.trunc(-12。5);   // -12
    
    // 整数部分为 0 时也会判断符号
	Math.trunc(-0.5); // -0
	Math.trunc(0.5);  // 0
	
    // Math.trunc 会将非数值转为数值再进行处理
	Math.trunc("12.3"); // 12
 
	// 空值或无法转化为数值时时返回 NaN
	Math.trunc();           // NaN
	Math.trunc(NaN);        // NaN
	Math.trunc("hhh");      // NaN
	Math.trunc("123.2hhh"); // NaN
---

**Math.fround()**	

	// 用于获取数字的32位单精度浮点数形式    
    // 对于 负的 2 的 24 次方至 2 的 24 次方之间的整数（不含两个端点），返回结果与参数本身一致
    Math.fround(-(2**24)+1);  // -16777215
	Math.fround(2 ** 24 - 1); // 16777215
    
    // 用于将 64 位双精度浮点数转为 32 位单精度浮点数
	Math.fround(1.234) // 1.125
    
	// 当小数的精度超过 24 个二进制位，会丢失精度
	Math.fround(0.3); // 0.30000001192092896
    
	// 参数为 NaN 或 Infinity 时返回本身
	Math.fround(NaN)      // NaN
	Math.fround(Infinity) // Infinity
 
	// 参数为其他非数值类型时会将参数进行转换 
	Math.fround('5');  // 5
	Math.fround(true); // 1
	Math.fround(null); // 0
	Math.fround([]);   // 0
	Math.fround({});   // NaN
---

**Math.sign()**	

	// 判断数字的符号(正、负、0)
    Math.sign(1);  // 1
	Math.sign(-1); // -1
    Math.sign(5); // 1
    Math.sign(-5); // -1
 
	// 参数为 0 时，不同符号的返回不同
	Math.sign(0);  // 0
	Math.sign(-0); // -0
 
	// 判断前会对非数值进行转换
	Math.sign('1');  // 1
	Math.sign('-1'); // -1  
 
	// 参数为非数值（无法转换为数值）时返回 NaN
	Math.sign(NaN);   // NaN 
	Math.sign('hhh'); // NaN
---

**Math.expm1(x)**	

	// 用于计算 e 的 x 次方减 1 的结果(即 Math.exp(x)-1)
    Math.expm1(1);  // 1.718281828459045
	Math.expm1(0);  // 0
	Math.expm1(-1); // -0.6321205588285577
    
	// 会对非数值进行转换
	Math.expm1('0'); //0
 
	// 参数不为数值且无法转换为数值时返回 NaN
	Math.expm1(NaN); // NaN
---

**Math.log1p(x)**	

	//  用于计算 1+x 的自然对数(即 Math.log(1+x))
    Math.log1p(1);  // 0.6931471805599453
	Math.log1p(0);  // 0
	Math.log1p(-1); // -Infinity
 
	// 参数小于 -1 时返回 NaN
	Math.log1p(-2); // NaN
---

**Math.log10(x)**	

	// 用于计算以 10 为底的 x 的对数
    Math.log10(1);   // 0
    Math.log10(10);   // 1
    Math.log10(0.1);   // -1
    
	// 计算前对非数值进行转换
	Math.log10('1'); // 0
    
	// 参数为0时返回 -Infinity
	Math.log10(0);   // -Infinity
    
	// 参数小于0或参数不为数值（且无法转换为数值）时返回 NaN
	Math.log10(-1);  // NaN
---

**Math.log2()**	

	// 用于计算以 2 为底的 x 的对数
    Math.log2(1);   // 0
    
	// 计算前对非数值进行转换
	Math.log2('1'); // 0
    
	// 参数为0时返回 -Infinity
	Math.log2(0);   // -Infinity
    
	// 参数小于0或参数不为数值（且无法转换为数值）时返回 NaN
	Math.log2(-1);  // NaN
---

**双曲线函数方法**	

	Math.sinh(x); // 用于计算双曲正弦。
	Math.cosh(x); // 用于计算双曲余弦。
	Math.tanh(x); // 用于计算双曲正切。
	Math.asinh(x); // 用于计算反双曲正弦。
	Math.acosh(x); // 用于计算反双曲余弦。
	Math.atanh(x); // 用于计算反双曲正切。
---

**指数运算符**	

	// a ** b, 即 a 的 b 次幂
    1 ** 2; // 1
    3 ** 2; // 9
    
	// 右结合，从右至左计算
	2 ** 2 ** 3; // 256
    3 ** 2 ** 2; // 81
---

# 9. ES6 对象

	
    

    
    



	