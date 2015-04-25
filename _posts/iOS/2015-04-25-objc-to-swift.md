---
layout: post
title: 从Objc到Swift
category: iOS
tags: [iOS, Swift]
keywords: iOS,Swift
description: 
---

> Swift学习笔记


###Swift语言特性###

**1）可选**
一个可选的Int被协作为Int?而不是Int。可以通过if来判断一个可选是否包含值，如果包含，可通过后缀感叹号(!)来取值。

**2）区间**
闭区间运算符 a...b 定义一个包含从 a 到 b (包括 a 和 b)的所有值。
半闭区间 a..b 定义一个从 a 到 b 但不包括 b 的区间. 之所以称为半闭区间, 是因为该 区间包含第一个值而不包括最后的值.

**3）数组**
Swift数组不同于NSArray，类型必须确定，如Array<Int>

**4）enumerate 函数**
enumerate 返回一个由每一个数据项索引值和数据值组成的键值对组，如
     for (index, value) in enumerate(shoppingList)

**5）switch**
Swift 的 switch 语句比 C 语言中更加强大。在 C 语言中,如果􏿒个 case 不小心漏写了 break,这个 case 就会“掉入”下一个 case,Swift 无需写 break,所以不会发生这种“掉入”的情况。Case 还可以匹配更多的类型模式,包括范围(range)匹配,元组(tuple)和特定类型的􏿓述。

**6）Fallthrough**

**7）内部形参名称/外部形参名称**
func someFunction(externalParameterName localParameterName: Int) {
}
如果内部外部使用相同名称，可以用#作为前缀
func someFunction(#parameterName: Int) {
}

**8）默认形参值**
有默认值的形参如果没有提供外部形参名称，Swift将自动提供一个同名的外部形参名称（效果和#前缀一样）。

**9）In-Out形参**
in-out参数不能有默认值，如
func swapTwoInts(inout a: Int, inout b: Int)
调用时参数加前缀&，如
swapTwoInts(&someInt, &anotherInt)

**10）闭包**














**11）Trailing闭包**
如果您需要将一个很长的闭包表达式作为最后一个参数传递给函数,可以使用 trailing 闭包
 来增强函数的可读性。


**12）捕获(Caputure)**
 闭包可以在其定义的上下文中捕获常量或变量。 即使定义这些常量和变量的原作用域已经 不存在,闭包仍然可以在闭包函数体内引用和修改这些值。

**13）枚举**
不像 C 和 Objective-C 一样,Swift 的枚举成员在被创建时不会被赋予一个默认的 整数值。

**14）枚举的关联值（Associated Values）**
每个成员的数据类型可以是各不相同的，如

定义变量时，

在switch枚举的时候，可以通过let/var获取关联值

或使用一个let


**15）枚举的原始值（Raw Values）**

原始值必须是相同类型的。

原始值与关联值的区别：原始值和关联值是不相同的。当你开始在你的代码中定义枚举的时候原始值是被预先 填充的值,像上述三个 ASCII 码。对于一个特定的枚举成员,它的原始值始终是相同的。 关联值是当你在创建一个基于枚举成员的新常量或变量时才会被设置,并且每次当你这么做 得时候,它的值可以是不同的。

获取枚举成员的原始值


通过原始值找到枚举成员

fromRaw返回时是可选类型，上面possiblePlanet是Planet?类型



**16）结构体**
结构体是值类型。Swift 中,所有的基本类型:整数(Integer)、浮点数(floating-point)、布尔值(Booleans)、字符串(string)、数组(array)和字典(dictionaries),都是值类型,并且都是以结构体的形式在后台所实现。
注意：NSArray/NSDictionary是类，为引用类型。

**17）类**
类是引用类型。

18）等价于（===）和等于（==）
“等价于”表示两个类类型(class type)的常量或者变量引用同一个类实例。 “等于”表示两个实例的值“相等”或“相同”,判定时要遵照类设计者定义定义的评判标准,因 此相比于“相等”,这是一种更加合适的叫法。