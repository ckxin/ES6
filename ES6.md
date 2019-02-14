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
## 对象字面量
### 属性简写

	// ES6允许对象的属性直接写变量，这时候属性名是变量名，属性值是变量值
    const age = 12;
	const name = "Amy";
	const person = {age, name};
	console.log(person);   //{age: 12, name: "Amy"}
	
    //等同于
	const person = {age: age, name: name};
    ----------------------------------------
    
    // 方法名也可以简写
    const person = {
    	sayHi(){
        	console.log('Hi');
        }
    }
    person.sayHi(); // 打印 Hi
    
    // 等同于
    const person = {
  		sayHi:function(){
    		console.log("Hi");
  		}
	}
	person.sayHi(); // "Hi"
---

### 属性名表达式

	// ES6允许用表达式作为属性名，但是一定要将表达式放在方括号内
    const obj = {
 		["he"+"llo"](){
   			return "Hi";
  		}
	}
	obj.hello();  //"Hi"
    
    // 注意：属性的简洁表示法和属性名表达式不能同时使用，否则会报错
    const hello = "Hello";
	const obj = {
 		[hello]
	};
	obj  // 报错 SyntaxError: Unexpected token }
    
    // 正确使用：
    const hello = "Hello";
	const obj = {
 		[hello+"2"]:"world"
	};
	obj  //{Hello2: "world"}
---

## 对象的拓展运算符
拓展运算符（...）用于取出参数对象所有可遍历属性然后拷贝到当前对象。	
### 基本用法

	let person = {name: "Amy", age: 15};
	let someone = { ...person };
	someone;  //{name: "Amy", age: 15}
---

### 用于合并两个对象

	let age = {age: 15};
	let name = {name: "Amy"};
	let person = {...age, ...name};
	person;  // {age: 15, name: "Amy"}
    
**注意：**	

	//自定义的属性和拓展运算符对象里面属性的相同的时候：自定义的属性在拓展运算符后面，则拓展运算符对象内部同名的属性将被覆盖掉
	let person = {name: "Amy", age: 15};
	let someone = { ...person, name: "Mike", age: 17};
	someone;  //{name: "Mike", age: 17}
    
    // 自定义的属性在拓展运算度前面，则变成设置新对象默认属性值
    let person = {name: "Amy", age: 15};
	let someone = {name: "Mike", age: 17, ...person};
	someone;  //{name: "Amy", age: 15}
   
	// 拓展运算符后面是空对象，没有任何效果也不会报错
    let a = {...{}, a: 1, b: 2};
	a;  //{a: 1, b: 2}
    
    // 拓展运算符后面是null或者undefined，没有效果也不会报错
    let b = {...null, ...undefined, a: 1, b: 2};
	b;  //{a: 1, b: 2}
---
    
## 对象的新方法
**Object.assign(target, source_1, ...)**	

	// 用于将源对象的所有可枚举属性复制到目标对象中
    let target = {a: 1};
	let object2 = {b: 2};
	let object3 = {c: 3};
	Object.assign(target,object2,object3); // 第一个参数是目标对象，后面的参数是源对象
	target;  // {a: 1, b: 2, c: 3}
    
    // 如果目标对象和源对象有同名属性，或者多个源对象有同名属性，则后面的属性会覆盖前面的属性
    let target = {a: 1};
	let object2 = {b: 2};
	let object3 = {c: 3};
	let object4 = {b: 4};
	let object5 = {a: 5};
	Object.assign(target, object2, object3, object4, object5);
	console.log(target); // {a:5, b:4, c:3}
    
    // 如果该函数只有一个参数，当参数为对象时，直接返回该对象；当参数不是对象时，会先将参数转为对象然后返回
    let a = 3;
	console.log(typeof(a)); // number
	let b = Object.assign(a);
	console.log(typeof(b)); // object
    
    // 因为 null 和 undefined 不能转化为对象，所以会报错:
    Object.assign(null);       // TypeError: Cannot convert undefined or null to object
	Object.assign(undefined);  // TypeError: Cannot convert undefined or null to object
    
	// 当参数不止一个时，null 和 undefined 不放第一个，即不为目标对象时，会跳过 null 和 undefined ，不报错
	Object.assign(1,undefined);  // Number {1}
	Object.assign({a: 1},null);  // {a: 1}
 
	Object.assign(undefined,{a: 1});  // TypeError: 	Cannot convert undefined or null to object
    
*注意：*	

	// assign 的属性拷贝是浅拷贝:
    let sourceObj = { a: { b: 1}};
	let targetObj = {c: 3};
	Object.assign(targetObj, sourceObj);
	targetObj.a.b = 2;
	sourceObj.a.b;  // 2; 对目标对象进行处理影响了源对象
    
    // 同名属性替换
    targetObj = { a: { b: 1, c:2}};
	sourceObj = { a: { b: "hh"}};
	Object.assign(targetObj, sourceObj);
	targetObj;  // {a: {b: "hh"}}
    
    // 数组的处理
    Object.assign([2,3], [5]);  // [5,3]
    // 这是因为Object.assign()函数会将数组处理成对象，所以先将 [2,3] 转为 {0:2,1:3} ，将[5] 转为 {0:5} 然后再进行属性复制，所以源对象的 0 号属性覆盖了目标对象的 0 号属性
---

**Object.is(value1, value2)**	

	// 用来比较两个值是否严格相等，与（===）基本类似
    Object.is("q","q");      // true
	Object.is(1,1);          // true
	Object.is([1],[1]);      // false
	Object.is({q:1},{q:1});  // false
    
    // 与（===）的区别:
    // 1. +0不等于-0
    Object.is(+0,-0);  //false
	+0 === -0  //true
    
    // 2. NaN等于本身
    Object.is(NaN,NaN); //true
	NaN === NaN  //false
---

# 10. ES6 数组
## 数组创建
### Array.of()
将参数中所有值作为元素形成数组。

	console.log(Array.of(1,2,3,4)); // (4) [1, 2, 3, 4]
    // 参数可以为不同的类型
	console.log(Array.of(1, 'true', "Tom", {name:"John"})); // (4) [1, "true", "Tom", {…}]
---

### Array.from(arrayLike, mapFn, thisArg)
将类数组对象或可迭代对象转化为数组，返回值为转换后的数组。

	// 参数为数组,返回与原数组一样的数组
	console.log(Array.from([1, 2])); // [1, 2]
 
	// 参数含空位
	console.log(Array.from([1, , 3])); // [1, undefined, 3]
---
#### 参数
**arrayLike**	

	// 想要转换的类数组对象或可迭代对象
    console.log(Array.from([1, 2, 3])); // [1, 2, 3]
    
    let set = new Set([1,2,3,3,5,6,6,8,5]); // 去重
	let array = Array.from(set); // 转换为数组
	console.log(array); // (6) [1, 2, 3, 5, 6, 8]
    
**mapFn**	

	// 可选，map 函数，用于对每个元素进行处理，放入数组的是处理后的元素
    console.log(Array.from([1, 2, 3], (n) => n * 2)); // [2, 4, 6]
    
    function f(n){
  		return n * 2;
	}
	console.log(Array.from([1, 2, 3], (n) => f(n))); // [2, 4, 6]
    
**thisArg**	

	// 可选，用于指定 map 函数执行时的 this 对象
    let map = {
    	do: function(n) {
        	return n * 2;
    	}
	}
	let arrayLike = [1, 2, 3];
	console.log(Array.from(arrayLike, function (n){
    	return this.do(n);
	}, map)); // [2, 4, 6]
    
    function f(n){
  		return n * 2;
	}
	console.log(Array.from([1, 2, 3], function(n){return this(n)}, f)); // [2, 4, 6]
---
    
#### 类数组对象	

	// 一个类数组对象必须含有 length 属性，且元素属性名必须是数值或者可转换为数值的字符,而元素属性值可为任意类型
	let arr = Array.from({
  		0: '1',
  		1: '2',
  		2: 3,
  		length: 3
	});
	console.log(arr); // ['1', '2', 3]
    
    // 转换时，从 0 开始， 1， 2， 3, ... 依次向下读，在 length 长度范围内读到数字几放到第几位
    let array = Array.from({
  		0: 1,
  		2: 2,
  		1: 3,
  		length: 3
	});
	console.log(array); // [1, 3, 2]
    // 若读到的位数不足长度 length 则用 undefined 补齐
    let array = Array.from({
  		0: 1,
  		1: 2,
  		3: 3,
  		length: 3
	});
	console.log(array); // [1, 2, undefined]
    
    // 若 length 小于给出长度则只转换 length 个元素
    let array = Array.from({
  		0: 1,
  		1: 2,
  		2: 3,
  		length: 2
	});
	console.log(array); // [0,1]
    
    // 没有 length 属性,则返回空数组
    let array = Array.from({
  		0: '1',
  		1: '2',
  		2: 3,
	});
	console.log(array); // []
    
    // 元素属性名不为数值且无法转换为数值，返回长度为 length 元素值为 undefined 的数组  
	let array1 = Array.from({
  		a: 1,
  		b: 2,
  		length: 2
	});
	console.log(array1); // [undefined, undefined]
    
    let array2 = Array.from({
  		0: 1,
  		1: 2,
  		a: 3,
  		length: 3
	});
	console.log(array2); // [1, 2, undefined]
---

#### 转换可迭代对象

	// 转换 map
    let map = new Map();
	map.set('key0', 'value0');
	map.set('key1', 'value1');
	console.log(Array.from(map)); 
    // (2) [Array(2), Array(2)]
    // 0: (2) ["key0", "value0"]
    // 1: (2) ["key1", "value1"]
    
    // 转换 set
    let arr = [1, 2, 3, 2, 4, 4, 5];
	let set = new Set(arr);
	console.log(Array.from(set)); // (5) [1, 2, 3, 4, 5]
    
    // 转换字符串
    let str = 'abc';
	console.log(Array.from(str)); // ["a", "b", "c"]
---

## 扩展的方法
### 查找
**find()**	

	// 查找数组中符合条件的元素,若有多个符合条件的元素，则返回第一个元素
    let arr = Array.of(1, 2, 3, 4);
	console.log(arr.find(n => n > 2)); // 3
    
    // 数组空位处理为 undefined
	console.log([, 1].find(n => true)); // undefined
*不太理解这里的 true 是什么*	

---

**findIndex()**

	// 查找数组中符合条件的元素索引，若有多个符合条件的元素，则返回第一个元素索引
    let arr = Array.of(1, 2, 5, 4);
	console.log(arr.findIndex(n => n > 2)); // 2
    
    // 数组空位处理为 undefined
	console.log([, 1].findIndex(n => true)); //0
*同样不太理解这里的 true 是什么*	

---

### 填充
**fill()**	

	// 将一定索引范围的数组元素内容填充为单个指定的值
    let arr = Array.of(1, 2, 3, 4);
	// 参数1：用来填充的值
	// 参数2：被填充的起始索引
	// 参数3(可选)：被填充的结束索引，默认为数组末尾
	console.log(arr.fill(0,1,3)); // [1, 0, 0, 4]
    console.log(arr.fill(0,1)); // [1, 0, 0, 0]
    // 填充的范围包括开始索引，但不包括结束索引
---

**copyWithin()**	

	// 将一定索引范围的数组元素修改为此数组另一指定索引范围的元素
    let arr = Array.of(1, 2, 3, 4);
    // 参数1：被修改的起始索引,修改长度即为覆盖长度
	// 参数2：被用来覆盖的数据的起始索引
	// 参数3(可选)：被用来覆盖的数据的结束索引，默认为数组末尾
	console.log(arr.copyWithin(2,0,2)); // [1, 2, 1, 2]
    
    // 参数1为负数表示从倒数第参数1位开始
    let arr = Array.of(1, 2, 3, 4);
	console.log(arr.copyWithin(-3,0,2)); // [1, 1, 2, 4]
    // 最后一位为倒数第一位...
    
    // 元素为空，则覆盖时也用空元素覆盖
    console.log([1, 2, ,4].copyWithin(0, 2, 4)); // [, 4, , 4]
---

### 遍历
**entries()**	

	// 遍历键值对
    // 使用 for... of 循环
    for(let [key, value] of ['a', 'b'].entries()){
    	console.log(key, value);
	}
	// 0 "a"
	// 1 "b"
    
    // 不适用 for... of 循环
    let entries = ['a', 'b'].entries();
	console.log(entries.next().value); // [0, "a"]
	console.log(entries.next().value); // [1, "b"]
    
    // 数组含空位
	console.log([...[,'a'].entries()]); // [[0, undefined], [1, "a"]]
*不太理解这里 ... 运算符复制的作用*

---

**keys()**	

	// 遍历键名
    for(let key of ['a', 'b'].keys()){
    	console.log(key);
	}
	// 0
	// 1
 
	// 数组含空位
	console.log([...[,'a'].keys()]); // [0, 1]
---

**values()**	

	// 遍历键值
    for(let value of ['a', 'b'].values()){
    	console.log(value);
	}
	// "a"
	// "b"
 
	// 数组含空位
	console.log([...[,'a'].values()]); // [undefined, "a"]
---

### 包含
**includes()**	

	// 数组是否包含指定值
    // 注意：与 Set 和 Map 的 has 方法区分；Set 的 has 方法用于查找值；Map 的 has 方法用于查找键名
    // 参数1：包含的指定值
    // 参数2：可选，搜索的起始索引，默认为0
    let arr = [1, 2, 3];
	console.log(arr.includes(1)); // true
	console.log(arr.includes(1, 1)); // false

	// NaN的判断
	let arr1 = [1, NaN, 3];
	console.log(arr1.includes(NaN)); // true
---

### 嵌套数组转一维数组
**flat()**	

	let arr1 = [1, [2, 3]];
    console.log(arr1.flat()); // [1, 2, 3]
    
    // 指定转换的嵌套层数
    let arr2 = [1, [2, [3, [4, [5, [6 ,[7, 8]]]]]]];
    console.log(arr2.falt(2)); // (4) [1, 2, 3, Array(2)]
    console.log(arr2.flat(5)); // (7) [1, 2, 3, 4, 5, 6, Array(2)]
    
    // 不管嵌套多少层
    let arr3 = [1, [2, [3, [4, [5, [6 ,[7, 8]]]]]]];
	console.log(arr3.flat(Infinity));
    
    // 自动跳过空位
	console.log([1, [2, , 3]].flat()); // [1, 2, 3]
    
**flatMap()**	

	// 先对数组中每个元素进行了的处理，再对数组执行 flat() 方法
    // 参数1：遍历函数，该遍历函数可接受3个参数：当前元素、当前元素索引、原数组
	// 参数2：指定遍历函数中 this 的指向
	console.log([1, 2, 3].flatMap(n => [n * 2])); // [2, 4, 6]
*不太懂 flatMap() 参数1 可接受的三种参数什么意思*
    
---

## 数组缓冲区
数组缓冲区是内存中的一段地址。	
定型数组的基础。	
实际字节数在创建时确定，之后只可修改其中的数据，不可修改大小。	
**创建数组缓冲区**

	// 通过构造函数创建
	let buffer = new ArrayBuffer(10);
	console.log(buffer.byteLength); // 10

	// 分割已有数组缓冲区
	let buffer1 = buffer.slice(1,3);
	console.log(buffer1.byteLength); // 2
---

**视图**	
视图是用来操作内存的接口。	
视图可以操作数组缓冲区或缓冲区字节的子集,并按照其中一种数值数据类型来读取和写入数据。	
DataView 类型是一种通用的数组缓冲区视图,其支持所有8种数值型数据类型。

	// 创建，默认 DataView 可操作数组缓冲区全部内容
	let buffer = new ArrayBuffer(10);
		dataview = new DataView(buffer);
	dataview.setInt8(0,1);
	dataview.setInt8(2,3);
	console.log(dataview.getInt8(0)); // 1
	console.log(dataview.getInt8(2)); // 3
    
    // 通过设定偏移量(参数2)与长度(参数3)指定 DataView 可操作的字节范围
    let buffer1 = new ArrayBuffer(10);
	dataView1 = new DataView(buffer1, 8, 2);
	dataView2 = new DataView(buffer1);
	// dataView1 的偏移量为 8，即从第 8 字节开始操作
	dataView1.setInt8(0,6); // 设置第 8 字节
	dataView1.setInt8(1,5); // 设置第 9 字节
    console.log(dataView2.getInt8(8)); // 6
	console.log(dataView2.getInt8(9)); // 5
---

## 定型数组
数组缓冲区的特定类型的视图。	
可以强制使用特定的数据类型，而不是使用通用的 DataView 对象来操作数组缓冲区。
**创建**

	// 通过数组缓冲区生成
    let buffer = new ArrayBuffer(10),
    	view = new Int8Array(buffer);
	console.log(view.byteLength); // 10
    
    // 通过构造函数
    let view = new Int32Array(10);
	console.log(view.byteLength); // 40
	console.log(view.length);     // 10
 
	// 不传参则默认长度为0
	// 在这种情况下数组缓冲区分配不到空间，创建的定型数组不能用来保存数据
	let view1 = new Int32Array();
	console.log(view1.byteLength); // 0
	console.log(view1.length);     // 0
 
	// 可接受参数包括定型数组、可迭代对象、数组、类数组对象
	let arr = Array.from({
  		0: '1',
  		1: '2',
  		2: 3,
  		length: 3
	});
	let view2 = new Int16Array([1, 2]),
    	view3 = new Int32Array(view2),
    	view4 = new Int16Array(new Set([1, 2, 3])),
    	view5 = new Int16Array([1, 2, 3]),
    	view6 = new Int16Array(arr);
	console.log(view2 .buffer === view3.buffer); // false
	console.log(view4.byteLength); // 6
	console.log(view5.byteLength); // 6
	console.log(view6.byteLength); // 6
    
**注意要点**

	// length 属性不可写，如果尝试修改这个值，在非严格模式下会直接忽略该操作，在严格模式下会抛出错误
    let view = new Int16Array([1, 2]);
	view.length = 3;
	console.log(view.length); // 2
    
    // 定型数组可使用 entries()、keys()、values()进行迭代
    let view = new Int16Array([1, 2]);
	for(let [k, v] of view.entries()){
    	console.log(k, v);
	}
	// 0 1
	// 1 2
    
    // find() 等方法也可用于定型数组，但是定型数组中的方法会额外检查数值类型是否安全,也会通过 Symbol.species 确认方法的返回值是定型数组而非普通数组。concat() 方法由于两个定型数组合并结果不确定，故不能用于定型数组；另外，由于定型数组的尺寸不可更改,可以改变数组的尺寸的方法，例如 splice() ，不适用于定型数组
    let view = new Int16Array([1, 2]);
	view.find((n) > 1); // 2
    
    // 所有定型数组都含有静态 of() 方法和 from() 方法,运行效果分别与 Array.of() 方法和 Array.from() 方法相似,区别是定型数组的方法返回定型数组,而普通数组的方法返回普通数组
    let view = Int16Array.of(1, 2);
	console.log(view instanceof Int16Array); // true
    
    // 定型数组不是普通数组，不继承自 Array 。
	let view = new Int16Array([1, 2]);
	console.log(Array.isArray(view)); // false
    
**定型数组中增加了 set() 与 subarray() 方法。 set() 方法用于将其他数组复制到已有定型数组, subarray() 用于提取已有定型数组的一部分形成新的定型数组。**

	// set 方法
	// 参数1：一个定型数组或普通数组
	// 参数2：可选，偏移量，开始插入数据的位置，默认为0
	let view= new Int16Array(4);
	view.set([1, 2]);
	view.set([3, 4], 2);
	console.log(view); // [1, 2, 3, 4]
 
	// subarray 方法
	// 参数1：可选，开始位置
	// 参数2：可选，结束位置(不包含结束位置)
	let view= new Int16Array([1, 2, 3, 4]), 
    	subview1 = view.subarray(), 
    	subview2 = view.subarray(1), 
    	subview3 = view.subarray(1, 3);
	console.log(subview1); // [1, 2, 3, 4]
	console.log(subview2); // [2, 3, 4]
	console.log(subview3); // [2, 3]

## 扩展运算符
	// 复制数组
    let arr = [1, 2],
    	arr1 = [...arr];
	console.log(arr1); // [1, 2]
 
	// 数组含空位
	let arr2 = [1, , 3],
    	arr3 = [...arr2];
	console.log(arr3); [1, undefined, 3]
    ------------------------------------
    
    // 合并数组
	console.log([...[1, 2],...[3, 4]]); // [1, 2, 3, 4]
---

# 11. ES6 函数
## 函数参数的扩展
### 默认参数
	// 基本用法
    function fn(name,age=17){
 		console.log(name+","+age);
	}
	fn("Amy",18);  // Amy,18
	fn("Amy","");  // Amy,
	fn("Amy");     // Amy,17
    ----------------------------------------------------
    
    // 注意：使用函数默认参数时，不允许有同名参数
    // 不报错
	function fn(name,name){
 		console.log(name);
	}
 
	// 报错
	//SyntaxError: Duplicate parameter name not allowed in this context
	function fn(name,name,age=17){
 		console.log(name+","+age);
	}
    ----------------------------------------------------
    
    // 只有在未传递参数，或者参数为 undefined 时，才会使用默认参数，null 值被认为是有效的值传递
    function fn(name,age=17){
    	console.log(name+","+age);
	}
	fn("Amy",null); // Amy,null
    ----------------------------------------------------
    
    // 函数参数默认值存在暂时性死区，在函数参数默认值表达式中，还未初始化赋值的参数值无法作为其他参数的默认值
    function f(x,y=x){
    	console.log(x,y);
	}
	f(1);  // 1 1
 
	function f(x=y){
    	console.log(x);
	}
	f();  // ReferenceError: y is not defined
---    
    
### 不定参数
	// 不定参数用来表示不确定参数个数，形如，...变量名，由...加上一个具名参数标识符组成。具名参数只能放在参数组的最后，并且有且只有一个不定参数
    // 基本用法
    function f(...values){
    	console.log(values.length);
	}
	f(1,2);      // 2
	f(1,2,3,4);  // 4
---

## 箭头函数
箭头函数提供了一种更加简洁的函数书写方式。基本语法是：	
参数 => 函数体

	// 基本用法
    var f = v => v;
	//等价于
	var f = function(a){
 		return a;
	}
	f(1);  //1
    
    // 当箭头函数没有参数或者有多个参数，要用 () 括起来
    var f = (a,b) => a+b;
	f(6,2);  //8
    
    // 当箭头函数函数体有多行语句，用 {} 包裹起来，表示代码块，当只有一行语句，并且需要返回结果时，可以省略 {} , 结果会自动返回
    var f = (a,b) => {
 			let result = a+b;
 			return result;
		}
	f(6,2);  // 8
    
    // 当箭头函数要返回对象的时候，为了区分于代码块，要用 () 将对象包裹起来
    // 报错
	var f = (id,name) => {id: id, name: name};
	f(6,2);  // SyntaxError: Unexpected token 
	// 不报错
	var f = (id,name) => ({id: id, name: name});
	f(6,2);  // {id: 6, name: 2}

**注意：没有 this、super、arguments 和 new.target 绑定**
	
    var func = () => {
  	// 箭头函数里面没有 this 对象，
  	// 此时的 this 是外层的 this 对象，即 Window 
  			console.log(this)
		}
	func(55)  // Window 
 
	var func = () => {    
  			console.log(arguments)
		}
	func(55);  // ReferenceError: arguments is not defined
 ---
 
 **箭头函数体中的 this 对象，是定义函数时的对象，而不是使用函数时的对象**
 
 	function fn(){
  		setTimeout(()=>{
    	// 定义时，this 绑定的是 fn 中的 this 对象
    		console.log(this.a);
  			},0)
		}
	var a = 20;
	// fn 的 this 对象为 {a: 19}
	fn.call({a: 18});  // 18
 	// 不可以作为构造函数，也就是不能使用 new 命令，否则会报错
*这里的 this 部分不太懂*

---

# 12. ES6迭代器
## Iterator
Iterator 是 ES6 引入的一种新的遍历机制，迭代器有两个核心概念：	
* 迭代器是一个统一的接口，它的作用是使各种数据结构可被便捷的访问，它是通过一个键为Symbol.iterator 的方法来实现。
* 迭代器是用于遍历数据结构元素的指针（如数据库中的游标）。
---

### 迭代过程
* 通过 Symbol.iterator 创建一个迭代器，指向当前数据结构的起始位置	
* 随后通过 next 方法进行向下迭代指向下一个位置， next 方法会返回当前位置的对象，对象包含了 value 和 done 两个属性， value 是当前属性的值， done 用于判断是否遍历结束	
* 当 done 为 true 时则遍历结束	

**示例如下：**	

	const items = ["zero", "one", "two"];
	const it = items[Symbol.iterator]();
 
	console.log(it.next());
	// {value: "zero", done: false}
	console.log(it.next());
	// {value: "one", done: false}
	console.log(it.next());
	// {value: "two", done: false}
	console.log(it.next());
	// {value: undefined, done: true}
    // 上例中，首先创建一个数组，然后通过 Symbol.iterator 方法创建一个迭代器，之后不断的调用 next 方法对数组内部项进行访问，当属性 done 为 true 时访问结束
---

**迭代器是协议（使用它们的规则）的一部分，用于迭代。该协议的一个关键特性就是它是顺序的：迭代器一次返回一个值。这意味着如果可迭代数据结构是非线性的（例如树），迭代将会使其线性化。**

---

### 可迭代的数据结构
**以下是可迭代的值：**	
* Array	
* String	
* Map	
* Set	
* Dom元素（正在进行中）
---

**Array**	

	// 数组 ( Array ) 和类型数组 ( TypedArray ) 他们是可迭代的
    for (let item of ["zero", "one", "two"]) {
  				console.log(item);
	}
	// output:
	// zero
	// one
	// two
 ---
 
**String**	

	// 字符串是可迭代的，但他们遍历的是 Unicode 码，每个码可能包含一个到两个的 Javascript 字符。
	for (const c of 'z\uD83D\uDC0A') {
    		console.log(c);
	}
	// output:
	// z
	// \uD83D\uDC0A
---

**Map**

	// Map 主要是迭代它们的 entries ，每个 entry 都会被编码为 [key, value] 的项， entries 是以确定的形式进行迭代，其顺序是与添加的顺序相同
	const map = new Map();
	map.set(0, "zero");
	map.set(1, "one");
 
	for (let item of map) {
  			console.log(item);
	}
	// output:
	// [0, "zero"]
	// [1, "one"]
注意： WeakMaps 不可迭代

---

**Set**

	// Set 是对其元素进行迭代，迭代的顺序与其添加的顺序相同
	const set = new Set();
	set.add("zero");
	set.add("one");
 
	for (let item of set) {
  		console.log(item);
	}
	// output:
	// zero
	// one
注意： WeakSets 不可迭代

---

**arguments**

	// arguments 目前在 ES6 中使用越来越少，但也是可遍历的
	function args() {
  		for (let item of arguments) {
    		console.log(item);
  		}
	}
	args("zero", "one");
	// output:
	// zero
	// one
---

**普通对象不可迭代**	

	// 普通对象是由 object 创建的，不可迭代：
	// TypeError, obj is not iterable
	for (let item of {}) { 
  		console.log(item);
	}
## for...of 循环
 for...of 是 ES6 新引入的循环，用于替代 for..in 和 forEach() ，并且支持新的迭代协议。它可用于迭代常规的数据类型，如 Array 、 String 、 Map 和 Set 等等。
 
### 迭代常规数据类型
**Array**

	const nums = ["zero", "one", "two"];
 
	for (let num of nums) {
  		console.log(num); // zero, one ,two
	}
    
	// TypedArray
	const typedArray1 = new Int8Array(6);
	typedArray1[0] = 10;
	typedArray1[1] = 11;
 
	for (let item of typedArray1) {
  		console.log(item); // 10, 11, (4) 0
	}
---

**String**

	const str = "zero";
 
	for (let item of str) {
  		console.log(item); // z, e, r, o
	}
---

**Map**

	let myMap = new Map();
	myMap.set(0, "zero");
	myMap.set(1, "one");
	myMap.set(2, "two");
 
	// 遍历 key 和 value
	for (let [key, value] of myMap) {
  		console.log(key + " = " + value); 
	}
    // 0 = zero
	// 1 = one
	// 2 = two
    
	for (let [key, value] of myMap.entries()) {
  		console.log(key + " = " + value);
	}
    // 0 = zero
	// 1 = one
	// 2 = two
    
	// 只遍历 key
	for (let key of myMap.keys()) {
 		console.log(key);
	}
    // 0
    // 1
    // 2
 
	// 只遍历 value
	for (let value of myMap.values()) {
  		console.log(value);
	}
    // zero
    // one
    // two
---

**Set**

	let mySet = new Set();
	mySet.add("zero");
	mySet.add("one");
	mySet.add("two");
 
	// 遍历整个 set
	for (let item of mySet) {
  		console.log(item);
	}
    // zero
	// one
	// two
 
	// 只遍历 key 值
	for (let key of mySet.keys()) {
  		console.log(key);
	}
    // zero
	// one
	// two
    
	// 只遍历 value
	for (let value of mySet.values()) {
  		console.log(value);
	}
    // zero
	// one
	// two
    
	// 遍历 key 和 value ，会发现，两者会相等
	for (let [key, value] of mySet.entries()) {
  		console.log(key + " = " + value);
	}
    // zero = zero
	// one = one
	// two = two
---

### 可迭代的数据结构
**of 操作数必须是可迭代，这意味着如果是普通对象则无法进行迭代。如果数据结构类似于数组的形式，则可以借助 Array.from() 方法进行转换迭代。**

	const arrayLink = {length: 2, 0: "zero", 1: "one"}
    
	// 报 TypeError 异常
	for (let item of arrayLink) {
  		console.log(item);
	}
 
	// 正常运行
	for (let item of Array.from(arrayLink)) {
  		console.log(item);
	}
    // output:
	// zero
	// one
---

**let 、const 和 var 用于 for..of**	

	// 如果使用 let 和 const ，每次迭代将会创建一个新的存储空间，这可以保证作用域在迭代的内部
    const nums = ["zero", "one", "two"];
 
	for (const num of nums) {
  		console.log(num);
	}
	// 报 ReferenceError
	console.log(num);
    // 原因是 num 的作用域只在循环体内部，外部无效
    -----------------------------------------------------
    
    // 使用 var 则不会出现上述情况，因为 var 会作用于全局，迭代将不会每次都创建一个新的存储空间
    const nums = ["zero", "one", "two"];
 
	for (var num of nums) {
  		console.log(num);
	}
    // zero
    // one
    // two
	
	console.log(num); // output: two
---

# 13. ES6 class 类
## 概述
在ES6中，class (类)作为对象的模板被引入，可以通过 class 关键字定义类。	
class 的本质是 function。	
它可以看作一个语法糖，让对象原型的写法更加清晰、更像面向对象编程的语法。		

---

## 基础用法
### 类定义
类表达式可以为匿名或命名。

	// 匿名类
	let Example = class {
    	constructor(a) {
        	this.a = a;
    	}
	}
	// 命名类
	let Example = class Example {
    	constructor(a) {
        	this.a = a;
    	}
	}
---

### 类声明
	class Example {
    	constructor(a) {
        	this.a = a;
    	}
	}

**注意要点：不可重复声明。**

	class Example{}
	class Example{}
	// Uncaught SyntaxError: Identifier 'Example' has already been declared
 
	let Example1 = class{}
	class Example1{}
	// Uncaught SyntaxError: Identifier 'Example' has already been declared
---

### 注意要点
* 类定义不会被提升，这意味着，必须在访问前对类进行定义，否则就会报错。	
* 类中方法不需要 function 关键字。	
* 方法间不能加分号。
---

### 类的主体
#### 属性
**prototype**	
ES6 中，prototype 仍旧存在，虽然可以直接自类中定义方法，但是其实方法还是定义在 prototype 上的。 覆盖方法 / 初始化时添加方法。

	Example.prototype={
    	//methods
	}
    
	// 添加方法
	Object.assign(Example.prototype,{
    	//methods
	})
---

**静态属性**	
静态属性：class 本身的属性，即直接定义在类内部的属性（ Class.propname ），不需要实例化。 ES6 中规定，Class 内部只有静态方法，没有静态属性。

	class Example {
	// 新提案
    	static a = 2;
	}
	// 目前可行写法
	Example.b = 2;
---

**公共属性**

	class Example{}
	Example.prototype.a = 2;
---

**实例属性**	
实例属性：定义在实例对象（ this ）上的属性。

	class Example {
    	a = 2;
    	constructor () {
        	console.log(this.a);
    	}
	}
---

**name 属性**	
返回跟在 class 后的类名(存在时)。

	let Example=class Exam {
    	constructor(a) {
        	this.a = a;
    	}
	}
	console.log(Example.name); // Exam
 
	let Example=class {
    	constructor(a) {
        	this.a = a;
    	}
	}
	console.log(Example.name); // Example
---

#### 方法
**constructor 方法**	
constructor 方法是类的默认方法，创建类的实例化对象时被调用。

	class Example{
    	constructor(){
      	console.log('我是constructor');
    	}
	}
	new Example(); // 我是constructor
---

返回对象

	class Test {
    	constructor(){
        	// 默认返回实例对象 this
    	}
	}
	console.log(new Test() instanceof Test); // true
 
	class Example {
    	constructor(){
        	// 指定返回对象
        	return new Test();
    	}
	}
	console.log(new Example() instanceof Example); // false
---

**静态方法**

	class Example{
    	static sum(a, b) {
        	console.log(a+b);
    	}
	}
	Example.sum(1, 2); // 3
---

**原型方法**

	class Example {
    	sum(a, b) {
        	console.log(a + b);
    	}
	}
	let exam = new Example();
	exam.sum(1, 2); // 3
    
**实例方法**

	class Example {
    	constructor() {
        	this.sum = (a, b) => {
            	console.log(a + b);
        	}
    	}
	}
---

### 类的实例化
#### new

class 的实例化必须通过 new 关键字。

	class Example {}
 
	let exam1 = Example(); 
	// Class constructor Example cannot be invoked without 'new'
---

#### 实例化对象
共享原型对象

	class Example {
    	constructor(a, b) {
        	this.a = a;
        	this.b = b;
        	console.log('Example');
    	}
    	sum() {
        	return this.a + this.b;
    	}
	}
	let exam1 = new Example(2, 1);
	let exam2 = new Example(3, 1);
	console.log(exam1._proto_ == exam2._proto_); // true
 
	exam1._proto_.sub = function() {
    	return this.a - this.b;
	} // 我理解共享是说，这种定义函数，所有实例化对象通用
	console.log(exam1.sub()); // 1
	console.log(exam2.sub()); // 2
---

### decorator
decorator 是一个函数，用来修改类的行为，在代码编译时产生作用。	
#### 类修饰
**一个参数**	
第一个参数 target，指向类本身。

	function testable(target) {
    	target.isTestable = true;
	}
	// testable
	class Example {}
	Example.isTestable; // true
---

**多个参数——嵌套实现**

	function testable(isTestable) {
    	return function(target) {
        	target.isTestable=isTestable;
    	}
	}
	// testable(true)
	class Example {}
	Example.isTestable; // true    
---

实例属性	
上面两个例子添加的是静态属性，若要添加实例属性，在类的 prototype 上操作即可。

#### 方法修饰
3个参数：target（类的原型对象）、name（修饰的属性名）、descriptor（该属性的描述对象）。

	class Example {
    	writable
    	sum(a, b) {
        	return a + b;
    	}
	}
	function writable(target, name, descriptor) {
    	descriptor.writable = false;
    	return descriptor; // 必须返回
	}
修饰器执行顺序	
由外向内进入，由内向外执行。

	class Example {
    	logMethod(1)
    	logMthod(2)
    	sum(a, b){
        	return a + b;
    	}
	}
	function logMethod(id) {
    	console.log('evaluated logMethod'+id);
    	return (target, name, desctiptor) => console.log('excuted         logMethod '+id);
	}
	// evaluated logMethod 1
	// evaluated logMethod 2
	// excuted logMethod 2
	// excuted logMethod 1
*decorator 这里有点模糊，我理解的是，整个‘修饰’所起的作用就是告诉计算机这个类的一些属性。描述，向计算机‘描述’这个类。*

---   

## 封装与继承
### getter / setter
**定义**

	class Example{
    	constructor(a, b) {
        	this.a = a; // 实例化时调用 set 方法
        	this.b = b;
    	}
    	get a(){
        	console.log('getter');
        	return this.a;
    	}
    	set a(a){
        	console.log('setter');
        	this.a = a; // 自身递归调用
    	}
	}
	let exam = new Example(1,2); // 不断输出 setter ，最终导致 RangeError
	class Example1{
    	constructor(a, b) {
        	this.a = a;
        	this.b = b;
    	}
    	get a(){
        	console.log('getter');
        	return this._a;
    	}
    	set a(a){
        	console.log('setter');
        	this._a = a;
    	}
	}
	let exam1 = new Example1(1,2); // 只输出 setter , 不会调用 getter 方法
	console.log(exam._a); // 1, 可以直接访问
    console.log(exam.a); // getter, 1； 这里调用了 getter 函数
---

**getter 不可单独出现**

	class Example {
    	constructor(a) {
        	this.a = a; 
    	}
    	get a() {
        	return this.a;
    	}
	}
	let exam = new Example(1); // Uncaught TypeError: Cannot set property // a of #<Example> which has only a getter
--

**getter 与 setter 必须同级出现**	

	class Father {
    	constructor(){}
    	get a() {
        	return this._a;
    	}
	}
	class Child extends Father {
    	constructor(){
        	super();
    	}
    	set a(a) {
        	this._a = a;
    	}
	}
	let test = new Child();
	test.a = 2;
	console.log(test.a); // undefined
 
	class Father1 {
    	constructor(){}
    	// 或者都放在子类中
    	get a() {
        	console.log('getter');
        	return this._a;
    	}
    	set a(a) {
        	console.log('setter');
        	this._a = a;
    	}
	}
	class Child1 extends Father1 {
    	constructor(){
        	super();
    	}
	}
	let test1 = new Child1();
	test1.a = 2;
	console.log(test1.a); 
    // setter
    // getter
    // 2
    // 先调用 set 赋值， 再调用 get 读值
---

### extends
通过 extends 实现类的继承。

	class Child extends Father { ... }
---

### super
子类 constructor 方法中必须有 super ，且必须出现在 this 之前。

	class Father {
    	constructor() {}
	}
	class Child extends Father {
    	constructor() {}
    	// or 
    	// constructor(a) {
        	// this.a = a;
        	// super();
    	// }
	}
	let test = new Child(); 
    // Uncaught ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
---

调用父类构造函数,只能出现在子类的构造函数。

	class Father {
    	test(){
        	return 0;
    	}
    	static test1(){
        	return 1;
    	}
	}
	class Child extends Father {
    	constructor(){
        	super();
    	}
	}
	class Child1 extends Father {
    	test2() {
        	super(); // Uncaught SyntaxError: 'super' keyword unexpected here
    	}
	}
    
调用父类方法, super 作为对象，在普通方法中，指向父类的原型对象，在静态方法中，指向父类

	class Child2 extends Father {
    	constructor(){
        	super();
        	// 调用父类普通方法
        	console.log(super.test()); // 0
    	}
    	static test3(){
        	// 调用父类静态方法
        	return super.test1()+2;
    	}
	}
	Child2.test3(); // 3
---

### 注意要点
不可继承常规对象。

	var Father = {
    	// ...
	}
	class Child extends Father {
     	// ...
	}
	// Uncaught TypeError: Class extends value #<Object> is not a constructor or null
 
	// 解决方案
	Object.setPrototypeOf(Child.prototype, Father);
---

# 14. ES6模块
## 概述
ES6 的模块化分为导出（export）与导入（import）两个模块。	
**特点：**	
* ES6 的模块自动开启严格模式，不管你有没有在模块头部加上 use strict;。	
* 模块中可以导入和导出各种类型的变量，如函数，对象，字符串，数字，布尔值，类等。	
* 每个模块都有自己的上下文，每一个模块内声明的变量都是局部变量，不会污染全局作用域。	
* 每一个模块只加载一次（是单例的）， 若再去加载同目录下同文件，直接从内存中读取。

---

## export 与 import
### 基本用法
模块导入导出各种类型的变量，如字符串，数值，函数，类。	
* 导出的函数声明与类声明必须要有名称（export default 命令另外考虑）。 	
* 不仅能导出声明还能导出引用（例如函数）。
* export 命令可以出现在模块的任何位置，但必需处于模块顶层。
* import 命令会提升到整个模块的头部，首先执行。	


	/*-----export [test.js]-----*/
	let myName = "Tom";
	let myAge = 20;
	let myfn = function(){
    	return "My name is" + myName + "! I'm '" + myAge + "years old."
	}
	let myClass =  class myClass {
    	static a = "yeah!";
	}
	export { myName, myAge, myfn, myClass }
 
	/*-----import [xxx.js]-----*/
	import { myName, myAge, myfn, myClass } from "./test.js";
	console.log(myfn());// My name is Tom! I'm 20 years old.
	console.log(myAge);// 20
	console.log(myName);// Tom
	console.log(myClass.a );// yeah!
建议使用大括号指定所要输出的一组变量写在文档尾部，明确导出的接口。	
函数与类都需要有对应的名称，导出文档尾部也避免了无对应名称。

---

### as 的用法
export 命令导出的接口名称，须和模块内部的变量有一一对应关系。	
导入的变量名，须和导出的接口名称相同，即顺序可以不一致。

	// 使用 as 重新定义导出的接口名称，隐藏模块内部的变量
	/*-----export [test.js]-----*/
	let myName = "Tom";
	export { myName as exportName }
 
	/*-----import [xxx.js]-----*/
	import { exportName } from "./test.js";
	console.log(exportName);// Tom 
	
    // 不同模块导出接口名称命名重复， 使用 as 重新定义变量名。
	/*-----export [test1.js]-----*/
	let myName = "Tom";
	export { myName }
	/*-----export [test2.js]-----*/
	let myName = "Jerry";
	export { myName }
	/*-----import [xxx.js]-----*/
	import { myName as name1 } from "./test1.js";
	import { myName as name2 } from "./test2.js";
	console.log(name1);// Tom
	console.log(name2);// Jerry
---

### import 命令的特点
**只读属性：**	
不允许在加载模块的脚本里面，改写接口的引用指向，即可以改写 import 变量类型为对象的属性值，不能改写 import 变量类型为基本类型的值。

	import {a} from "./xxx.js"
	a = {}; // error
 
	import {a} from "./xxx.js"
	a.foo = "hello"; // a = { foo : 'hello' }
---

**单例模式：**	
多次重复执行同一句 import 语句，那么只会执行一次，而不会执行多次。import 同一模块，声明不同接口引用，会声明对应变量，但只执行一次 import 。

	import { a } "./xxx.js";
	import { a } "./xxx.js";
	// 相当于 import { a } "./xxx.js";
 
	import { a } from "./xxx.js";
	import { b } from "./xxx.js";
	// 相当于 import { a, b } from "./xxx.js";
---

**静态执行特性：**	
import 是静态执行，所以不能使用表达式和变量。

	import { "f" + "oo" } from "methods";
	// error
	let module = "methods";
	import { foo } from module;
	// error
	if (true) {
  		import { foo } from "method1";
	} else {
  		import { foo } from "method2";
	}
	// error
---

### export default 命令
* 在一个文件或模块中，export、import 可以有多个，export default 仅有一个。	
* export default 中的 default 是对应的导出接口变量。	
* 通过 export 方式导出，在导入时要加{ }，export default 则不需要。	
* export default 向外暴露的成员，可以使用任意变量来接收。	
	

	var a = "My name is Tom!";
	export default a; // 仅有一个
	export default var c = "error"; 
	// error，default 已经是对应的导出变量，不能跟着变量声明语句
 
	import b from "./xxx.js"; // 不需要加{}， 使用任意变量接收
---

## 复合使用
*在同一模块使用 export 和 import ， 没弄明白怎么回事，无法验证。我理解这里的methods是一个模块，但是具体怎么操作的不太清楚。*	
export 与 import 可以在同一模块使用，使用特点：	
* 可以将导出接口改名，包括 default。	
* 复合使用 export 与 import ，也可以导出全部，当前模块导出的接口会覆盖继承导出的。


	export { foo, bar } from "methods";
 
	// 约等于下面两段语句，不过上面导入导出方式该模块没有导入 foo 与 bar
	import { foo, bar } from "methods";
	export { foo, bar };
 
	/* ------- 特点 1 --------*/
	// 普通改名
	export { foo as bar } from "methods";
	// 将 foo 转导成 default
	export { foo as default } from "methods";
	// 将 default 转导成 foo
	export { default as foo } from "methods";
 
	/* ------- 特点 2 --------*/
    // 我理解是导出全部
	export * from "methods";
---

# 15. ES6 Generator 函数
ES6 新引入了 Generator 函数，可以通过 yield 关键字，把函数的执行流挂起，为改变执行流程提供了可能，从而为异步编程提供解决方案。 
## Generator 函数组成
Generator 有两个区分于普通函数的部分：	
* 一是在 function 后面，函数名之前有个 * 	
* 二是函数内部有 yield 表达式		
其中 * 用来表示函数为 Generator 函数，yield 用来定义函数内部的状态。

	
    function* func(){
 		console.log("one");
 		yield '1';
 		console.log("two");
 		yield '2'; 
 		console.log("three");
 		return '3';
	}
---

## 执行机制
调用 Generator 函数和调用普通函数一样，在函数名后面加上()即可，但是 Generator 函数不会像普通函数一样立即执行，而是返回一个指向内部状态对象的指针，所以要调用遍历器对象Iterator 的 next 方法，指针就会从函数头部或者上一次停下来的地方开始执行。
	
    f.next();
	// one
	// {value: "1", done: false}
 
	f.next();
	// two
	// {value: "2", done: false}
 
	f.next();
	// three
	// {value: "3", done: true}
 
	f.next();
	// {value: undefined, done: true}
    
    // 第一次调用 next 方法时，从 Generator 函数的头部开始执行，先是打印了 one ,执行到 yield 就停下来，并将yield 后边表达式的值 '1'，作为返回对象的 value 属性值，此时函数还没有执行完， 返回对象的 done 属性值是 false
	// 第二次调用 next 方法时，同上步 
	// 第三次调用 next 方法时，先是打印了 three ，然后执行了函数的返回操作，并将 return 后面的表达式的值，作为返回对象的 value 属性值，此时函数已经结束，所以 done 属性值为true 。如果执行第三步时，没有 return 语句的话，就直接返回 {value: undefined, done: true}
	// 第四次调用 next 方法时， 此时函数已经执行完了，所以返回 value 属性值是 undefined ，done 属性值是 true 。
---

## 函数返回的遍历器对象的方法
### next 方法
 一般情况下，next 方法不传入参数的时候，yield 表达式的返回值是 undefined 。当 next 传入参数的时候，该参数会作为上一步yield的返回值
 
    function* sendParameter(){
    	console.log("strat");
    	var x = yield '2';
    	console.log("one:" + x);
        var y = yield '3';
    	console.log("two:" + y);
    	console.log("total:" + (x + y));
	}
    
    // next不传参
    var sendp1 = sendParameter();
	sendp1.next();
	// strat
	// {value: "2", done: false}
	sendp1.next();
	// one:undefined
	// {value: "3", done: false}
	sendp1.next();
	// two:undefined
	// total:NaN
	// {value: undefined, done: true}
    
	// next传参
	var sendp2 = sendParameter();
	sendp2.next(10);
	// strat
	// {value: "2", done: false}
	sendp2.next(20);
	// one:20
	// {value: "3", done: false}
	sendp2.next(30);
	// two:30
	// total:50
	// {value: undefined, done: true}
    
    // 除了使用 next ，还可以使用 for... of 循环遍历 Generator 函数生产的 Iterator 对象
---

### return 方法
return 方法返回给定值，并结束遍历 Generator 函数。	
return 方法提供参数时，返回该参数；不提供参数时，返回 undefined 。

	function* foo(){
    	yield 1;
    	yield 2;
    	yield 3;
	}
	var f = foo();
	f.next();
	// {value: 1, done: false}
	f.return("foo");
	// {value: "foo", done: true}
	f.next();
	// {value: undefined, done: true}
---

### throw 方法
	// throw 方法可以再 Generator 函数体外面抛出异常，再函数体内部捕获。
	var g = function* () {
  		try {
    		yield;
  		} catch (e) {
    		console.log('catch inner', e);
  		}
	};
 
	var i = g();
	i.next();
 
	try {
  		i.throw('a');
  		i.throw('b');
	} catch (e) {
  		console.log('catch outside', e);
	}
	// catch inner a
	// catch outside b
    
    // 遍历器对象抛出了两个错误，第一个被 Generator 函数内部捕获，第二个因为函数体内部的catch 函数已经执行过了，不会再捕获这个错误，所以这个错误就抛出 Generator 函数体，被函数体外的 catch 捕获
---

### yield* 表达式
yield* 表达式表示 yield 返回一个遍历器对象，用于在 Generator 函数内部，调用另一个 Generator 函数。

	function* callee() {
    	console.log('callee: ' + (yield));
	}
	function* caller() {
    	while (true) {
        	yield* callee();
    	}
	}
	const callerObj = caller();
	callerObj.next();
	// {value: undefined, done: false}
	callerObj.next("a");
	// callee: a
	// {value: undefined, done: false}
	callerObj.next("b");
	// callee: b
	// {value: undefined, done: false}
 
	// 等同于
	function* caller() {
    	while (true) {
        	for (var value of callee) {
          		yield value;
        	}
    	}
	}
---
    
## 使用场景
**实现 Iterator**	
为不具备 Iterator 接口的对象提供遍历方法。

	function* objectEntries(obj) {
    	const propKeys = Reflect.ownKeys(obj);
        // Reflect.ownKeys() 返回对象所有的属性，不管属性是否可枚举，包括 Symbol
    	for (const propKey of propKeys) {
        	yield [propKey, obj[propKey]];
    	}
	}
 
	const jane = { first: 'Jane', last: 'Doe' };
	for (const [key,value] of objectEntries(jane)) {
    	console.log(`${key}: ${value}`);
	}
	// first: Jane
	// last: Doe
	// jane 原生是不具备 Iterator 接口无法通过 for... of遍历。这边用了 Generator 函数加上了 Iterator 接口，所以就可以遍历 jane 对象了。