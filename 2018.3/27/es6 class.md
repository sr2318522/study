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
>私有方法是常见需求,但是es6不提供,只能通过变通的方法模拟实现.
>一种做法是在命名上加以区别

```
	class Widget {
	  // 公有方法
	  foo (baz) {
	    this._bar(baz);
	  }
	
	  // 私有方法
	  _bar(baz) {
	    return this.snaf = baz;
	  }
	
	  // ...
	}
```
在上面代码中_bar方法前面的下划线,标识这是体格只限于内部使用的私有方法.但是,这种命名是不保险的,在类的外部,还是可以调用这个方法.

另一种方法是索性将私有方法移除模块,因为模块内部的所有方法都是对外可见的

```
	class Widget{
		foo(baz){
			bar.call(this.baz);
		}
		//...
	}
	function bar (baz){
		return this.snaf=baz;
	}
```
上面代码中foo是公有方法,内部调用了bar.call(this,baz)这使得bar实际上成为了当前模块的私有方法.

还有一种方法是利用symbol值的唯一性,讲私有方法的名字命名为一个symbol值.

```
	const bar =Symbol('bar');
	const snaf=Symbol('snaf');
	
	export default class myClass{
	
	  // 公有方法
	  foo(baz) {
	    this[bar](baz);
	  }
	
	  // 私有方法
	  [bar](baz) {
	    return this[snaf] = baz;
	  }
	
	  // ...
	};
```
上面代码中,bar和snaf都是symbol值 导致第三方无法取到他们,因此达到了私有方法和私有属性的效果
###私有属性的提案
与私有方法一样,es6不支持私有属性,目前有一个提案,为class加了私有属性 方法是在属性名之前使用#表示

```
	class Point{
		#x;
		constructor(x=0){
			#x=+x;
		}
		get x(){
			return #x;
		}
		set x(value)
			#x=+value;
		}
	}
```
上面的代码中,#x就标识私有属性x,在Point类之外是读取不到这个属性的.还可以看到,私有属性与实例属性是可以同名的,虽然提案之规定了私有属性的写法,但是很自然的 它也可以用来写私有方法
###this的指向
类的方法内部如果有this,它默认指向类的实例但是必须非常小心,一旦单独使用该方法很可能报错

```
	class Logger {
	  printName(name = 'there') {
	    this.print(`Hello ${name}`);
	  }
	
	  print(text) {
	    console.log(text);
	  }
	}
	
	const logger = new Logger();
	const { printName } = logger;
	printName()
```
上面代码中printName方法中的this会默认指向logger类的实例但是如果将这个方法单独使用,this会指向运行所在的环境,会因为找不到print而报错

一个比较简单的解决方法是在构造方法是,在构造方法中绑定this,这样就不会找不到print方法了

```
	class Logger{
		construcotor{
			this.printName=this.printName.bind(this);
		}
	}
```
另一种解决方法就是使用箭头函数

```
	class Logger{
		constructor(){
			this.printName=(name='there')=>{
				this.print(`hello ${name}`);
			}
		}
	}
```
###name属性
>由于本质上es6的类只是es5的构造函数的一层包装,所以函数的许多特性都被class继承包括name属性 name属性总是返回紧跟在class关键字后面的类名

```
	class Point{}
	Point.name//Point
```

###class取值函数(getter)和存值函数(setter)
>与es5一样,在类的内部可以使用get和set关键字,对某个属性设置存值和取值函数,拦截该属性的存取行为

```
	class MyClass{
		constructor(){
			//...
		}
		get prop(){
			return 'getter';
		}
		set prop(){
			console.log('setter');
		}
	}
	
	let inst=new MyClass();
	inst.prop=123;
	//setter
	inst.prop
	//getter
```
上面代码中prop熟悉有对应的存值函数和取值函数,因此赋值和取值行为都被定义了

存值函数和取值函数都是设置在属性Descriptor对象上的

```
	class CustomHTMLElement(){
		constructor(element){
			this.element=element
		}
		get html(){
			return this.element.innerHTML
		}
		set html(value){
			this.element.innerHTML=value;
		}
	}
	var descriptor=Object.getOwnPropertyDescriptor(
		CustomHTMLElement.prototype,'html'
	);
	'get' in descriptor //true
	'set' in descriptor //true
```

###class的静态方法
>类相当于实例的原型,所有在类中定义的方法,都会被实例继承,如果在一个方法前加上static关键字 就标识这个方法不会被实例继承 而是直接通过类来调用,这个被称为静态方法

```
	class Foo{
		static classMethod(){
			return 'hellow';
		}
	}
	Foo.classMethod();//hellow;
	var foo=new foo();
	foo.classMethod();//报错
```

上面代码中 foo类的classMethod方法前有static关键字 表面该方法是一个静态方法可以直接在foo类上调用而不是在foo类的实例上调用 如果在实例上调用静态方法会抛出一个错误 表示该方法不存在

注意如果静态方法上包含this关键字 这个this指向的是类 而不是实例

```
	class Foo{
		static bar(){
			this.baz();
		}
		static baz (){
			console.log('hello')
		}
		baz(){
			console.log('world)
		}
	}
	Foo.bar()//hello
```

上面代码汇总静态方法bar调用了this.baz这里的this指向的是Foo类 而不是Foo实例 等同于调用Foo.baz 另外这个例子还可以看出 静态方法可以与非静态方法重名
父类的静态方法可以被子类继承

```
	class Foo{
		static classMethod(){
			return 'hello';
		}
	}
	class Bar extends Foo {
	
	}
	Bar.classMethod() //hello
```

上面代码中，父类Foo有一个静态方法，子类Bar可以调用这个方法。
静态方法也是可以从super对象上调用的。

```
	class Foo {
	  static classMethod() {
	    return 'hello';
	  }
	}
	
	class Bar extends Foo {
	  static classMethod() {
	    return super.classMethod() + ', too';
	  }
	}
	
	Bar.classMethod() // "hello, too"

```

###class的静态书写和实例属性
>静态属性指的是class本身的属性 既class.propName,而不是定义在实例对象this上的属性

```
class Foo {
}

Foo.prop = 1;
Foo.prop // 1

```

上面的这个写法为foo类定义了一个静态书写prop

目前只有这种写法可行 因为es6明确规定 class 内部只有静态方法没有静态属性

```
// 以下两种写法都无效
class Foo {
  // 写法一
  prop: 2

  // 写法二
  static prop: 2
}

Foo.prop // undefined

```
目前有一个静态属性的提案 对实例属性和静态属性都规定了新的写法


1. 类的实例属性
>类的实例属性可以用等式写入类的定义中

```
class MyClass {
  myProp = 42;

  constructor() {
    console.log(this.myProp); // 42
  }
}
```

上面的代码中 myprop就是myclass的实例属性 在myclass 实例上 可以读取这个属性

以前我们定义实例属性 只能写在类的constructor方法里面

```
class ReactCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
}
```

上面的代码中构造方法constructor里面定义了this.state属性

有了新的写法以后 可以不再constructor方法里面定义

```
class ReactCounter extends React.Component {
  state = {
    count: 0
  };
}
```

这种写法比以前更清晰

为了可读性的目的,对比哪些在constructor里面已经定义的实例属性,新写法允许直接列出

```
class ReactCounter extends React.Component {
  state;
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
}
```
2类的静态属性

类的静态书写只需要在实例属性前面加上static关键字就可以了

```
class MyClass {
  static myStaticProp = 42;

  constructor() {
    console.log(MyClass.myStaticProp); // 42
  }
}
```

同样的这个新写法 大大方便了静态属性的表达

```
// 老写法
class Foo {
  // ...
}
Foo.prop = 1;

// 新写法
class Foo {
  static prop = 1;
}
```

上面代码中,老邪法的静态属性定义在类的外部.整个类生成以后,在生成静态属性.这样很容易让人忽略这个静态属性,也不符合相关代码应该放在一起的代码组织原则.另外,新鞋发是显示声明 而不是赋值处理,语义更好

###new.target属性
>new是从构造函数生成实例对象的命令.e6 为new命令引入了一个new.target书写 改属性一般用在构造函数之中返回new 命令作用与那个构造函数 如果构造函数不是通过new 命令盗用的 new.target返回undefined 因此这个属性可以用来确定构造函数是怎么调用的

```
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错

```

 上面代码确保构造函数只能通过new 命令调用
 
 class内部调用 new.target返回当前class
 
 ```
 class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    this.length = length;
    this.width = width;
  }
}

var obj = new Rectangle(3, 4); // 输出 true
 
 ```
 
 需注意的是 子类继承父类时 new.target会返回子类
 
 ```
 class Rectangle {
  constructor(length, width) {
    console.log(new.target === Rectangle);
    // ...
  }
}

class Square extends Rectangle {
  constructor(length) {
    super(length, length);
  }
}

var obj = new Square(3); // 输出 false
 ```


上面代码汇总  new.target会返回子类 

利用这个特点 可以写出不能独立使用 必须继承之后才能使用的类


```
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}

var x = new Shape();  // 报错

```
上面diamante中 shape类不能被实例化,只能用于继承

注意 在函数外部,调用new.target会报错