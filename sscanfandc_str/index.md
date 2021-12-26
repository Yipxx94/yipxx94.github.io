# 巧用sscanf()和c_str()解题


<!--more-->


## `sscanf()`

### 描述

从字符串中读取格式化数据。

### 函数声明

`int sscanf(const char *str, const char *format, ...)`

从`str`读取数据并根据参数格式将它们存储到附加参数给定的位置，就像使用了`scanf`一样，但从`str`而不是标准输入`(stdin)`读取。

### 参数

* `str`：C 字符串，是函数检索数据的源。

* `format`：C 字符串，该字符串遵循与`scanf`中的格式相同的规范。

`format`说明符形式为`[=%[*][width][modifiers]type=]`，具体讲解如下：

| 参数 | 描述 |
| :----: | :----: |
| `*` | 这是一个可选的星号，表示数据是从流`stream`中读取的，但是可以被忽视，即它不存储在对应的参数中。 |
| `width` | 这指定了在当前读取操作中读取的最大字符数。 |
| `modifiers` | 为对应的附加参数所指向的数据指定一个不同于整型（针对 d、i 和 n）、无符号整型（针对 o、u 和 x）或浮点型（针对 e、f 和 g）的大小： h ：短整型（针对 d、i 和 n），或无符号短整型（针对 o、u 和 x） l ：长整型（针对 d、i 和 n），或无符号长整型（针对 o、u 和 x），或双精度型（针对 e、f 和 g） L ：长双精度型（针对 e、f 和 g）。 |
| `type` | 一个字符，指定了要被读取的数据类型以及数据读取方式。 |

### 返回值

如果成功，该函数返回成功匹配和赋值的个数。如果到达文件末尾或发生读错误，则返回`EOF`。

### 实例

1. 提取时间

```
int y, m, d;
char str[] = "2021-12-22";
sscanf(str, "%d-%d-%d", &y, &m, &d);
printf("%d\n%d\n%d", y,m,d);
结果：
    y = 2021, m = 12, d = 22;
```

2. 分割字符串

```
char sentence[] = "Zhangsan is 12 years old.";
char str[20];
int i;
sscanf(sentence, "%s %*s %d", str, &i);
printf("%s -> %d\n", str, i);
结果：
	Zhangsan -> 12
```



## `c_str()`

### 描述

`c_str()`是`Borland`封装的`String`类中的一个函数，它返回当前字符串的首字符地址。换种说法，`c_str()`函数返回一个指向正规C 字符串的指针常量，内容与本`string`串相同。这是为了与C 语言兼容，在C 语言中没有`string`类型，故必须通过`string`类对象的成员函数`c_str()`把`string`对象转换成C 中的字符串样式。

### 函数声明

```
const char *c_str() const
```

### 参数

该函数不接受任何参数。

### 返回值

该函数返回一个指向数组的指针，该数组包含一个以空字符结尾的字符序列（即 C 字符串），表示字符串对象的当前值。

{{< admonition >}}

操作`c_str()`函数的返回值时，只能使用C 字符串的操作函数，如，`strcpy()`等函数。因为`string`对象可能在使用后被析构函数释放掉，那么你所指向的内容就具有不确定性。

{{< /admonition >}}

### 实例

**示例1**

```
#include <iostream>
#include <cstring>
#include <string>

using namespace std;

int main()
{
	string str = "Please divide this sentance into parts";
	
	char * cstr = new char[str.length() + 1];
	strcpy(cstr, str.c_str());
	
	char * p = strtok(cstr, " ");
	while (p != 0)
	{
		cout << p << '\n';
		p = strtok(NULL, " ");
	}
	
	delete[] cstr;
	return 0;
}
```

输出：

```
Please
divide
this
sentance
into
parts
```

**示例2**

```
#include <iostream>
#include <cstring>
#include <string>

using namespace std;

int main()
{
	string s1 = "Aditya";
	
	for (int i = 0; i < s1.length(); i ++ )
	{
		cout << "The " << i + 1 << "th character of string " << s1 << " is " << s1.c_str()[i] << endl;
	}
	return 0;
}
```

输出：

```
The 1th character of string Aditya is A
The 2th character of string Aditya is d
The 3th character of string Aditya is i
The 4th character of string Aditya is t
The 5th character of string Aditya is y
The 6th character of string Aditya is a
```



