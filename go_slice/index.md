# Go的切片在函数传递中的用法


<!--more-->


## 前言

在力扣刷题的过程中，如果像使用C++的引用一样来使用go的切片来进行递归，有时候会出现问题。下面我来详细的解释一下。

## Slice——切片

切片（Slice）是一个拥有相同类型元素的可变长度的序列。它是基于数组类型做的一层封装。它非常灵活，支持自动扩容。

切片是一个引用类型，它的内部结构包含**地址**、**长度**和**容量**。切片一般用于快速地操作一块数据集合。

### 定义

```
var name []T
```

* name：表示变量名
* T：表示切片中的元素类型，如，int

### 切片的长度和容量

切片拥有自己的长度和容量，我们可以通过使用内置的`len()`函数求长度，使用内置的`cap()`函数求切片的容量。

### 切片表达式

切片表达式从字符串、数组、指向数组或切片的指针构造子字符串或切片。它有两种变体：一种指定`low`和`high`两个索引界限值的简单的形式，另一种是除了`low`和`high`索引界限值外还指定容量的完整的形式。

#### 简单切片表达式

切片的底层就是一个数组，所以我们可以基于数组通过切片表达式得到切片。 切片表达式中的`low`和`high`表示一个索引范围（左闭右开），得到的切片`长度=high-low`，容量等于得到的切片的底层数组的容量。

如：

```
func main() {
	a := [5]int{1, 2, 3, 4, 5}
	s := a[1:3]  // s := a[low:high]
	fmt.Printf("s:%v len(s):%v cap(s):%v\n", s, len(s), cap(s))
}
```

输出：

```
s:[2 3] len(s):2 cap(s):4
```

可以省略切片表达式中的任何索引。省略了`low`则默认为0；省略了`high`则默认为切片操作数的长度:

```
a[2:]  // 等同于 a[2:len(a)]
a[:3]  // 等同于 a[0:3]
a[:]   // 等同于 a[0:len(a)]
```

{{< admonition >}}
对于数组或字符串，如果`0 <= low <= high <= len(a)`，则索引合法，否则就会索引越界（out of range）。
{{< /admonition >}}

#### 完整切片表达式

对于数组，指向数组的指针，或切片a（**注意不能是字符串**）支持完整切片表达式。

```
a[low:high:max]
```

上述代码会将得到的结果切片的容量设置为`max-low`。在完整切片表达式中只有第一个索引值`（low）`可以省略；它默认为0。

如：

```
func main() {
	a := [5]int{1, 2, 3, 4, 5}
	t := a[1:3:5]
	fmt.Printf("t:%v len(t):%v cap(t):%v\n", t, len(t), cap(t))
}
```

输出：

```
t:[2 3] len(t):2 cap(t):4
```

完整切片表达式需要满足的条件是`0 <= low <= high <= cap(a) <= max`，其他条件和简单切片表达式相同。

## append()函数为切片添加元素

Go语言的内置函数`append()`可以为切片动态添加元素。 可以一次添加一个元素，可以添加多个元素，也可以添加另一个切片中的元素。

```
func main(){
	var s []int
	s = append(s, 1)        // [1]
	s = append(s, 2, 3, 4)  // [1 2 3 4]
	s2 := []int{5, 6, 7}  
	s = append(s, s2...)    // [1 2 3 4 5 6 7]
}
```

**通过var声明的零值切片可以在append()函数直接使用，无需初始化。**

{{< admonition info >}}
每个切片会指向一个底层数组，这个数组的容量够用就添加新增元素。当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略进行“扩容”，此时该切片指向的底层数组就会更换。“扩容”操作往往发生在append()函数调用时，所以我们通常都需要用原变量接收append函数的返回值。
{{< /admonition >}}

如：

```
func main() {
	//append()添加元素和切片扩容
	var numSlice []int
	for i := 0; i < 10; i++ {
		numSlice = append(numSlice, i)
		fmt.Printf("%v  len:%d  cap:%d  ptr:%p\n", numSlice, len(numSlice), cap(numSlice), numSlice)
	}
}
```

输出：

```
[0]  len:1  cap:1  ptr:0xc0000a8000
[0 1]  len:2  cap:2  ptr:0xc0000a8040
[0 1 2]  len:3  cap:4  ptr:0xc0000b2020
[0 1 2 3]  len:4  cap:4  ptr:0xc0000b2020
[0 1 2 3 4]  len:5  cap:8  ptr:0xc0000b6000
[0 1 2 3 4 5]  len:6  cap:8  ptr:0xc0000b6000
[0 1 2 3 4 5 6]  len:7  cap:8  ptr:0xc0000b6000
[0 1 2 3 4 5 6 7]  len:8  cap:8  ptr:0xc0000b6000
[0 1 2 3 4 5 6 7 8]  len:9  cap:16  ptr:0xc0000b8000
[0 1 2 3 4 5 6 7 8 9]  len:10  cap:16  ptr:0xc0000b800
```

可以看出：

* `append()`函数将元素追加到切片的最后并返回该切片。
* 切片numSlice的容量按照1，2，4，8，16这样的规则自动进行扩容，每次扩容后都是扩容前的2倍。

### 作为函数参数的切片

示例1：

```
func main(){

	slice:=[]int{1,2,3}
	fmt.Printf("slice %v,slice address %p\n",slice,&slice)
	slice=changeSlice(slice)
	fmt.Printf("slice %v,slice address %p",slice,&slice)
}
func changeSlice(nums []int)[]int{
	nums[1]=111
	return nums

}
```

输出：

```
slice [1 2 3],slice address 0xc0420023e0
slice [1 111 3],slice address 0xc0420023e0
```

由示例1可以看出，外部切片中的值进行了改变，地址没有进行改变。

示例2：

```
func main(){

	slice:=[]int{1,2,3}
	fmt.Printf("slice %v,slice address %p\n",slice,&slice)
	slice=changeSlice(slice)
	fmt.Printf("slice %v,slice address %p\n",slice,&slice)
}
func changeSlice(nums []int)[]int{
	fmt.Printf("nums: %v,nums addr: %p\n",nums,&nums)
	nums[1]=111
	return nums

}
```

输出：

```
slice [1 2 3],slice address 0xc04204a3a0
nums: [1 2 3],nums addr: 0xc04204a400
slice [1 111 3],slice address 0xc04204a3a0
```

由示例2可以看出，我们在函数中打印了传入函数的切片地址，发现和外部切片地址并不一样。

当函数的参数是切片的时候，到底是传值还是传引用？从changeSlice函数中打出的参数s的地址，可以看出肯定不是传引用，毕竟引用都是一个地址才对。然而changeSlice函数内改变了s的值，也改变了原始变量slice的值，这个看起来像引用的现象，实际上正是我们前面讨论的**切片共享底层数组**的实现。

即切片传递的时候，传的是数组的值，等效于从原始切片中再切了一次。原始切片slice和参数s切片的底层数组是一样的。因此修改函数内的切片，也就修改了数组。

{{< admonition info >}}
当使用append()函数时，如果切片的容量不够，就会新生成一个底层数组，所以内存地址改变，并且改变新切片对原来的没有任何影响。
{{< /admonition >}}

