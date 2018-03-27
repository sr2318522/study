##Class
es6提供了更接近传统语言的写法,引入了Class的概念,作为对象的模板.通过Class关键字可以定义类
>基本上,es6的class可以看做只是一个语法糖,他的绝大部分功能es5都可以做到,新的class写法只是让对象原型的写法更加清晰,更面向对象编程的语法而已

```javascript
	class Point{
		constructor(x,y){
			this.x=x;
			this.y=y;
		}
		toString(){
			return this.x+this.y;
		}
	}
```
上面定义了一个类,可以看到里面有个constructor方法,这个就是构造方法,而this关键字代表的是实例对象.也就是说es5的构造函数Point对应es6的Point类的构造方法

Point类除了构造方法,还定义了一个toString方法.注意定义类方法的时候之前不需要加上function关键字.直接帮函数定义进去就可以了,而且方法之间不需要逗号分隔,加了会报错(事实上类所有的方法都定义在类的prototype属性上面)
>由于类的方法都定义在prototype对象上面,所以累的新方法可以添加在prototype对象上面,Object.assign方法可以很方便的向类添加多个方法

```
	class Point{
		constructor(){
		
		}
	}
	Object.assign(Point.prototype,{
		toString(){},
		toValue(){}
	})
```
prototype对象的constructor属性直接指向类本身 这与es5的行为是一致的
另外所有的类的内部方法都是不可枚举的(这一点与es5的行为不一致);

```javascript
	class Point {
	  constructor(x, y) {
	    // ...
	  }
	
	  toString() {
	    // ...
	  }
	}
	
	Object.keys(Point.prototype)
	// []
	Object.getOwnPropertyNames(Point.prototype)
	// ["constructor","toString"]
```
```		
	var Point = function (x, y) {
	  // ...
	};
	
	Point.prototype.toString = function() {
	  // ...
	};
	
	Object.keys(Point.prototype)
	// ["toString"]
	Object.getOwnPropertyNames(Point.prototype)
	// ["constructor","toString"]
```

####constructor方法
>
constructor方法是类的默认方法.通过new命令生成对象实例时,自动调用该方法.一个类必须有constructor方法,如果没有显示定义,一个空的constructor方法会被默认添加
>
constructor默认返回实例对象(this),但也完全可以指定返回另外一个对象
>
类的调用必须使用new 关键字 否则会报错 这是它跟普通构造函数的主要区别

####类的实例对象
>与es5一样,实例的属性除非显示定义在其本身(this)上,否则都是定义在原型上与es5一样,所有的实例共享一个原型对象

####class表达式
>与函数一样类也可以使用表达式的形式定义

```
	const MyClass = class Me {
	  getClassName() {
	    return Me.name;
	  }
	};
```
注意这个类的名字是MyClass而不是Me,Me只在Class的内部代码可以使用,指当前类
####不存在变量提升
>类不存在变量提升,这一点与es5完全不同

###私有方法和私有属性

