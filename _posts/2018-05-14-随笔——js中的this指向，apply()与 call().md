---
layout:     post                    # 使用的布局（不需要改）
title:      js中的this，apply()与call()                # 标题 

date:       2018-05-14              # 时间
author:     WZH                     # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:
    - 前端
    - JavaScript
---



####    js中apply和Math.max()函数（[原文](http://www.php.cn/js-tutorial-390323.html)）

1.  **apply()**

    `Function.apply()` 是JS的一个OOP特性，一般用来模拟继承和扩展this的用途，对于上面这段代码，可以这样去理解：
    `XXX.apply()` 是一个调用函数的方法，其参数为：`apply(Function, Args)`，Function为要调用的方法，Args是参数列表，**当Function为null时，默认为上文**，
    
    ```
    Math.max.apply(null, arr)
    ```
    
    可认为是
        
    ```
    apply(Math.max, arr)
    ```
    
    然后，arr是一个参数列表，对于max方法，其参数是若干个数，即:
    
    ```
    arr = [a, b, c, d, ...]
    ```
    
    代入原式中：
    
    ```
    Math.max.apply(null, [a, b, c, d, ...])
    ```
    
    等同于：
    
    ```
    Math.max(a, b, c, d, ...)
    ```

2.  **区别：**

    `Math.math()` 方法可以求出给定参数中最大的数，但是参数不能是数组。
    
    例如：
    
    ```
    > Math.max('1','2','3.1','3.2')
    < 3.2
    > Math.min(1,0,-1)
    < -1
    ```
    此时可用 `apply()` 解决（第一个参数为null，第二个参数为数组; `apply()` 如何实现的参见上文 `apply()` 的转换介绍）：
    
    ```
    > Math.max.apply(null, ['1','2','3.1','3.2'])
    < 3.2
    > Math.min.apply(null, [1,0,-1])
    < -1
    ```

####    JavaScript中的this（[原文](http://www.php.cn/js-tutorial-389965.html)）
1.  **this**

    在 JavaScript 中，处处使用者 this ，但是，很多时候 this 指向并不固定，而是随着它的执行环境的改变而改变。总结一句：**this总是指向调用它所在方法的对象**。

2.  **this** 在函数里
    
    这种方式也称为“全局性的函数调用”，例如：
    
    ```
    <script type='text/javascript'>
        var name='Hello_World';
        function test(){
            this.name='Hello_JavaScript';
        }
        
        test();
        
        console.log(this.name);     //Hello_JavaScript
        console.log(window.name);   //Hello_JavaScript
        console.log(name);          //Hello_JavaScript
    <script>
    ```
    
    通过结果可以更加证明了全局的name在函数内部被修改了，因为这个函数内部的this指的就是window。
    
    总结：**对于全局性函数调用，函数内部的this就是指的全局对象window，即是：this是调用函数所在的对象。实际上这个test()函数是由全局对象window来去调用的，那么函数内部的this当然就指的是window**。
 
    
3.  **this** 在构造函数里
    
    ```
    <script type='text/javascript'>
        var name='Hello_World';
        function test(){
            this.name='Hello_JavaScript';
        }
        
        var person = new test();
        
        console.log(person.name);     //Hello_JavaScript
        console.log(window.name);   //Hello_World
    <script>
    ```
    
    分析：我们通过new关键字创建一个对象的实例，可以发现new关键字改变了this的指向，将这个this指向了对象person。在构造函数内部，我们对this.name=“HelloWorld”进行重新赋值，并没有改变全局变量name的值。
    
    总结：**声明一个构造函数的实例对象时，构造函数内部的this都会指向新的实例对象，或者说，构造函数内部的this指向的是新创建的对象本身**。
    
4.  **在对象的方法中调用**

    ```
    <script type='text/javascript'>
        var name='Hello_World';
        
        var person= {
          name:'Hello_JavaScript',
          info:function(){
            alert(this.name);       
          }
        }
        
        person.info();        //Hello_JavaScript
    <script>
    ```
    
    总结：**当person对象调用info()函数时，info函数内部的this指向的就是person对象。即，当this出现在对象的方法中时，那么该方法内部的this指向的就是这个对象本身，也就是说this指向的调用函数的对象**。
    
####    JavaScript之call和apply（[原文](http://www.php.cn/js-tutorial-389965.html)）
    
1.  **使用call和apply的作用**
    
    上文提到 **this** 的出现的三种方式，总结为两种：
    
    *   直接调用
        
        `test()`, 此种调用方式中，函数内部的this指向的window
    *   构造函数形式的调用
    
        var person = new test(); 此种方式调用中，函数内部的this指向的是person

    以上两种方式，实际上可以这么说，**函数内部的this都是代表当前对象，只不过是JavaScript中函数内部的this会随着程序而指向不同的对象**。
    
    那么我的问题是：我们能不能手动修改this的指向呢？

    答案：可以的，使用call或者apply。这个也就是**call和apply的作用 → 改变函数内部this的指向**。

2.  **应用场景**
    
    ```
    //代码一
    <script type='text/javascript'>
        function Person(name,age){
          this.name=name;
          this.age=age;
        }
        
        function info(){
          alert(this.name +','+this.age);
        }
        
        var p1=new Person('jack','20');
    <script>
    ```
    
    问题：我现在想借用这个info方法来去实现对p1对象的打印，怎么做？

    方案一：直接调用info函数，即：`info()` ；通过上面的讲解，仔细想想，肯定不行，因为这样调用的话，info函数内部的this其实是指向的window。
    
    方案二：通过对象调用，即 `p1.info()` ；其实也不行，因为p1对象压根就没有info这个方法，p1对象只有name和age属性。
    
    解决方法如下：
    
     ```
    //代码二
    <script type='text/javascript'>
        function Person(name,age){
          this.name=name;
          this.age=age;
        }
        
        function info(){
          alert(this.name +','+this.age);
        }
        
        var p1=new Person('jack','20');
        p1.show=info;
        p1.show();
        
    <script>
    ```
    
    可以发现我们通过向p1对象添加了一个show属性，而这个show属性的值其实是一个函数的地址，是一个函数对象，然后通过p1.show()就可以实现打印了。此种方法确实可以实现功能，但是**这种方法是通过为p1对象添加属性完成的，如果仍然有类似的需求，是不是都要向p1对象添加属性来完成需求呢，这样就会导致p1对象的占用空间越来越大，所以方式并不优雅**。

    针对上面的问题，本质上就是想通过修改info函数内部的this指针的问题来完成对当前对象的一个打印，那么我们可以在不增加属性的方式上来完成功能，这个就需要使用到了call和apply。
    
3.  **call和apply的使用**

    *   功能：改变this指向，使用指定的对象调用当前函数
    *   语法：
    
        ```
        call([thisObj[,arg1[, arg2[, [,.argN]]]]])
        ```
        
        ```
        apply(thisObj[,argArray])
        ```
    *   说明：
    
        1.  两个方法的功能完全一样，唯一区别就是参数。

            对于第一个参数来说thisObj，作用是一样的，用作代表当前对象的对象，说白了就表示的是函数执行时，this指向谁。
        
            对于 **第二个参数** ，**apply要求传入的是一个参数数组** ，也就是说将一系列参数组成一个数组传入，而 **对于call来说，参数以散列的值的方式传入** 。例如，func(thisObj，arg1，arg2，arg3...)对应的apply用法就是func(thisObj，[arg1，arg2，arg3...])。本文以call方法为例
        
        2.  这两个方法都是Function对象中的方法，因为我们定义的每个对象都拥有该方法。
    
        3.  call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象，如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。(?)
        
        4.  使用call和apply解决上面代码的问题
        
        ```
        //代码三
        <script type='text/javascript'>
            function Person(name,age){
              this.name=name;
              this.age=age;
            }
            
            function info(){
              alert(this.name +','+this.age);
            }
            
            var p1=new Person('jack','20');
            
            info.call(p1);      //或者 info.apply(p1)
            
        <script>
        ```
        
        分析：
        
        **当在函数中调用call方法时，函数内部的this会自动指向call方法中的第一个参数。上面的例子中，当执行info.call(p1)时，info函数内部的this则会自动指向p1对象，所以当然就可以call这种方式来完成对p1对象的打印。**
        
        5.  call()重写
        
        ```
        <script type='text/javascript'>
            function Person(){
                this.name = 'Hello_World';
                this.info=function(){
                    alert(this.name)
                }
            }
            
            function Cat(){
                this.name='猫';
            }
            var cat =new Cat();
            
            Person.call(cat);
            
            cat.info();    //Hello_World
            
        <script>
        ```
        
        上面代码最终会打印：‘Hello_World’，这个答案最关键是 `Person.call(cat)` 这一行代码，仔细去想想究竟发生了什么事情，当调用call方法时，函数Person内部的this其实已经自动的指向了cat对象，相当于就是给cat对象执行了下面的两行代码：
        
        ```
        this.name='Hello_World',
        this.info=function(){
            alert(this.name)
        }
        ```
        然后重写了原来cat对象中的name属性，把name由“猫”改成了“ Hello_World ”，而且并获得了一个新的info方法(可以这么理解，相当于给cat对象添加了一个info属性)，所以cat对象当然可以调用info方法了，所以结果就是“ Hello_World ”。apply的使用和call的功能相同，使用方式也很类似。
        
        
        
        
