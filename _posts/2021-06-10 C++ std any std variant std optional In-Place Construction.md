---
title: C++ std::any、std::variant和std::optional的原位构造（In-Place Construction）
categories: Programming Language
tags:
  - c++
  - c++17
  - in_place
date: 2021-06-10 20:13:32
---

本文翻译自 Bartlomiej Filipek 的博客文章 [In-Place Construction for std::any, std::variant and std::optional](https://www.bfilipek.com/2018/07/in-place-cpp17.html)，翻译已获作者授权。

------

当你读到关于 std::any、std::variant 或 std::optional的文章或者参考页面时，你可能会注意到它们有几个名为 in_place_* 的辅助类型可用于构造函数。

我们为什么需要这样的语法？它比“标准”的构造函数更有效吗？

**目录**

​	[介绍](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t0)

​	[std::optional](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t1)

​		[默认构造函数](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t2)

​		[不可复制/移动类型](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t3)

​		[有多个参数的构造函数](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t4)

​		[emplace() 方法](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t5)

​		[std::make_optional()](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t6)

[		其他](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t7)

​	[std::variant](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t8)

​		[歧义](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t9)

​		[复杂类型](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t10)

​		[其他](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#其他)

​	[std::any](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t11)

​		[复杂类型](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#复杂类型)

​		[std::make_any](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t12)

​		[其他](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#其他)

​	[总结](https://blog.csdn.net/yihuajack/article/details/117781831?spm=1001.2014.3001.5501#t13)

------

## 介绍

in_place 有三种辅助类型：

- std::in_place_t 类型和全局值 std::in_place，用于 std::optional
- std::in_place_type_t 类型和全局值 std::in_place_type，用于 std::variant 和 std::any
- std::in_place_index_t 类型和全局值 std::in_place_index，用于 std::variant

这些辅助类型用于有效地原位初始化对象，而无需额外的临时拷贝或移动操作。

让我们看看这些辅助类型是如何使用的。

## std::optional

std::optional 是一个包装器类型，所以你可以用和包装器对象几乎一样的方法创建 optional 对象。在大多数情况下你可以：

```c++
std::optional<std::string> ostr{"Hello World"};
std::optional<int> oi{10};
```

你可以对构造函数不加说明地将上述代码重写为：

```c++
std::optional<std::string> ostr{std::string{"Hello World"}};
std::optional<int> oi{int{10}};
```

这是因为 std::optional 有一个接受 U&& （右值引用，可以转换为 optional 中存储的类型）的构造函数。对于我们的例子，它被推导成可以初始化字符串的 const char* 类型。

所以在 std::optional 中使用 std::in_place_t 有什么好处呢？

至少有两点：

- 默认构造函数
- 对于有很多参数的构造函数的有效构造

### 默认构造函数

考虑一个有默认构造函数的类：

```c++
class UserName
{
public:
    UserName() : mName("Default")
    { 
 
    }
    // ...
};
```

你怎么创建一个包含 UserName{} 的 optional ？

你可以这样写：

```c++
std::optional<UserName> u0; // 空的 optional
std::optional<UserName> u1{}; // 也是空的

// 用“默认构造函数构造出的对象”构造出的 optional:
std::optional<UserName> u2{UserName()};
```

这个方法可以，但是它会创建额外的临时对象。上述代码的实际执行情况是：

```c++
UserName::UserName('Default')
UserName::UserName(move 'Default')  // 移动临时对象
UserName::~UserName('')             // 删除临时对象
UserName::~UserName('Default')
```

这段代码会创建一个临时对象然后把它移动到 optional 中存储的对象。

这里我们可以借助 std::in_place_t 来使用一种更有效的构造函数：

```c++
std::optional<UserName> opt{std::in_place{}};
```

这段代码的实际执行情况是：

```c++
UserName::UserName('Default')
UserName::~UserName('Default')
```

optional 中存储的对象是被原位创建出来的，和你调用 UserName{} 是一样的，不需要额外的拷贝或者移动。

你可以在 [@Coliru](http://coliru.stacked-crooked.com/a/00976d6c9c1b0c5f) 运行这些样例。

### 不可复制/移动类型

如你在上一节的例子里所见，如果你用一个临时对象来初始化 std::optional 中包含的值，那么编译器会使用移动或拷贝构造函数。

但是如果你的类型不允许这样做呢？比如 std::mutex 不可移动或复制。

在这种情况下，std::in_place 是处理这些类型的唯一办法。

### 有多个参数的构造函数

还有一种要用到 std::in_place 的情况是你的构造函数里有很多参数。optional 可以默认接受单个参数（右值引用），然后有效地把它传递给包装器类型。但是如果你想初始化 std::complex(double, double) 或者 std::vector 呢？

你可以创建一个临时拷贝然后在构造过程中传递它：

```c++
// 有4个1的 vector:
std::optional<std::vector<int>> opt{std::vector<int>{4, 1}};

// complex 类型:
std::optional<std::complex<double>> opt2{std::complex<double>{0, 1}};
```

或者用 in_place 和处理可变参数列表版本的构造函数：

```c++
template< class... Args >
constexpr explicit optional( std::in_place_t, Args&&... args );

// 或 initializer_list:

template< class U, class... Args >
constexpr explicit optional( std::in_place_t,
                             std::initializer_list<U> ilist,
                             Args&&... args );
```

```c++
std::optional<std::vector<int>> opt{std::in_place, 4, 1};
std::optional<std::complex<double>> opt2{std::in_place, 0, 1};
```

第二个版本很长，而且没有创建临时对象。临时对象不如原位构造有效，尤其是当对象很大或者是容器类型时。

### emplace() 方法

如果你想改变 optional 中存储的值，那么你可以用赋值运算符或者调用 emplace()。

遵循 C++11 引入的概念（容器的 emplace 方法），你可以高效地创建（并销毁旧值）一个新的对象。

### std::make_optional()

如果你不喜欢 std::in_place，你可以尝试 make_optional 工厂函数。

以下代码

```c++
auto opt = std::make_optional<UserName>();

auto opt = std::make_optional<std::vector<int>>(4, 1);
```

和

```c++
std::optional<UserName> opt{std::in_place};

std::optional<std::vector<int>> opt{std::in_place, 4, 1};
```

是同样有效的。make_optional 等效地实现了原位构造：

```c++
return std::optional<T>(std::in_place, std::forward<Args>(args)...);
```

同样，归功于[从 C++17 开始的强制拷贝优化](https://www.bfilipek.com/2017/06/cpp17-details-clarifications.html#guaranteed-copy-elision)，不会有临时对象参与进来了。

### 其他

std::optional 有8个版本的构造函数！如果你很勇你可以在 [@cppreference - `std::optional` constructor](https://en.cppreference.com/w/cpp/utility/optional/optional) 研究它们。

## std::variant

std::variant 有两个 in_place 辅助类型：

- std::in_place_type - 用于指定你想在 variant 里改变或者设定哪个类型
- std::in_place_index - 用于指定你想改变或者设定的索引。类型从0开始枚举。
  在 invariant std::variant<int, float, std::string> 中 - int 的索引是0，float的索引是1，string的索引是2。索引和 variant::index 方法的返回值是一样的。

幸运的是，你不用每次创建一个 variant 时都用这些辅助类型。它可以自动识别它能否由单个参数构造：

```c++
// 构造第二个参数 float:
std::variant<int, float, std::string> intFloatString { 10.5f };
```

对于 variant 我们在至少两种情况下需要辅助类型：

- 歧义 - 多个类型匹配，区分要创建哪个类型
- 高效创建复杂类型（类似 optional）

注意：variant 默认由第一个类型初始化 - 假设它有一个默认构造函数的话。如果没有可用的默认构造函数，那么编译器会报错。这和 std::optional 由一个空的 optional 初始化不同 - 如前所述。

### 歧义

如果你像这样初始化：

```c++
std::variant<int, float> intFloat { 10.5 }; // double 转换成？
```

10.5 可以被转换成 int 或 float，所以编译器会报告几页模板错误……但是大体上说，它不能推断 double 转换成什么类型。

但是你可以指明你要创建哪个类型来简单地处理这样的错误：

```c++
std::variant<int, float> intFloat { std::in_place_index<0>, 10.5 };

// 或

std::variant<int, float> intFloat { std::in_place_type<int>, 10.5 };
```

### 复杂类型

类似于 std::optional，如果你想高效创建对象接受多个构造函数参数，就用 std::in_place*：

例如：

```c++
std::variant<std::vector<int>, std::string> vecStr { 
    std::in_place_index<0>, { 0, 1, 2, 3 } // initializer list passed into vector
};
```

### 其他

std::variant 有8个版本的构造函数！如果你很勇你可以在 [@cppreference - `std::variant` constructor](https://en.cppreference.com/w/cpp/utility/variant/variant) 研究它们。

## std::any

和之前的两个类型的风格一样，std::any 可以用 std::in_place_type 来高效原位创建对象。

### 复杂类型

在下面的例子里要用到临时对象：

```c++
std::any a{UserName{"hello"}};
```

但是用

```c++
std::any a{std::in_place_type<UserName>,"hello"};
```

对象会由一组给定的参数原位创建。

### std::make_any

方便起见，std::any 有一个名为 std::make_any 的工厂函数返回

```c++
return std::any(std::in_place_type<T>, std::forward<Args>(args)...);
```

所以之前的例子可以被写成：

```c++
auto a = std::make_any<UserName>{"hello"};
```

make_any 很可能更容易使用。

### 其他

std::variant 有6个版本的构造函数（不是 variant 或者 optional 那样有8个）！如果你很勇你可以在 [@cppreference - `std::any` constructor](https://en.cppreference.com/w/cpp/utility/any/any) 研究它们。

Bonus：如果你对 C++17 很感兴趣可以看看：[下载 C++17 语言参考卡片的免费拷贝！](http://eepurl.com/cyycFz)

## 总结

C++11 程序员有了一个新技术来原位初始化对象，这可以避免不必要的临时对象拷贝，并且允许处理不可移动/复制类型。

到了 C++17 我们有了几个包装器类型 - std::any，std::optional 和 std::variant - 允许高效地原位创建对象。

如果你想发挥这些类型的全部效用，你最好学习一下怎么使用 std::in_place* 辅助类型或者调用 make_any 或 make_optional 得到同样的结果。

作为这个主题的参考，参见最近 Jason Turner 的 C++ Weekly 频道的视频：[C++ Weekly - Ep 123 - Using in_place_t](https://youtu.be/Fs2nhjIysKA)。本篇文章的讨论可见于[In-Place Construction for std::any, std::variant and std::optional : cpp (reddit.com)](https://www.reddit.com/r/cpp/comments/8z8mpb/inplace_construction_for_stdany_stdvariant_and/)。
