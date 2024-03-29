---
layout: post
title: Delphi中的函数指针判断是否为空
categories: delphi之函数 delphi之指针与内存
tags: delphi 函数 指针
---


delphi函数指针 只有@@p才代表了函数指针本身的地址  

assigned(p) 判断是否为空

或者用 @p=nil 来判断函数指针是不是为空

--- 

Delphi中的函数指针实际上就是指针，只是在使用的时候有些不同

函数指针要先定义一个函数类型，比如

    type
        TTestProc = procedure of object;

这是一个最简单的函数类型，没有参数，也没有返回值，并且要求是类的成员函数

类的成员函数其实就代表了调用的时候参数的不同，因为类的成员函数隐含着一个对象参数，而不是显式写明，函数都是静态的。

当然了，如果有重载就变成了虚函数指针表，其中的调用就复杂一些

函数类型可以定义一个函数指针变量

    var
        p: TTestProc;

这个指针变量是4自己的 Pointer。可以与 Pointer直接做转换，但是要加上一个 @，比如：

    var
        p: TTestProc;
        p1: Poniter;
    begin
        p1:= @p;
        @p:= p1;
    end;

这里的 p1 是一个 Pointer类型

当 p 被赋值成一个真正的函数之后，就可以使用了，如下

    p();

如果有参数可以直接加上参数，与普通的函数调用方法没有什么区别，如果需要取得函数指针本身的地址就需要

    @@p;

加一个 @ 其实就是为了防止歧义，因为 p 本身也可以当成函数来使用，所以用 @p 来代表指针，不过特殊情况下p 也可以代表一个指针，比如

    Assigned(p);

这时候没有歧义，所以不需要加上 @，当然也可以使用

    Assigned(@p);

其实 assigned() 函数的参数要求是一个指针变量，用来判断这个指针是不是为 nil，如果是则返回 False，如果不是则返回 True

　　
###总结：###

p和 @p 都代表函数指针，只有@@p 才代表函数指针本身的地址，为了不产生歧义，所以有的时候需要使用 @p，有时候使用 p（比如 assigned(p)）