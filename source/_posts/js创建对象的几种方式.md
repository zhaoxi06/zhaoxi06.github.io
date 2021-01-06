---
title: js创建对象的几种方式
date: 2018-11-28 14:40:10
categories: js
tags:
---
js创建对象的几种方式
<!--more-->
### 1、Object构造函数
<pre><code>
    var person = new Object();
    person.name = "Nicholas";
    person.name = 29;
</code></pre>

### 2、对象字面量方法（实际上不会调用Object构造函数）
<pre><code>
    var person = {
         name : "Nicholas",
         age : 29
         //属性名可用字符串表示，也可以不用
         //"name" : "Nicholas"
     }
     //也可以留空花括号
     var person = {};    //与 new Object()相同
     person.name = "Nicholas";
     person.age = 29;
</code></pre>

构造函数或对象字面量创建很多对象，会产生大量的重复代码。

### 3、工厂模式
<pre><code>
    function createPerson(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        }
        return o;
    }
    var person1 = createPerson("Nicholas",29,"Software Enginerr");
    var person2 = createPerson("Greg", 27, "Doctor");
</code></pre>

工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎样知道一个对象的类型）。

### 4、构造函数模式
<pre><code>
    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = function(){
            alert(this.name);
        }
    }
    var person1 = new Person(“Nicholas”, 29, “Software Engineer”);
    var person2 = new Person(“Greg”, 27, “Doctor”);
</code></pre>

 使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。函数是对象，因此每定义一个函数，也就是实例化了一个对象。以这种方式创建函数，会导致不同的作用域链和标识符解析。没有必要创建两个完成同样任务的Function实例。

### 5、原型模式
<pre><code>
    function Person(){}
    Person.prototype = {
        constroutor: Person,
        name: “Nicholas”,
        age: 29,
        job: “Software Engineer”,
        sayName: function(){
            alert(this.name);
        }
    }
    var person1 = new Person();
    person1.sayName();      //"Nicholas"
    var person2 = new Person();
    person2.sayName();      //"NIcholas"
    alert(person1.sayName == person2.sayName);      //true
</code></pre>

原型模式的最大问题在于，原型中所有属性是被很多实例共享的，这种共享对于函数非常合适。对于那些包含基本值的属性也还行。然而，对于包含引用类型值的属性来说，问题就比较突出了。
### 6、组合使用构造函数和原型模式
<pre><code>
    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["Shelly", "Court"];
    }
    Person.prototype = {
        constructor: Person,
        sayName: function(){
            alert(this.name);
        }
    }
</code></pre>

### 7、动态原型模式
<pre><code>
    function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        //在sayName()不存在的情况下才会执行。
        //即这段代码只会在初次调用构造函数时才会执行。
        if(typeof this.sayName != "function"){
            Person.prototype.sayName = function(){
                alert(this.name);
            }
        }
    }
</code></pre>

把所有信息都封装在了构造函数中，而通过在构造函数中初始化原型（仅在必要的情况下），又保持了同时使用构造函数和原型的优点。

### 8、寄生构造函数模式
这种模式的思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象。

<pre><code>
    function Person(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        }
        return o;
    }
    var friends = new Person("Nicholas", 29, "Software Engineer");
    friends.sayName();  //Nicholas
</code></pre>

### 9、稳妥构造函数模式
稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：一是新创建对象的实例方法不引用`this`；二是不使用`new`操作符调用构造函数。

<pre><code>
    function Person(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(name);
        }
        return o;
    }
    var friends = Person("Nicholas", 29, "Software Engineer");
    friends.sayName();  //Nicholas
</code></pre>

以这种模式创建的对象，除了使用sayName()之外，没有其他办法访问name的值。

[详细介绍请看 6.2创建对象](https://github.com/zhaoxi06/Professional-JavaScript-for-Web-Developers/blob/master/%E5%85%AD%E3%80%81%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1/note.md)