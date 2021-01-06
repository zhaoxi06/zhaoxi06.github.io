---
title: JS中实现继承的几种方式
date: 2018-11-29 19:36:15
categories: js
tags:
---
JS中实现继承的几种方式：原型链、借用构造函数、组合继承、原型式继承、寄生式继承、寄生组合式继承。

<!--more-->
### 1、原型链
&emsp;&emsp;其基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。假如我们让一个原型对象等于另一个类型的实例（A' == b），结果会怎么样呢？（用A’代表原型，A代表构造函数，a代表实例）显然，此时的原型对象将包含一个指向另一个原型的指针（即：A'.\_\_proto\_\_ == B' == B.prototype)，相应第，另一个原型中也包含着一个指向另一个构造函数的指针（B'.constructor == B.prototype.consuctor == B）。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条。这就是所谓原型链的概念。

    function SuperType(){
        this.prototype = true;
    }
    SuperType.prototype.getSuperValue = function(){
        return this.prototype;
    };
    function SubType(){
        this.subprototype = false;
    }
    //subType继承了SuperType
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function(){
        return this.subprototype;
    };
    var instance = new SubType();
    alert(instance.getSuperValue());

&emsp;&emsp;原型链最主要的问题来自包含引用类型值的原型。即包含引用类型值的原型属性会被所有实例共享。在通过原型来实现继承时，原型实际上会变成另一个类型的实例。于是，原先的实例属性也就变成了现在的原型属性了。
&emsp;&emsp;原型链的第二个问题是：在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法再不影响所有对象实例的情况下，给超类型的构造函数传递参数。因以上两个问题，实践中很少会单独使用原型链。

### 2、借用构造函数
&emsp;&emsp;这种技术的基本思想相当简单，即在子类型构造函数的内部调用超类型构造函数。因此通过使用apply()和call()方法也可以在（将来）新创建的对象上执行构造函数。

    function SuperType(name){
         this.name = name;
     }
     SuperType.prototype.getName = function(){
         console.log(this.name);
     }
     function SubType(){
         //①继承了SuperType，同时还传递了参数
         SuperType.call(this, "Nicholas");
         //②实例属性
         this.age = 29;
     }
     var instance = new SubType();
     console.log(instance.name);     //Nicholas
     console.log(instance.age);      //29
     instance.getName();             //error

&emsp;&emsp;如果仅仅是借用构造函数，那么将无法避免构造函数模式存在的问题——方法都在构造函数中定义，因此函数复用就无从谈起了。而且，在超类型的原型中定义的方法（如：getName()），对子类型而言也是不可见的，结果所有类型都只能使用构造函数模式。因此，借用构造函数逇技术也是很少单独使用的。

### 3、组合继承
&emsp;&emsp;将原型链和借用构造函数的技术组合在一块，从而发挥二者之长的一种继承模式。其实现思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来使用对实例属性的继承。
    
    function SuperType(name){
        this.name = name;
        this.colors = ["red","blue","green"];
    }
    SuperType.prototype.sayName = function(){
        console.log(this.name);
    };
    function SubType(name, age){
        //继承属性
        SuperType.call(this, name);     //第二次调用SuperType()
        this.age = age;
    }
    //继承方法
    SubType.prototype = new SuperType();    //第一次调用SuperType()
    SubType.prototype.sayAge = function(){
        console.log(this.age);
    };
    var instance1 = new SubType("Nicholas", 29);
    instance1.colors.push("black");
    console.log(instance1.colors);      //"red,blue,green,black"
    instance1.sayName();    //"Nicholas"
    instance1.sayAge();     //29
    var instance2 = new SubType("Greg", 28);
    console.log(instance2.colors);      //"red,blue,green"
    instance2.sayName();    //"Greg"
    instance2.sayAge();     //28

组合继承最大的问题就是无论什么情况下，都会调用两次超类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。
### 4、原型式继承
&emsp;&emsp;借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。

    function object(o){
        function F(){}
        F.prototype = o;
        return new F();
    }
    var person = {
        name: “Nicholas”,
        friends: [“Shelby”, “Court”, “van”];
    };
    var anotherPerson = object(person);
    anotherPerson.name = “Greg”;
    anotherPerson.friends.push(“Rob”);
    var anotherPerson = object(person);
    anotherPerson.name = “Linda”;
    anotherPerson.friends.push(“Barbie”);
    console.log(person.friends);    //”Shelby,Court,Van,Rob,Barbie”

### 5、寄生式继承
&emsp;&emsp;寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，最后再返回对象。

    function object(o){
        function F(){}
        F.prototype = o;
        return new F();
    }
    function createAnother(original){
        var clone = object(original);
        clone.sayHi = function(){
            alert(hi);
        }
        return clone;
    }
    var person = {
        name: “NIcholas”,
        friends: [“Shelby”, “Court”, “van”];
    };
    var anotherPerson = createAnother(person);
    anotherPerson.sayHi();  //hi

### 6、寄生组合式继承
&emsp;&emsp;通过借用构造函数来继承属性，通过原型链的混成形式来继承方法。其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

    function object(o){
        function F(){}
        F.prototype = o;
        return new F();
    }
    function inherPrototype(subType, superType){
        var prototype = object(superType.prototype);    //创建对象
        prototype.constructor = subType;                //增强对象
        subType.prototype = prototype;                  //指定对象
    }
    function SuperType(name){
        this.name = name;
        this.colors = [“red”,”blue”,”green”];
    }
    SuperType.prototype.sayName = function(){
        console.log(this.name);
    };
    function SubType(name, age){
        SuperType.call(this, name);
        this.age = age;
    }
    inherPrototype(subType, superType);
    SubType.prototype.sayAge = function(){
        console.log(this.age);
    };

[详细介绍请看 6.3继承](https://github.com/zhaoxi06/Professional-JavaScript-for-Web-Developers/blob/master/%E5%85%AD%E3%80%81%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/note.md)