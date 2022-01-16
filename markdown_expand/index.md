# Markdown常用扩展语法


<!--more-->

## LaTeX数学公式

公式一律使用另取一行，并且上下都空一行。

### 行内公式排版

$ c = \sqrt{a^{2}+b_{0}^{2}+e^{x}} $

```markdown
$ c = \sqrt{a^{2}+b_{0}^{2}+e^{x}} $
```

### 块公式排版

$$
\begin{aligned}
y = y(x,t) &= A e^{i\theta} \\\\
    &= A (\cos \theta + i \sin \theta) \\\\
    &= A (\cos(kx - \omega t) + i \sin(kx - \omega t)) \\\\
    &= A\cos(kx - \omega t) + i A\sin(kx - \omega t)  \\\\
    &= A\cos \Big(\frac{2\pi}{\lambda}x - \frac{2\pi v}{\lambda} t \Big) + i A\sin \Big(\frac{2\pi}{\lambda}x - \frac{2\pi v}{\lambda} t \Big)  \\\\
    &= A\cos \frac{2\pi}{\lambda} (x - v t) + i A\sin \frac{2\pi}{\lambda} (x - v t)
\end{aligned}
$$

```markdown
$$
\begin{aligned}
y = y(x,t) &= A e^{i\theta} \\\\
    &= A (\cos \theta + i \sin \theta) \\\\
    &= A (\cos(kx - \omega t) + i \sin(kx - \omega t)) \\\\
    &= A\cos(kx - \omega t) + i A\sin(kx - \omega t)  \\\\
    &= A\cos \Big(\frac{2\pi}{\lambda}x - \frac{2\pi v}{\lambda} t \Big) + i A\sin \Big(\frac{2\pi}{\lambda}x - \frac{2\pi v}{\lambda} t \Big)  \\\\
    &= A\cos \frac{2\pi}{\lambda} (x - v t) + i A\sin \frac{2\pi}{\lambda} (x - v t)
\end{aligned}
$$
```

### 常用转义

* `\boxed`或者`\fbox`：给公式加一个方框。
* `\mathbf`：将字体加粗。
* `\boldsymbol`：将字体斜体且加粗。
* `\dfrac`：把字号设置为独立公式中的大小。
* `\tfrac`：把字号设置为行间公式中的大小。

组合数用法与分数类似，在命令前加d和t也能达到分数字号设置同样的功能。

### 格式

* **上下标**

`x`元素的上标通过`^`符号后接的内容体现，下表通过`_`符号后接的内容体现，多于一位是要加`{}`包裹的。如，$x_{m}^{n}$。

语法

`$x_{m}^{n}$`

* **分式**

$\frac{1}{2}$

语法

`$\frac{分子}{分母}$`

* **根式**

`[]`中代表是几次根式，`{}`代表根号下的表达式。如，$\sqrt[3]{2}$

`$\sqrt[3]{2}$`

* **求和积分**

求和函数表达式
$\sum_{k=1}^{n}\frac{1}{k}$

语法

`$\sum_{k=1}^{n}\frac{1}{k}$`

积分函数表达式
$\int_a^bf(x)dx$

语法

`$\int_a^bf(x)dx$`

* **空格**

1. 紧贴$a\!b$

语法

`$a\!b$`

2. 没有空格$ab$

语法

`$ab$`

3. 小空格$a\,b$

语法

`$a\,b$`

4. 中等空格$a\;b$

语法

`$a\;b$`

5. 大空格$a\ b$

语法

`$a\ b$`

6. quad空格$a\quad b$

语法

`$a\quad b$`

7. 两个quad空格$a\qquad b$

语法

`$a\qquad b$`

* **公式界定符**

通过`\left`和`\right`后面跟界定符来对同时进行界定。

$\left(\sum_{k=\frac{1}{2}}^{N^2}\frac{1}{k}\right)$

语法

`$\left(\sum_{k=\frac{1}{2}}^{N^2}\frac{1}{k}\right)$`

* **多行**

`\\\\`用来换行，`&=`用来对齐等号,`\begin{aligned}...\end{aligned}`用来写多行。

$\begin{aligned}
x = a + b \\\\ y = c + d \\\\ z = e + f
\end{aligned}$

语法
```markdown
$\begin{aligned}
x = a + b \\\\ y = c + d \\\\ z = e + f
\end{aligned}$
```

## 流程图

{{< mermaid >}}
graph LR;
    A[Hard edge] -->|Link text| B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
graph LR;
    A[Hard edge] -->|Link text| B(Round edge)
    B --> C{Decision}
    C -->|One| D[Result one]
    C -->|Two| E[Result two]
{{</* /mermaid */>}}
```

## UML时序图

{{< mermaid >}}
sequenceDiagram
    participant Alice
    participant Bob
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br/>prevail...
    John-->Alice: Great!
    John->Bob: How about you?
    Bob-->John: Jolly good!
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
sequenceDiagram
    participant Alice
    participant Bob
    Alice->>John: Hello John, how are you?
    loop Healthcheck
        John->John: Fight against hypochondria
    end
    Note right of John: Rational thoughts <br/>prevail...
    John-->Alice: Great!
    John->Bob: How about you?
    Bob-->John: Jolly good!
{{</* /mermaid */>}}
```

## 甘特图

{{< mermaid >}}
gantt
    dateFormat  YYYY-MM-DD
    title Adding GANTT diagram functionality to mermaid
    section A section
    Completed task            :done,    des1, 2014-01-06,2014-01-08
    Active task               :active,  des2, 2014-01-09, 3d
    Future task               :         des3, after des2, 5d
    Future task2               :         des4, after des3, 5d
    section Critical tasks
    Completed task in the critical line :crit, done, 2014-01-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :1d
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
gantt
    dateFormat  YYYY-MM-DD
    title Adding GANTT diagram functionality to mermaid
    section A section
    Completed task            :done,    des1, 2014-01-06,2014-01-08
    Active task               :active,  des2, 2014-01-09, 3d
    Future task               :         des3, after des2, 5d
    Future task2               :         des4, after des3, 5d
    section Critical tasks
    Completed task in the critical line :crit, done, 2014-01-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :1d
{{</* /mermaid */>}}
```

## 类图

{{< mermaid >}}
classDiagram
    Class01 <|-- AveryLongClass : Cool
    Class03 *-- Class04
    Class05 o-- Class06
    Class07 .. Class08
    Class09 --> C2 : Where am i?
    Class09 --* C3
    Class09 --|> Class07
    Class07 : equals()
    Class07 : Object[] elementData
    Class01 : size()
    Class01 : int chimp
    Class01 : int gorilla
    Class08 <--> C2: Cool label
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
classDiagram
    Class01 <|-- AveryLongClass : Cool
    Class03 *-- Class04
    Class05 o-- Class06
    Class07 .. Class08
    Class09 --> C2 : Where am i?
    Class09 --* C3
    Class09 --|> Class07
    Class07 : equals()
    Class07 : Object[] elementData
    Class01 : size()
    Class01 : int chimp
    Class01 : int gorilla
    Class08 <--> C2: Cool label
{{</* /mermaid */>}}
```

## 状态图

{{< mermaid >}}
stateDiagram
    [*] --> Still
    Still --> [*]
    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
stateDiagram
    [*] --> Still
    Still --> [*]
    Still --> Moving
    Moving --> Still
    Moving --> Crash
    Crash --> [*]
{{</* /mermaid */>}}
```

## Git 图

{{< mermaid >}}
gitGraph:
options
{
    "nodeSpacing": 100,
    "nodeRadius": 10
}
end
    commit
    branch newbranch
    checkout newbranch
    commit
    commit
    checkout master
    commit
    commit
    merge newbranch
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
gitGraph:
options
{
    "nodeSpacing": 100,
    "nodeRadius": 10
}
end
    commit
    branch newbranch
    checkout newbranch
    commit
    commit
    checkout master
    commit
    commit
    merge newbranch
{{</* /mermaid */>}}
```

## 饼图

{{< mermaid >}}
pie
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15
{{< /mermaid >}}

语法

```markdown
{{</* mermaid */>}}
pie
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15
{{</* /mermaid */>}}
```

## 特殊字符

常见的一些特殊字符。

| 特殊字符 | 描述 | 字符的语法 |
| :----: | :----: | :----: |
|  | 空格符 | `&nbsp;` |
| &lt; | 小于 | `&lt;` |
| &gt; | 大于 | `&gt;` |
| &amp; | 与 | `&amp;` |
| &yen; | 人民币 | `&yen;` |
| &copy; | 版权 | `&copy;` |
| &reg; | 注册商标 | `&reg;` |
| &deg;C | 摄氏度 | `&deg;C` |
| &plusmn; | 正负 | `&plusmn;` |
| &times; | 乘 | `&times;` |
| &divide; | 除 | `&divide;` |
| &sup2; | 平方 | `&sup2;` |
| &sup3; | 立方 | `&sup3;` |

