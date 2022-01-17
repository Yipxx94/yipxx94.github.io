# C++ 就地建堆的方法


<!--more-->


## 描述

`make_heap()`用于将序列转换为堆，被定义在**“algorithm”**头文件中。

`make_heap`的时间复杂度是**`O(n)`**，低于一个一个插进堆中的`O(nlogn)`。

标准容器适配器`priority_queue`自动调用`make_heap`、`push_heap`和`pop_heap`来维护容器的堆属性。

## 函数说明

`make_heap(iter_first, iter_last, comp)`：在容器范围内，就地建堆，保证最大值在所给范围的最前面，其他值的位置不确定。

`pop_heap(iter_first, iter_last, comp)`：将堆顶（所给范围的最前面）元素移动到所给范围的最后，并且将新的最大值置于所给范围的最前面。

`push_heap(iter_first, iter_last, comp)`：当已建堆的容器范围内有新的元素插入末尾后，应当调用push_heap将该元素插入堆中。

## 参数

* **first，last**：定义要修改的合法非空堆的元素范围。

* **comp**：比较函数对象（即满足比较 (Compare) 要求的对象），若首个参数小于第二个，则返回 true。

  比较函数如下：

  `bool cmp(const Type1 &a, const Type2 &b);`

{{< admonition >}}
函数的首个版本使用`operator<`比较元素，这使堆成为最大堆。第二版本使用给定的比较函数`comp`。 
{{< /admonition >}}

## 示例

```
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    //自定义cmp函数
    auto cmp = [](const int x, const int y){
    	return x > y;
    };
    vector<int> nums = {3, 1, 4, 1, 5, 9};
    //就地建堆
    make_heap(nums.begin(), nums.end(), cmp);
    cout << nums.front() << endl;
	//移动堆顶
    pop_heap(nums.begin(), nums.end(), cmp);
    nums.pop_back();	//删除堆顶元素
    cout << nums.front() << endl;
	//插入新元素
    nums.push_back(0);
    push_heap(nums.begin(), nums.end(), cmp);
    cout << nums.front() << endl;

    return 0;
}
```

输出：

```
1
3
0
```



