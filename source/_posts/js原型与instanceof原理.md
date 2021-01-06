---
title: JS原型链与instanceof底层原理
date: 2019-12-17 17:20:00
categories: js
tags:
---
转载自：[JS原型链与instanceof底层原理](https://www.jianshu.com/p/6c99d3678283)

<!--more-->

## 问题
instanceof 可以判断一个引用是否属于某构造函数；

另外，还可以在继承关系中用来判断一个实例是否属于它的父类型。

老师说：instanceof的判断逻辑是： 从当前引用的proto一层一层顺着原型链往上找，能否找到对应的prototype。找到了就返回true。

如果没有发生继承关系，这个逻辑自然是没有疑惑的。

但是，利用原型继承，切断了原来的prototype的指向，而指向了一个新的对象，这时instanceof又是如何进行判断的呢？

本文通过代码与画图分析instanceof的底层原理，借此复习原型链的知识。

## instanceof底层是如何工作的：
```

function instance_of(L, R) {//L 表示左表达式，R 表示右表达式 

    var O = R.prototype;   // 取 R 的显示原型 

    L = L.__proto__;  // 取 L 的隐式原型

    while (true) {    

        if (L === null)      

             return false;   

        if (O === L)  // 当 O 显式原型 严格等于  L隐式原型 时，返回true

             return true;   

        L = L.__proto__;  

    }

}

```

## 案例：未发生继承关系时
```

 function Person(name,age,sex){
 
     this.name = name;
 
     this.age = age;
 
     this.sex = sex;
 
 }
 
 function Student(score){
 
     this.score = score;
 
  }
 
 var per = new Person("小明"，20，“男”)；
 
 var stu = new Student（98）；
 
 console.log(per instanceof Person);  // true
 
 console.log(stu instanceof Student);  // true
 
 console.log(per instanceof Object);  // true
 
 console.log(stu instanceof Object);  // true

```

### 1、下图1是未发生继承关系时的原型图解：
![未发生继承关系时的原型图解](/images/js/17.jpg)

### 2、instanceof的工作流程分析
**首先看per instanceof Person**
```
 function instance_of(L, R) { // L即per ；  R即Person
 
   var O = R.prototype; //O为Person.prototype
 
   L = L.__proto__;       // L为per._proto_
 
   while (true) {    //执行循环
 
        if (L === null)   //不通过
 
            return false;   
 
        if (O === L)       //判断：Person.prototype ===per._proto_？
 
             return true;  //如果等于就返回true，证明per是Person类型
 
        L = L.__proto__;                   
 
   }
 
}         
```
执行per instanceof Person ，通过图示看出 Person.prototype ===per.proto 是成立的，所以返回true，证明引用per是属于构造函数Person的。

**接下来再看 per instanceof Object**
```
 function instance_of(L, R) { //L即per ；  R即Object        

    var O = R.prototype;  //O为Object.prototype        
 
    L = L.__proto__;    // L为per._proto_             
 
    while (true) {     //执行循环                   
 
        if (L === null)   //不通过                            
 
            return false;                      
 
        if (O === L)   //Object .prototype === per._proto_？  不成立**
 
             return true;                         
 
         L = L.__proto__;   //令L为 per._proto_ ._proto_ ，**
 
                         //即图中Person.prototype._proto_指向的对象
 
                         //接着执行循环，
 
                         //到Object .prototype === per._proto_ ._proto_  ？
 
                         //成立，返回true
 
          }
 
 }
```
## 案例：发生继承关系时
```
 function Person(name,age,sex){
 
     this.name = name;
 
     this.age = age;
 
     this.sex = sex;
 
 }
 
 function Student(name,age,sex,score){
 
     Person.call(this,name,age,sex);  
 
     this.score = score;
 
  }
 
 Student.prototype = new Person();  // 这里改变了原型指向，实现继承
 
 var stu = new Student("小明",20,"男",99); //创建了学生对象stu
 
 console.log(stu instanceof Student);    // true
 
 console.log(stu instanceof Person);    // true
 
 console.log(stu instanceof Object);    // true
```

### 1、下图2是发生继承关系后的原型图解
![发生继承关系后的原型图解](/images/js/18.jpg)

### 2、instanceof的工作流程分析
**首先看 stu instanceof Student**
```
 
 function instance_of(L, R) { //L即stu ；  R即Student
 
   var O = R.prototype;  //O为Student.prototype,现在指向了per
 
    L = L.__proto__;    //L为stu._proto_，也随着prototype的改变而指向了per
 
    while (true) {    //执行循环
 
          if (L === null)  //不通过
 
              return false;   
 
          if (O === L)    //判断： Student.prototype ===stu._proto_？
 
              return true;  //此时，两方都指Person的实例对象per，所以true
 
          L = L.__proto__;                   
 
      }
 
 } 
```
所以，即使发生了原型继承，stu instanceof Student 依然是成立的。

**接下来看 stu instanceof Person，instanceof是如何判断stu继承了Person**
```
 function instance_of(L, R) { // L即stu ；  R即Person        
 
   var O = R.prototype; // O为Person.prototype     
 
    L = L.__proto__;   //L为stu._proto_，现在指向的是per实例对象
 
    while (true) {   // 执行循环                   
 
       if (L === null)   //不通过                            
 
           return false;                    
 
       if (O === L)    //判断：   Person.prototype === stu._proto_ ？      
 
            return true;   //此时，stu._proto_ 指向per实例对象，并不满足
 
        L = L.__proto__;  //令L=  stu._proto_._proto_，执行循环
 
   }                      //stu._proto_ ._proto_，看图示知：
 
}                        //指的就是Person.prototype，所以也返回true
```
stu instanceof Person返回值为true，这就证明了stu继承了Person。

stu instanceof Object也是同理的，根据图示即可轻易得出。

## 结论
### 1、instanceof 的作用
用于判断一个引用类型是否属于某构造函数；

还可以在继承关系中用来判断一个实例是否属于它的父类型。

### 2、和typeof的区别：
typeof在对值类型number、string、boolean 、null 、 undefined、 以及引用类型的function的反应是精准的；但是，对于对象{ } 、数组[ ] 、null 都会返回object

为了弥补这一点，instanceof 从原型的角度，来判断某引用属于哪个构造函数，从而判定它的数据类型。
