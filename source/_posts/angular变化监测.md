---
title: angular变化检测
date: 2020-03-29 21:24:02
tags: angular
---
angular提供了数据绑定的功能，即将组件类的数据和页面的DOM元素关联起来。当数据发生变化时，angular能够监测到这些变化，并对其所绑定的DOM元素进行相应的更新，反之亦然。

<!--more-->
## angular变化监测的过程：
数据发生变化———>NgZone监听异步事件的执行———>触发angular变化检测———>变化监测的响应处理


## 数据变化的源头
1、用户的行为操作，如click、change、hover等
2、前后端的数据交互，如XMLHttpRequest、WebSocket
3、各类定时任务，如setTimeout、setInterval、requestAnimationFrame
&emsp;这三种可能导致数据变化的情景有一个共同的特征，即它们都是异步的处理。任意一个异步操作，都有可能在数据层面上发生变化，这样导致应用程序状态被改变。如果可以在每一个异步回调函数执行结束后，通知angular内核进行变化监测，那么任何数据的更改就可以在视图层实时地反馈出来。

## 变动通知机制
&emsp;通过异步事件来通知angular进行变化监测，实际上angular本身并不具备这样的捕获异步事件的机制，于是，angular引入了Zone.js这个异步事件库来解决。
&emsp;Zone.js运行时重写所有的浏览器异步事件API（如setTimeout、XHR等），所以就可以感知到这些异步事件的执行及其上下文。一旦有异步事件发生，即可在适当的时机触发angular变化检测。
&emsp;Zone.js记录这些异步事件发的上下文环境是在一个名为“Zone”的对象里完成的，每个Zone对象都保存了异步事件的执行信息，以及暴露了异步事件执行的生命周期钩子。Zone.js初始化时会生成一个初始的Zone对象，称为Root Zone。Zone对象可以通过fork操作生成子Zone，异步事件都是挂靠在某个Zone（Root Zone或子Zone都可以）之下运行的，而不同的Zone之间有一定的隔离性。
&emsp; Zone。这意味着Angular应用中触发的所有异步事件都可被Angular Zone感知（可在异步事件执行后进行一些必要操作，如触发变化监测等），而Root Zone或其他子Zone是感知不到Angular的异步事件执行的（即不会触发变化监测）。了解这点至关重要。
&emsp;为了管理Root Zone和子Zone,Angular为Zone.js又封装了一层，称为NgZone。NgZone本身是一个服务，除管理各个Zone对象外，NgZone还封装了一些支持Angular运行的友好事件，当有异步任务发生，完成或者抛出异常时，都可以监听对应的事件并进行捕获处理。这些事件列举如下：
* onUnstable：当代码进入angular的执行环境时被触发，在VM（JavaScript Virtual Machine）中最先触发，即通常在异步任务执行前触发。
* onMicrotaskEmpty：当VM中Microtasks队列为空时被触发，用于提示angular执行变化监测。
* onStable：当最后一个onMicrotaskEmpty已经执行完成并且Microtasks队列为空时被触发，这个事件仅仅被触发一次，即通常在异步事件执行完成且变化监测已经执行完成时触发。
* onError：当有错误异常抛出时触发。
&emsp;在默认情况下，几乎每一次异步事件触发都会执行变化监测，有些时候这并不是开发者所需要的，如mousemove、scroll事件，如果这些事件频繁地触发变化监测，就很容易导致页面卡顿。因此，NgZone提供了run()和runOutsideAngular()两个方法帮助开发者控制变化监测是否执行。
&emsp;Angular应用默认运行在Angular Zone下，所以只有在Angular Zone下触发的事件才会触发变化监测，而在Root Zone下触发的事件不会触发变化监测。runOutsideAngular()方法让angular应用绑定的事件逃离Angular Zone执行，即在Root Zone下执行。需要注意的是，一旦执行了runOutsideAngular()方法，在该回调函数里后续新绑定的异步事件也会一直在Root Zone下执行，直到遇到run()方法。run()方法对runOutsideAngular()进行还原操作，将这些逃离的异步事件重新挂靠到Angular Zone下执行。
```
<div (mousedown)="doMouseDown($event)" (mouseup)="doMouseUp($event)"></div>

doMouseDown(event){
    this.zone.runOutsideAngular(() => {
        window.document.addEventListener('mouseover', this.doMouseMove.bind(this));
    })
}
```
&emsp;在默认情况下，所有的异步事件都是在Angular Zone下执行的，每当有异步事件触发时，无论是否导致了数据变化，都会指向变化监测逻辑。
&emsp;变化监测的入口函数定义在ApplicationRef这个服务类里，函数名为tick()。
&emsp;Angular变化监测是以组件为单元的，在默认的变化监测策略里，每一次变化监测都从根组件开始，以深度优先的原则向子组件遍历，直至整棵组件树的所有组件都执行一次变化监测。有时应用不需要所有组件都执行一次变化监测，所有可以通过一些策略调整来提升变化监测的性能。

## 变化监测的响应处理
### 变化监测的处理机制
&emsp;Angular由大大小小的组件组成，这些有相互依赖关系的组件组成了一棵线性的组件树。此外，每一个组件都有自己的变化监测器，由此组成了一棵变化监测树。变化监测树的数据是由上到下单向流动的，这是因为变化监测的执行总是由根组件开始，从上到下监测每一个组件的变化。

### 变化监测类
&emsp;Angular在整个运行期间会为每一个组件创建变化监测类的实例，该实例提供了相关的方法来手动管理变化监测。
变化监测类ChangeDetectorRef提供的主要接口如下：
* markForCheck()：标记根组件到该组件之间的这条路径，通知angular在下次触发变化监测时必须检查这条路径上的组件。
* detach()：从变化监测树中分离变化监测器，该组件的变化监测器将不再执行变化监测，除非手动执行reattach()
* reattach()：把分离的变化监测器重新安装上，使得该组件及其子组件都能执行变化监测。
* detechChanges()：手动触发执行该组件到各个子组件的一次变化监测
&emsp;当数据发生变化时，Angular并不知道哪个组件发生了变化，但开发者知道，所以可以给这个组件做标记，以此来通知Angular仅仅监测这个组件所在路径上的组件即可。
```
export class ListComponent implements OnInit {
    contacts: any = {};

    constructor(
        private cd: ChangeDetectorRef
    ){
        // 从当前组件中分离变化监测器
        cd.detach();
        // 在需要变化监测的时候再通过detectChanges()手动触发变化监测
        setInterval(() => {
            this.cd.detectChanges();
        }, 5000);
    }

    ngOnInit(){
        this.getContacts();
    }

    getContacts(){
        this.contacts = data;
    }
}
```

### 变化监测策略
&emsp;@Component装饰器中的元，数据changeDetection，它的作用是让开发者定义每个组件的变化监测策略。这个元数据的值是一个枚举，是ChangeDetectionStrategy对象的Default和OnPush。当值为Default时，组件的每次变化监测都会检查其内部的所有数据（引用对象也会被深度遍历），以此得到前后的数据变化情况；而当值为OnPush时，组件的变化监测只检查输入属性（即@Input修饰的变量）的值是否发生变化，当这个值为引用类型时，则只对比该值的引用。
&emsp;显然，OnPush策略相比Default降低了变化监测的复杂度，很好地提升了变化监测的性能。如果子组件的更新只依赖输入属性的值，那么在子组件上使用OnPush策略是一个很好的选择。但是OnPush只对比值的引用，如果这个值是引用类型时，可能会达不到预期效果。如果希望子组件能正常更新数据，解决办法有两个：
* 修改变化监测策略为Default，但这样会降低性能。
* 使用Immutable对象来传值，这是比较推荐的做法。
