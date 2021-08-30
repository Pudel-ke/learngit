#### 理解对象

##### 属性类型  

javascript中不能直接访问，一般通过Object.defineProperty(obj,propertyName,{})配置

###### 数据属性

- [[configurable]] : 能否通过delete删除，能否修改属性的特性，能否修改为访问器属性，默认为true。**属性一旦设置为不可配置，就不能再变回可配置**了。此时修改除**[[writable]]**之外的特性，都会抛出错误。
- [[Enumerable]]: 能否for-in循环返回属性。默认为true。
- [[writable]]: 能否修改属性值。默认为true。
- [[value]]: 表示属性的值。默认为undefined。

开发者直接创建的对象，数据属性的特性都是默认值，true/undefined

通过Object.defineProperty()创建新属性时，如不指定特性，configurable、writable、enumerable 都是false 

###### 访问器属性

- [[configurable]]:能否通过delete删除，能否修改属性特性，能否修改为数据属性。
- [[enumerable]] : 能否for-in循环返回属性.
- [[get]]: 读取属性时调用的函数，默认undefined

- [[set]]: 写入属性时调用的函数，默认undefined

**访问器属性不能直接定义，需要Object.defineProperty()来定义**

**建议不要再IE8中使用该方法**

属性名前带'_'，表示只能通过对象方法访问

```js
var book = {
    _year: 2004,
    edition: 1
}
Object.defineProperty(book, "year", {
    get: function(){
        return this._year;
    },
    set: function(newValue){
      if(newValue>2004) {
          this._year = newValue;
          this.edition += newValue - 2004;
      }  
    },
});
book.year = 2005;  //写操作，写入的值会作为set的参数
alert(book.edition); //2  读操作， book.edittion 返回
```

通过此方法

- 只指定getter ： 只都不写
- 只指定setter：只写不读

##### 定义多个属性

使用花括号

```js
Object.defineProperty(book, {
		_year: {

		writable: true,
			value: 2004
},
		edition:  {
			writable: true,
			value: 1
}
    	year: {
    	get: function(){
    return this._year;
},
    	set: function() {
            return
        }
    
}

})
```

##### 读取属性

使用 Object.getOwnPropertyDescriptor()方法

参数为，属性所在对象、要读取的描述符名称

```js
var descriptor = Object.getOwnPropertyDescriptor(book,"_year");
```



#### 创建对象

###### 工厂模式

```js
function createPerson(name, age) {
    var person = {
        name: name,
        age: age,
        sayName: function(){alert(this.name)}
    };
    return person;
}
var person1 = createPerson("qk", 14);

```

**特点：结构简单，每次只用调用函数就能创建,但未解决对象识别的问题**



###### 构造函数模式

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.sayName = function(){
        alert(this.name)};
}
var person1 = new Person();

```

**特点：解决了对象识别问题。但创建每个实例都需要声明方法，没有做到函数复用**



###### 原型模式

```js
function Person(){}
Person.prototype.name = "qk";
Person.prototype.sayName = function(){
    alert(this.name);
}; 
//或者通过字面量添加  注意生成字面量时 如不指定，constructor会指向Object，因为默认是Object
// Person.prototype = {
//     constructor: Person,
//     name: "qk",
//     sayName: function(){
//         alert(this.name)
//     }
// };
// let person2 = new Person();

```

**在构造函数指向的原型对象中创建方法和属性，但若有引用类型属性，则实例之间操作会相互作用**



###### 组合模式

```js
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.friends = ["Tom", "Jack"];
}
Person.prototype.gender = "man";
Person.prototype.sayName = function(){
    alert(this.name);
};
let person1 = new Person();

```

**构造函数中设置各自拥有的实例属性，原型对象中设置共享属性和方法 ！使用的最多的方式！**

**另注意**

- for-in循环，返回所有能通过对象访问的、可枚举的属性，包括原型中的属性
- Object.keys()方法，接收对象作为参数，返回包含所有可枚举属性的字符串数组
- Object.getOwnPropertyNames() 接收对象作为参数，返回所有**实例属性**，**不论是否可枚举**

###### 寄生构造函数模式

```js
function Person(name, job, age) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function() {
        alert(this.name);
    };
    return o;
}
var friend = new Person("xx","cat", 22);
```

**可以创建一个具有额外方法的对象，不能依赖instanceof来判断**



###### 稳妥构造函数模式

```js
function Person(name,age){
    var o = new Object();
    //可添加私有变量和函数
    o.sayName = function(){
        alert(this.name);
    };
    return o;
}

```

**除了使用sayName()方法外，没办法访问name的值，instanceof 无意义**



#### 继承对象

###### 原型链继承

```js
function SuperType(){

  this.property = true;

};

SuperType.prototype.getSuperProperty = function() {
  return this.property;
};

function SubType() {

  this.subProperty = false;

}

SubType.prototype = new SuperType(); 

//此时supertype实例化为subtype.prototype 故supertype中的实例属性变成subtype的原型对象属性

//注意 这之后不能通过字面量为subty.prototype设定原型属性和方法，否则会切断与超类构造函数原型的联系

SubType.prototype.constructor = "SubType"; //如不指定，则constructor的指向为SuperType.prototype.constructor的指向

SubType.prototype.getSubProperty = function(){

  return this.subProperty;

};

let instance1 = new SubType();

console.log(instance1.property) //true
console.log(instance1.subProperty) //false
instance1.getSubProperty();//false
instance1.getSuperProperty(); //ture
```



###### 借用构造函数继承

```js
function SuperType(name,age) {
    this.name = name;
    this.age = age;
    this.friends = ["Tobby", "Jane", "Duke"]
}
SuperType.prototype.sayAge = function(){
    alert(this.age);
};

function SubType(name, age) {
    SuperType.apply(this, arguments);
}
SubType.prototype.sayName = function() {  
    alert(this.name);
};
let instane1 = new SubType("john", 22);
```

**在子类构造函数中调用父类构造函数，同时可以传递参数缺点 创建一个对象会调用两次构造函数，且子类构造函数的实例无法继承超类的原型对象属性和方法**



###### 组合继承（最常用）

```js
function SuperType(name, age) {
    this.name = name;
    this.age = age;
    this.colors = ["red", "green", "blue"];
}

SuperType.prototype.sayName = function(){
    alert(this.name);
};

function SubType(name, age) {
    SuperType.apply(this, [name, age]); //this指针指向新的实例，因此超类构造函数中的实例属性变成子类构造函数的原型对象属性后，会被屏蔽
}

SubType.prototype = new SuperType(); //先继承 后增加属性和方法
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
    alert(this.age);
};
var instance1 = new SubType("John", 21);
var instance2 = new SubType("Peter", 22);
```

**最常用 借用构造函数继承实例属性（运用到apply方法），通过原型链继承原型属性及方法。**

**缺点： 调用了两次超类的构造函数**



###### 原型式继承

```js
var obj = {
    name: "qk",
    age: 22
};
function create(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
let newPerson = create(obj);
```

**对已有对象的浅复制 即 让已有对象作为构造函数的原型对象，并在函数中返回新对象, 新的对象中不存在prototype属性，但是有proto**



###### 寄生式继承

```js
function createAnother(obj) {
    let clone = create(obj);
    clone.sayName = function(){
        alert(this.name); //增强对象 添加属于自己的方法
    };
    return clone;
}
```

**基于已有的对象创建拷贝对象，然后对对象进行增强。是原型式继承的增强，适用于相似对象的继承**

任何能返回新对象的函数都使用于此模式

**缺点：不能做到函数复用，引用类型数据依然共享**



###### 寄生组合式继承（最佳） 

```js
function create(o) {
    function F() {}
    F.prototype = o;
    return new F();
}
//浅拷贝函数
function inheritPrototype(superType, subType) {
    var prototype = create(superType.prototype); //其中create函数可用Object.create()方法替代
    prototype.constructor = subType;
    subType.prototype = prototype;
}
function SuperType(name) {
    this.name = name;
    this.colors = ['pink', 'blue', 'green'];
}
//...code
function SubType(name, age) {
    SuperType.call(this, name);
    this.age = age;
}
// SubType.prototype = new SuperType();  此为组合继承的调用方式
inheritPrototype(SubType, SuperType);  //寄生组合式继承
let instance1 = new SubType("qk", 22);
```

**为了解决超类型构造函数两次调用问题 通过寄生式继承来创建超类原型对象的拷贝**

**只调用了一次超类构造函数，效率更高。避免在SubType.prototype上面创建不必要的、多余的属性，与其同时，原型链还能保持不变。**

