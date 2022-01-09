# C++的Lambda表达式


<!--more-->


## 前言

c++在c++11标准中引入了`lambda`表达式，一般用于定义匿名函数，使得代码更加灵活简洁。`lambda`表达式与普通函数类似，也有参数列表、返回值类型和函数体，只是它的定义方式更简洁，并且可以在函数内部定义。

lambda 来源于函数式编程的概念，也是现代编程语言的一个特点。

### `lambda`的优点

* 声明式编程风格：就地匿名定义目标函数或函数对象，不需要额外写一个命名函数或者函数对象。以更直接的方式去写程序，好的可读性和可维护性。

* 简洁：不需要额外再写一个函数或者函数对象，避免了代码膨胀和功能分散，让开发者更加集中精力在手边的问题，同时也获取了更高的生产率。

* 在需要的时间和地点实现功能闭包，使程序更灵活。

## `lambda`的基本概念

一个`lambda`表达式表示一个可调用的代码单元。我们可以将其理解为一个未命名的**内联函数**。与任何函数类似，一个`lambda`具有一个返回类型、一个参数列表和一个函数体。但与函数不同，`lambda`可能定义在函数内部。

一个`lambda`表达式具有如下形式：

```
[capture list](parameter list) -> return type {function body}
```

* capture list：捕获列表，是一个`lambda`所在函数中定义的局部变量的列表，通常为空。
* parameter list：参数列表，与普通函数一样。

我们可以忽略**参数列表**和**返回类型**，但必须永远包含捕获列表和函数体。

```
auto f = [] {return 27; };
cout << f() <<endl; 	#打印27
```

## 向`lambda`传递参数

与一个普通函数调用类似，调用一个`lambda`时给定的实参被用来初始化`lambda`的形参。通常，实参和形参的类型必须匹配。但与普通函数不同，`lambda`不能有默认参数。因此，一个`lambda`调用点实参数目永远与形参数目相等。

如：

```
[](const string& a, const string& b){
	return a.size() < b.size();
}
```

* 空捕获列表表明此`lambda`不使用它所在函数中的任何局部变量。

可以使用此`lambda`来调用`stable_sort`：

```
//按长度排序，长度相同的单词维持字典序
stable_sort(words.begin(), words.end(), [](const string& a, const string& b){
	return a.size() < b.size();
});
```

当`stable_sort`需要比较两个元素时，它就会调用给定的这个`lambda`表达式。

## 使用`lambda`捕获列表

**值捕获**：类似参数传递，变量的捕获方式也可以是值或引用。采用值捕获的前提是变量可以拷贝，与参数不同，被捕获的变量的值是在`lambda`创建时拷贝，而不是调用时拷贝。

**引用捕获**：如果采用引用方式捕获一个变量，就必须确保被引用的对象在`lambda`执行的时候是存在的。<u>`lambda`捕获的都是局部变量，这些变量在在函数结束后就不复存在了。</u>

**隐式捕获**：& 告诉编译器采用引用捕获方式，= 则表示采用值捕获方式。

**Tips**：当我们混合使用隐式捕获和显示捕获时，捕获列表中的第一个元素必须是一个 & 或 = 。此符号指定了默认捕获方式为引用或值。

`lambda`表达式还可以通过捕获列表捕获一定范围内的变量：

* []：不捕获任何变量。
* [&]：捕获外部作用域中所有变量，并作为引用在函数体中使用（按引用捕获）。
* [=]：捕获外部作用域中所有变量，并作为副本（**复制**）在函数体中使用（按值捕获）。
* [=, &x]：按值捕获外部作用域中所有变量，并按引用捕获x变量。
* [&, x]：默认以引用捕获所有变量，但是x是例外，通过复制捕获（按值捕获）。
* [x]：按值捕获x变量，同时不捕获其他变量。
* [x...]：以包展开方式复制捕获参数包变量。
* [&x]：仅以引用捕获x，其它变量不捕获。
* [this]：捕获当前类中的 this 指针，让`lambda`表达式拥有和当前类成员函数同样的访问权限。如果已经使用了 & 或者 =，就默认添加此选项。捕获 this 的目的是可以在`lambda`中使用当前类的成员函数和成员变量。
* [*this]：通过复制方式捕获当前对象。

{{< admonition >}}
一个`lambda`只有在其捕获列表中捕获一个它所在函数中的局部变量，才能在函数体中使用该变量。
{{< /admonition >}}

## 实例

有一个按单词长度从小到大排列的字符串数组words(`vector<string> words`)。

1. 调用 find_if

使用此`lambda`，我们可以查找第一个长度大于`sz`的元素：

```
//获取一个迭代器，指向第一个满足size() >= sz的元素
auto f = find_if(words.begin(), words.end(), [sz](const string& a){
	return a.size() >= sz;
});
```

这里对 find_if 的调用返回一个迭代器，指向第一个长度不小于给定参数`sz`的元素。如果这样的元素不存在，则返回 `words.end()`（尾迭代器）的一个拷贝。

2. for_each 算法

打印 words 中长度大于等于`sz`的元素。

```
//打印长度大于等于给定值的单词，每个单词后面接一个空格
for_each(f, words.end(), [](const string& s){
	cout << s << " ";
});
```

此`lamnda`中的捕获列表为空，是因为我们只对`lambda`所在函数中定义的（非`static`）变量使用捕获列表。

{{< admonition >}}
捕获列表只用于局部非`static`变量，`lambda`可以直接使用局部`static`变量和在它所在函数之外声明的名字。
{{< /admonition >}}

## 可变`lambda`

默认情况下，对于一个值被拷贝的变量，`lambda`不会改变其值。如果我们希望能改变一个值捕获的变量的值，就必须在参数列表后加上关键字`mutable`。

```
int main()
{
	int t = 27;	//局部变量
	//f可以改变它所捕获的变量的值
	auto f = [t]() mutable { return t++; };
	cout << f() << endl;
	cout << f() << endl;
	cout << t << endl;
	
	return 0;
}
```

结果：

```
28
29
27
```

一个引用捕获的变量是否可以修改依赖于此引用指向的是一个`const`类型还是一个非`const`类型。

## 指定`lambda`的返回类型

默认情况下，如果一个`lambda`体包含`return`之外的任何语句，则编译器假定此`lambda`返回`void`。

```
auto f = [](int i){
	return i < 0 ? -i : i;
};
```

在本例中，`lambda`体是单一的`return`语句，返回一个条件表达式的结果。我们无须指定返回类型，因为可以根据条件运算符的类型推断出来。

```
//错误：不能判断lambda的返回类型
auto f = [](int i){
	if (i < 0)
		return -i;
	else
		return i;
};
```

编译器推断这个版本的`lambda`返回类型为`void`，但它返回了一个`int`值。

当我们需要为一个`lambda`定义返回类型时，必须使用**尾置**返回类型：

```
auto f = [](int i) -> int {
	if (i < 0)
		return -i;
	else
		return i;
};
```

## `lambda`表达式的类型

`lambda`表达式的类型在 C++11 中被称为“闭包类型（Closure Type）”。

因此，我们可以认为它是一个带有`operator()`的类，即仿函数。因此，我们可以使用`std::function`和`std::bind`来存储和操作`lambda`表达式：

```
std::function<int(int)>  f1 = [](int a){ return a; };
std::function<int(void)> f2 = std::bind([](int a){ return a; }, 123);
```

另外，对于没有捕获任何变量的 lambda 表达式，还可以被转换成一个普通的函数指针：

```
using func_t = int(*)(int);
func_t f = [](int a){ return a; };
f(123);
```

`lambda`表达式可以说是就地定义仿函数闭包的**”语法糖“**。它的捕获列表捕获住的任何外部变量，最终均会变为闭包类型的成员变量。而一个使用了成员变量的类的`operator()`，如果能直接被转换为普通的函数指针，那么`lambda`表达式本身的 this 指针就丢失掉了。而没有捕获任何外部变量的`lambda`表达式则不存在这个问题。

这里也可以很自然地解释为何按值捕获无法修改捕获的外部变量。因为按照 C++ 标准，`lambda`表达式的`operator()`默认是`const`的。一个`const`成员函数是无法修改成员变量的值的。而`mutable`的作用，就在于取消`operator()`的`const`。

需要注意的是，没有捕获变量的 lambda 表达式可以直接转换为函数指针，而捕获变量的 lambda 表达式则不能转换为函数指针：

```
typedef void(*Ptr)(int*);
Ptr p = [](int* p){delete p;};	// 正确，没有状态的lambda（没有捕获）的lambda表达式可以直接转换为函数指针
Ptr p1 = [&](int* p){delete p;};	// 错误，有状态的lambda不能直接转换为函数指针
```


