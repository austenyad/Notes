1. `sizeof` 函数是否使用括号取决于 运算的是 类型 还是 特定两，如果是类型必须加括号，比如：`sizeof(int)`，如果是特定量括号有可有无，比如：`sizeof 6.28`。

### #define + C 预处理器

在程序顶部添加：

`#define TAXTRATE 0.015` 

编译程序时，程序中所有使用 `TAXTRATE` 的地方都会被替换成 0.015。这一过程被称为 **编译时替换（compile-time substitution）**。

上面是定义常量，通用格式是：

`#define NAME value`

注意末尾不用加分号，因为这是一种由预处理器处理的替换机制。书写时用大写书写常量值。另外，还有一个不常用的命名约定，即在名称前带c_或k_前缀来表示常 量（如，`c_level`或`k_line`）。

### Const 限定符

C90 新增 const 关键字，用于限定一个变量只读。

`const int MONTHS = 12;` // MONTHS在程序中不可更改，值为12。

### 常用的常量

C 头文件 `limits.h` 和 `float.h` 分别提供了与 整数类型和浮点数类型大小限制的详细信息。每个头文件都定义了一系列供使用的常量，例如 `limits.h` 文件包含类似的代码：

`#define INT_MAX  2147483647`

`#define INT_MIN (-2147483647-1)`

这些明示常量代表int类型可表示的最大值和最小值。如果系统使用32 位的int，该头文件会为这些常量提供不同的值。

下面图片列出了一下常量：

![](https://note-austen-1256667106.cos.ap-beijing.myqcloud.com/2025-07-03-113617.jpg)

类似的，`float.h` 头文件中也定义一些明示常量，如`FLT_DIG`和 `DBL_DIG`，分别表示`float`类型和`double`类型的有效数字位数。

下面图片中列出了一些常量：

![image-20250703194141926](https://note-austen-1256667106.cos.ap-beijing.myqcloud.com/2025-07-03-114146.png)

表中所列都与`float`类型相关。把明示常量名中的`FLT`分别替换成 `DBL`和`LDBL`，即可分别表示`double`和`long double`类型对应的明示常量（表中 假设系统使用2的幂来表示浮点数）。