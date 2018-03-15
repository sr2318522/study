##promise
promise是一个构造函数,new promise返回一个promise对象 接受一个excutor执行函数做为参数,excutor有两个函数类型形参resolve reject


```javascript
	const promise=new Promise ((resolve,reject)=>{
		//异步处理结果
		//结果处理后调用resolve或者reject
	})
```
##Promise是一个承诺
承诺返回三种状态:
>1. pending
>2. fulfilled
>3. rejected
>

promise对象初始化状态为pending

当调用resolve成功会有pending=>fulfilled

当调用reject失败会由pending=>rejected

且promise只能有pending转化成一种状态且不能再次改变

##promise对象方法
1. then方法注册 当resolve/reject的回调函数
2. resolve成功以后onFulfilled会被调用
3. reject onRejected会被调用
4. promise.catch在链式写法中可以捕获前面的then发送的异常
5. promise chain promise.then方法每次调用以后都会返回一个新的promise对象所以可以使用链式写法

>promise的静态方法
>
>1.Promise.resolve返回一个fulfilled状态的promise对象
>
>2.Promise.reject返回一个rejected转台的promise对象
>
>3.Promise.all接受一个promise对象数组为参数
>
>4.Promise.race接受一个promise对象数组为参数


##promise代码实现
```javascript
	//首先定义promise的三个状态
	const PENDING= 'pending';
	const FULFILLED='fulfilled';
	const REJECTED='rejected';
	function Promise(excutor){
		let that =this;
		that.status=PENDING;//获取初试状态
		this.value=undefined;
		this.reason=undefined;
		//存储fulfilled转台对应的onFulfilled函数
		that.onFulfilledCallbacks=[];
		that.onRejectedCallbacks=[];
		//value成功转台接受的最终值
		function resolve(value){
			if(value instanceof Promise){
				return value.then(resolve,reject)
			}
		}
	}
```

