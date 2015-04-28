---
layout: post
title: JUnit4的TestSuite和顺序控制
category: JUnit4
tags: [JUnit]
keywords: JUnit
description: 
---

> Junit的测试包、以及测试方法的顺序控制

## TestSuite

在JUnit4中，如果想要同时运行多个测试类，需要使用两个注解：

@RunWith(Suite.class)指定使用Suite运行器来运行测试；

@SuiteClasses(ClassName.class)指定运行哪些测试类。测试类可以指定为Suite，这样JUnit会继续再去寻找里面的测试类，一直找到能够执行的Test Case并执行之。

```
@RunWith(Suite.class)
// 指定运行器
@Suite.SuiteClasses({ CalculatorTest.class, ParametersTest.class })
// 指定要测试的类
public class TestAll
{
}
```

## 顺序控制

从4.11版本开始,JUnit将默认使用一个确定的,但不可预测的顺序( MethodSorters.DEFAULT )。
要改变测试执行的顺序只需要在测试类(class)上使用 @FixMethodOrder 注解,并指定一个可用的MethodSorter即可:

@FixMethodOrder(MethodSorters.JVM) : 保留测试方法的执行顺序为JVM返回的顺序。每次测试的执行顺序有可能会所不同。

@FixMethodOrder(MethodSorters.NAME_ASCENDING) :根据测试方法的方法名排序,按照词典排序规则(ASC,从小到大,递增)。