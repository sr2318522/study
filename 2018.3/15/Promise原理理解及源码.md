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
function Promise(executor) {
	var self = this;
	// Promise当前的状态
	self.status = 'pending';
	// Promise的值
	self.data = undefined;
	// Promise resolve时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面
	self.onResolvedCallback = [];
	// Promise reject时的回调函数集，因为在Promise结束之前有可能有多个回调添加到它上面
	self.onRejectedCallback = [];

	function resolve(value) {
		if (self.status === 'pending') {
			self.status = 'resolved';
			self.data = value;
			for (var i = 0; i < self.onResolvedCallback.length; i++) {
				self.onResolvedCallback[i](value);
			}
		}
	}

	function reject(reason) {
		if (self.status === 'pending') {
			self.status = 'rejected';
			self.data = reason;
			if (self.onRejectedCallback.length===0) {
				console.log(reason);
			}
			for (var i = 0; i < self.onRejectedCallback.length; i++) {
				self.onRejectedCallback[i](reason);
			}
		}
	}
	// 执行executor并传入相应的参数
	try {
		executor(resolve, reject);
	} catch (e) {
		reject(e);
	}
}
Promise.prototype.then = function(onResolved, onRejected) {
	var self = this;
	var promise2;
	//根据标准如果then的参数如果不是一个function那么我就要忽略他
	onResolved = typeof onResolved === 'funciton' ? onResolved : function(value) {return value};
	onRejected = typeof onRejected === 'funciton' ? onRejected : function(reason) {return reason};
	if (self.status === 'resolved') {
		//如果promise1(此处的状态已经是确定的并且返回resolve),我们就调用onResolved;
		//因为考虑到有可能throw,所以我们将其包裹在try/catch中
		return promise2 = new Promise(function(resolve, reject) {
			try{
				x=onResolved(self.data);
				//如果onResolved返回的结果是一个Promise对象,直接帮他的结果
				//做完promise2的结果
				if (x instanceof Promise){
					x.then(resolve,reject);
				}
				//否则以他的返回值做完promise2的结果
				resolve(x);
			}catch(e){
				//如果出错以捕获到的错误做完promise2的结果
				reject(e);
			}
		})
	}
	// 此处与前一个if块的逻辑几乎相同，区别在于所调用的是onRejected函数
	if (self.status === 'rejected') {
		return promise2 = new Promise(function(resolve, reject) {
			return promise2=new Promise (function (resolve,reject) {
				try{
					var x=onRejected(self.data)
					if (x instanceof Promise) {
						x.then(resolve,reject)
					}
				}catch(e){
					reject(e);
				}
			})
		})
	}
	if (self.status === 'pending') {
		//如果当前的Promise还处于pending状态,我们并不能确定调用的是onResolved还是onRejected
		//只能等Promise状态确定好才能确定如何处理
		//所以我们需要吧我们的两种情况作为callback放入promise的回调数组里
		return promise2 = new Promise(function(resolve, reject) {
			self.onResolvedCallbacn=push(function (value) {
				try{
					var x=onResolved(self.data)
					if (x instanceof Promise) {
						x.then(resolve,reject)
					}
				}catch(e){
					reject(e)
				}
			})
			self.onRejectedCallback.push(function (reason) {
				try{
					var x=onRejected(self.data)
					if (x instanceof Promise) {
						x.then(resolve,reject)
					}
				}catch(e){
					reject(e);
				}
			})
		})
	}
}
Promise.prototype.catch=function (onRejected) {
	return this.then(null,onRejected);
}
Promise.prototype.done=function () {
	return this.catch(function (e) {
		console.log(e);
	})
}
```

