## new 的实现原理是什么

## 如何正确判断 this 的指向

    1.是否是new的绑定
    		如果new这个构造函数返回的不是函数和对象
    		则this指向这个新对象
    		foo()返回这个this对象
    ```
    	function foo(age) {
    		this.age = age
    	}
    	let suwei = foo(18)
    	console.log(suwei.age) //18
     ```
    	如果new这个构造函数返回的是函数和对象
    	```
    		function foo(age) {
    			let a = {
    				obj:1
    			}
    			this.age = age
    			return a
    		}
    		console.log(new foo(18)) // undefined
    	```
    	this 并不能指向构造函数返回的对象
    2.函数是否通过 call, apply 调用，或者使用了 bind 绑定，如果是，那么this绑定的就是指定的对象【归结为显式绑定】。

## 1.深拷贝和浅拷贝的区别是什么？实现一个深拷贝

    	深拷贝和浅拷贝是针对复杂数据类型来说的。
    	浅拷贝是复制对象的引用,当引用指向的值改变的时候，复制的值也跟着改变.
    	深拷贝复制变量值,当复制的是非基础类型的值，则层层递归到基础类型的值再复制，当引用指向的值改变的时候，复制的值不会改变。
    			实现浅拷贝
    	```
    	let item = {
    			a = {
    				name:'suwei',
    				age:18,
    				color:['blue','red']
    			}
    	}

    	let item1 = {...item}
    	let item2 = Object.assign({},item)
    	```
    	实现一个深拷贝
    	1.深拷贝最简单的实现方式
    	```
    		JSON.parse(JSON.stringify(obj))
    	```
    	但是它有一点缺陷
    		1.对象的属性值是函数时，无法拷贝。
    		2.原型链上的属性无法拷贝
    		3.不能正确的处理 Date 类型的数据
    		4.不能处理 RegExp
    		5.会忽略 symbol
    		6.会忽略 undefined
    	2.实现一个深拷贝的函数

    		如果是基本数据类型，直接返回
    		如果是 RegExp 或者 Date 类型，返回对应类型
    		如果是复杂数据类型，递归。
    		考虑循环引用的问题
    	```
    	function deepClone(obj, hash = new WeakMap()) { //递归拷贝
    if (obj instanceof RegExp) return new RegExp(obj);
    if (obj instanceof Date) return new Date(obj);
    if (obj === null || typeof obj !== 'object') {
        //如果不是复杂数据类型，直接返回
        return obj;
    }
    if (hash.has(obj)) {
        return hash.get(obj);
    }
    /**
     * 如果obj是数组，那么 obj.constructor 是 [Function: Array]
     * 如果obj是对象，那么 obj.constructor 是 [Function: Object]
     */
    let t = new obj.constructor();
    hash.set(obj, t);
    for (let key in obj) {
        //递归
        if (obj.hasOwnProperty(key)) {//是否是自身的属性
            t[key] = deepClone(obj[key], hash);
        }
    }
    return t;

}

```

    	funciton deepClone(obj,hash = new WeakMap()) {
    		//对基本数据类型 正则 日期做判断
    		if(obj instanceof RegExp) return new RegExp(obj)
    		if(obj instanceof Date) return new Date(obj)
    		if(obj === null || typeof obj !== Object) {
    			return obj
    		}
    		//对数组 和 对象做判断
    		let t = new obj.constructor()
    		for(let i in obj) {
    			if(obj.hasOwnProperty(i)) {
    				t[i] =
    			}
    		}
    	}

./build.sh leased-area

scp flowportal-leased-area.tar.gz root@172.16.5.31:/home/daho/packages
ssh root@172.16.5.31
cd /home/daho/packages
tar -xzf flowportal-leased-area.tar.gz
cd /usr/share/nginx/
ll
ln -sfn /home/daho/packages/flowportal-leased-area flow
```
