---
title: C#利用正则和反射给对象赋值
date: 2017/11/18 17:02:00
tags:
  - 编程
  - C#
  - 代码片段
---

如果类中的成员过多，一个属性一个属性的赋值太过繁琐，C#中可以利用反射给对象赋值。
　　
我们如果想利用凡是给一个对象属性赋值可以通过 ` PropertyInfo.SetValue(object obj, object value) ` 方式进行赋值。

<!--more-->
　　
##### 什么是反射：

> 反射提供了封装程序集、模块和类型的对象（Type 类型）。可以使用反射动态创建类型的实例，将类型绑定到现有对象，或从现有对象获取类型并调用其方法或访问其字段和属性。如果代码中使用了属性，可以利用反射对它们进行访问。

> 反射是.Net中获取运行时类型信息的方式，.Net的应用程序由：程序集(Assembly)、模块(Module)、类型(class)组成，而反射提供一种编程的方式，让程序员可以在程序运行期获得这几个组成部分的相关信息。

> Assembly类定义和加载程序集，获得正在运行的装配件信息，也可以动态的加载装配件，以及在装配件中查找类型信息，并创建该类型的实例。
Type类可以获得对象的类型信息，此信息包含对象的所有要素：方法、构造器、属性等等，通过Type类可以得到这些要素的信息，并且调用之。
Module了解包含模块的程序集以及模块中的类等，还可以获取在模块上定义的所有全局方法或其他特定的非全局方法。
ConstructorInfo了解构造函数的名称、参数、访问修饰符（如pulic 或private）和实现详细信息（如abstract或virtual）等。使用Type的GetConstructors或 GetConstructor方法来调用特定的构造函数。
> MethodInfo了解方法的名称、返回类型、参数、访问修饰符（如pulic 或private）和实现详细信息（如abstract或virtual）等。使用Type的GetMethods或GetMethod方法来调用特定的方法。
> PropertyInfo了解属性的名称、数据类型、声明类型、反射类型和只读或可写状态等，获取或设置属性值。
> FiedInfo了解字段的名称、访问修饰符（如public或private）和实现详细信息（如static）等，并获取或设置字段值。
> EventInfo了解事件的名称、事件处理程序数据类型、自定义属性、声明类型和反射类型等，添加或移除事件处理程序。
> ParameterInfo了解参数的名称、数据类型、是输入参数还是输出参数，以及参数在方法签名中的位置等。
> 诸如此类，还有FieldInfo、EventInfo等等，这些类都包含在System.Reflection命名空间下。

##### 反射的作用：

> 1. 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型
> 2. 应用程序需要在运行时从某个特定的程序集中载入一个特定的类型，以便实现某个任务时可以用到反射。
> 3. 反射主要应用与类库，这些类库需要知道一个类型的定义，以便提供更多的功能。

##### 实例：

假设我们有如下一个结构：

``` csharp
public class RegionConfig
{
    public string TOP { get; set; }
    public string RIGHT { get; set; }
    public string LEFT { get; set; }
    public string DOWN { get; set; }
    public string WIDTH { get; set; }
    public string HEIGHT { get; set; }
}

```

想要从以下字符串中找到相应的元素给以上结构的属性赋值：

``` csharp
string rgCfg = "TOP:topVal\nRIGHT:rigthVal\nLEFT:leftVal\nDOWN:downVal\nWIDTH=1200\nHEIGHT=1300";
```

可以使用以下代码利用正则和反射给对象赋值：

``` csharp

public static RegionConfig GetRegionConfig()
{
    string rgCfg = "TOP:topVal\nRIGHT:rigthVal\nLEFT:leftVal\nDOWN:downVal\nWIDTH=1200\nHEIGHT=1300";
    RegionConfig rConfig = new RegionConfig();

    Regex rgRCfg = new Regex(@"(.+)[:=](.+)\n{0,1}");
    MatchCollection mc = rgRCfg.Matches(rgCfg);

    foreach (Match m in mc)
    {
        string key = m.Groups[1].ToString();
        string value = m.Groups[2].ToString();

        // 利用反射赋值
        var property = rConfig.GetType().GetProperty(key);
        if (property != null) property.SetValue(rConfig, value);
    }

    return rConfig;
}
```

运行结果：
![运行结果](/assets/blogImg/csharpRegex.png)