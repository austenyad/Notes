# C primer plus 笔记

 

## C 语言类型

### 字符串

在C语言中，可以把多个连续的字符串组合成一个字符串，比如：`printf("me16 = %" "d" "\n", me16);` 等价于 `printf("me16 = %d\n", me16);`

### 浮点数

`float` 、`double` 、`long double` 



`long double` C 语言保证精度至少与 `double` 的精度相同。

#### 常量书写规则

浮点数的书写形式：有符号的数字（包括小数点），后面紧跟e或E，最后是一个有符号数 表示10的指数。不要中间加空格。

`-10.88e-10`

`-1.56E+12`

`2.87e-3`

正号可以省略。

可以没有小数点，如 `2e5`，或者指数部分 ，如 `19.28`，但是不能同时省略两者。

可以省略小数部分，如 `3.e16`，或者整数部分，如 `.45e-6`，但是不能同时省略两者。

#### 默认编译器处理方式

默认情况下，编译器假定浮点型常量是double类型的精度。例如

```c
float some = 4.0 * 2.0
```

通常情况下 4.0 和 2.0 被存储为 64 位的 double 类型，使用双精度进行乘法运 算，然后将乘积截断成float类型的宽度。这样做虽然计算精度更高，但是会 减慢程序的运行速度。

在浮点数后面加上 f 或 F 可以强制告诉编译器为 `float` 类型，如 `2.3f` 或者 `2.3F`。

在浮点数后面加上 l 获取 L 告诉编译器为 `long double` 类型，如 `2.3l` 或者 `2.3L`。

没有后缀默认为 double 类型。

建议使用大写字母作为后缀，不容易被混淆。

C99 标准添加了一种新的浮点型常量格式：用十六进制表示浮点型常量，即在十六进制数前加上十六进制前缀（0x或0X），用p和P分别代替e和 E，用2的幂代替10的幂（即，p计数法）。如下所示：

 `0xa.1fp10` 

十六进制a等于十进制10，.1f是1/16加上15/256（十六进制f等于十进制 15），p10是2 10或1024。0xa.1fp10表示的值是`(10 + 1/16 + 15/256)×1024`（即，十进制10364.0）

#### 打印浮点数

使用 `%f` 打印十进制记法的 `float` 和 `double` 类型的浮点数；

使用 `%e` 打印指数记法的浮点数；

使用 `%a` 或 `%A` 打印 十六进制浮点数（提前是系统支持十六进制浮点数）

对于 `long double` 类型 使用 `%Lf`、`%Le` 或者 `%La`。

函数原型未显示声明参数类型的函数传递参数时，C 编译器会把 `float`类型自动转换为 `double`类型。

```c
int main(void) {
    //  0x1.f4p+14 = (1 + 15/16 + 4/256) * 2^14
    float aboat = 32000.0;
    double abet = 2.14e9;
    long double dip = 5.32e-5;
    printf("%f can be written %e\n",aboat,aboat);

    //C99
    printf("And it's  %a in hexadecimal ,powers of 2 notation\n",aboat);
    printf("%f can be written %e\n",abet,abet);
    printf("%Lf can be written %Le\n",dip);
    return 0;
}
// 打印
And it's  0x1.f4p+14 in hexadecimal ,powers of 2 notation
2140000000.000000 can be written 2.140000e+09
0.000053 can be written 2.140000e+09
```

#### 浮点数的上溢和下溢

**上溢**： 例子如下：

```c
float toobig = 3.4e38 * 100.0f;
printf("%e\n",toobig);
```

上面计算导致数字过大，超出当前类能表达的范围时，就发生了 **上溢**。现代 C 语言规定使用 `inf` 或 `infinity` 显示该值。

**下溢**：？？？

C语言把损失了类型全精度的浮点值称为低于正常的 （subnormal）浮点值。

**浮点值 `NaN`**（not a number的缩写）。比如，使用反正选函数，传递一个大于 1 的值，就会返回一个 `NaN` 或 `nan`。

#### 浮点数舍入错误

给定一个数，加上1，再减去原来给定的数，结果是多少？你一定认为 是1。但是，下面的浮点运算给出了不同的答案：

```c
int main(void) {
    float a = 2.0e20 + 1.0;
    float b = a - 2.0e20;
    printf("%f \n",b);
    return 0;
}
```

该程序的输出如下：![image-20250630233812043](https://p.ipic.vip/hmrb05.png)

得出这些奇怪答案的原因是，计算机缺少足够的小数位来完成正确的运 算。2.0e20是 2后面有20个0。如果把该数加1，那么发生变化的是第21位。 要正确运算，程序至少要储存21位数字。而float类型的数字通常只能储存按 指数比例缩小或放大的6或7位有效数字。在这种情况下，计算结果一定是错 误的。另一方面，如果把2.0e20改成2.0e4，计算结果就没问题。因为2.0e4 加1只需改变第5位上的数字，float类型的精度足够进行这样的计算。

### 复数和虚数

C语言有3种复数类型：`float _Complex`、`double _Complex`和 `long double _Complex`。例如，`float _Complex`类型的变量应包含两个 float 类型的值，分别表示复数的实部和虚部。

C语言的3种虚数类型是`float _Imaginary`、`double _Imaginary`和`long double _Imaginary`。

如果引入了 `complex.h` 头文件，使用 `complex` 代替 `_Complex`，用`imaginary` 代替`_Imaginary`，还可以用`I`代替-1的平方根。



## 小结

#### 关键字

基本数据类型由11个关键字组成

`int`、`long`、`short`、`unsigned`、`char`、 `float`、`double`、`signed`、`_Bool`、`_Complex`和`_Imaginary`。

#### 有符号整形

有符号整型可用于表示正整数和负整数。

* `int` 系统给定的基本整数类型。C语言规定`int`类型不小于16位。
* `short`或`short int` ——`最大的short类型整数` <= `最大的int类型整数`。C语言规定short类型至少占16位。
* `long`或`long int` ——`该类型可表示的整数 >= `最大的int类型整数`。 C语言规定long类型至少占32位。
* `long long`或`long long int` ——`该类型可表示的整数 >= `最大的long 类型整数`。Long long类型至少占64位。

一般而言，long类型占用的内存比short类型大，int类型的宽度要么和 long类型相同，要么和short类型相同。

#### 无符号整形

无符号整型**只能用于表示零和正整数**，因此无符号整型可表示的正整数比有符号整型的大。在整型类型前加上关键字`unsigned`表明该类型是无符号整型：

`unsigned int`、`unsigned long`、`unsigned short`。单独的`unsigned`相当于 `unsigned int`。

#### 字符类型

`char` —— 字符类型的关键字。有些编译器使用**有符号的char**，而有些则使用**无符号的char**。在需要时，可在char前面加上关键字 `signed` 或 `unsigned` 来指明具体使用哪一种类型。

`char` 类型要占用 1个字节内存，即 8 位bit ，但是如果要 表示基本字符集，也可以是16位或更大。

#### 布尔类型

`_Bool` 布尔值表示 true 和 false。C语言用1表示true，0表示false。布尔类型是无符号 `int`类型，所占用的空间只要能储存 0 或 1 即可。

#### 实浮点类型

实浮点类型可表示正浮点数和负浮点数。

`float` ——系统的基本浮点类型，可精确表示至少6位有效数字。

`double` ——储存浮点数的范围（可能）更大，能表示比 float 类型更多 的有效数字（至少 10位，通常会更多）和更大的指数。

`long double` ——储存浮点数的范围（可能）比double更大，能表示比 double更多的有效数字和更大的指数。

#### 复数和虚数浮点数？？？



#### 声明类型使用变量

**C 语言在类型匹配方面不太严格**。C编译器甚至允许二次初始化，但在激活了较高级别警告时，会给出警告。

把一个特定类型的数值 初始化给 不同类型的变量时，编译器会把值转换成与变量匹配的类型，这将导致部分数据丢失。例如：

`int const = 12.99;` // 使用 double 类型的数值初始化 int 类型的变量

`float pi = 3.1415926536;` // 使用 double 类型的值初始 float 类型的变量 

第1个声明，cost的值是12。C编译器把浮点数转换成整数时，会直接丢 弃（截断）小数部分，而不进行四舍五入。第2个声明会损失一些精度，因 为C只保证了float类型前6位的精度。

#### 从变量命名中体现变量类型

有些公司或程序员都有系统化的命名规范，在变量名中可以体现该变量的类型，例如：`i_` 前缀表示 int 类型，`us_` 前缀表示 `unsigned short` 类型。这样一眼就能看出 `i_smart` 是 int 类型的变量，`us_versmart` 是 `unsigend short` 类型的变量。





# 四. 输入/输出

### 字符串

1. 字符串和字符的区别是 字符串的末尾一定是空字符：`\0`。
2. `scanf("%s",name)`  输入函数在读取输入的字符时，只会读取一个单词，比如输入 `austen Yang` ，输入函数只会读取 `austen` 存储到 `name` 变量中。
3. `strlen()` 函数计算出字符串的长度，虽然字符串一定以空字符 `\0` 结尾，但是这个函数不会计入空字符，对一个字符串 `sizeof` ，会计入空字符的个数。
4. C99 和 C11 标准专门为 `sizeof` 函数的返回值类型添加 `%zd` 的格式符，这个对于 `strlen()` 函数同于适用。对于早期的C，还要知道sizeof和 strlen()返回的实际类型（通常是`unsigned`或`unsigned long`）。所以有的 IDEA 编辑时，提示使用 `%lu` 进行 格式化。
5. `sizeof` 函数是否使用括号取决于 运算的是 类型 还是 特定两，如果是类型必须加括号，比如：`sizeof(int)`，如果是特定量括号有可有无，比如：`sizeof 6.28`。

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

### Printf 函数

列出常用的格式化符号

![image-20250703221106173](https://p.ipic.vip/fbi1dv.png)

# 七 C 控制语句：分支和跳转

1. `getchar()` ：函数不带任何参数，它从输入队列中返回下一个字符。`putchar(char)` ：函数打印字符。例子如下：

```c
int main(void) {
    char ch;
    while ((ch = getchar()) != '\n') {
        if (ch == SPACE) {
            putchar(ch);
        } else {
            putchar(ch + 1);
        }
    }
    putchar(ch);
    return 0;
}
```

2. `ctype.h`头文件中的函数，接受一个字符为参数，如果该字符属于某特殊的类别，就返回一个非零值（真）；否则，返回0（假）。例如 `isalpha()` 函数 输入要给参数，判断参数是不是字母。

```c
int main(void) {
    char ch;
    while ((ch = getchar()) != '\n') {
        if (isalpha(ch)) {
            putchar(ch + 1);
        } else {
            putchar(ch);
        }
    }
    putchar(ch);
    return 0;
}
```

下面表列出 `ctype.h` 字符测试函数

![](https://p.ipic.vip/3d47h0.png)

下面表列出 `ctype.h` 字符映射函数

![](https://p.ipic.vip/bnu2si.png)

# 八、字符输入/输出和输入验证

1. C可以使用主机操作系统的基本文件工具直接处 理文件，这些直接调用操作系统的函数被称为底层 I/O （low-level I/O）。
2. 由于计算机系统各不相同，所以不可能为普通的底层I/O函数创建标准库，ANSI C也不打算这样做。然而从较高层面上，C还可以通过标准I/O包 （standard I/O package）来处理文件。这涉及创建用于处理文件的标准模型 和一套标准I/O函数。在这一层面上，具体的C实现负责处理不同系统的差 异，以便用户使用统一的界面。
3. 在C语言中，用 getchar()读取文件检测到文件结尾时将返回一个特殊的值，即EOF（end of file的缩写）

```c
int main(void) {
    char ch;
    while ((ch = getchar()) != EOF) putchar(ch);
    return 0;
}
// example3.c
```

4. 输入输出重定向 

重定向运算符连接一个可执行程序（包括标准操作系统命令）和一个数 据文件，不能用于连接一个数据文件和另一个数据文件，也不能用于连接一 个程序和另一个程序

在 Mac 电脑中

```c
example3.c < a文件 > b 文件 (读取A文件输入到B文件)
// 或者
example3.c > b文件 > a 文件 （读取A文件输入到B文件）
```

上面程序会把 a 文件中的内容输出到 b 文件中。上面两个是一样的。

使用重定向运算符不能读取多个文件的输入，也不能把输出定向至多个 文件。

5. `>>` 运算符 可以把数据添加到文件末尾。
6. `|`运算符可以把一个文件的输出连接到另一个文件的输入。
7. 代码片段

```c
// 从控制台输入读取文件名的名字，并且输入文件内容到控制台。
int main(void) {
    int ch;
    FILE *fp;
    char filename[50]; // 存储文件名
    printf("Enter the name of the file: ");
    scanf("%s", filename);
    fp = fopen(filename, "r"); // 打开待读取文件
    if (fp == NULL) {
        printf("Failed to open file. Bye\n");
        exit(1);
    }
    // getc(fp) 从打开的文件中获取一个字符
    while ((ch = getc(fp)) != EOF) {
        putchar(ch);
    }
    fclose(fp); // 关闭文件
    return 0;
}
```

# 第九章 函数

1. 函数声明与调用：因为 C 语言 ANSI C 标准之前，声明函数只需要声明函数的名字，返回值，不需要声明函数才参数。所以我们在现在的编译器下，会出现：

```c
int imin();


int main(void){
	imin(1,3,3,3);
}
```

上面的代码编译时是不会报错的。

```c
int imax(); /* 旧式函数声明 */
int main(void) {
    printf("The maximum of %d and %d is %d.\n",3,5,imax(3));
    printf("The maximum of %d and %d is %d.\n",3,5,imax(3.0,5.0));
    return 0;
}

int imax(n, m)
int n, m; {
    return (n > m ? n : m);
}

```

上面的代码是可以运行的。

2. 参数不规定的函数声明，比如 `printf`,使用 `...` 来代表未知类型且多个参数。

```
int printf(const char *,...)
```

3. C 语言可以定义函数原型，也可以不定义，不定义时，就必须声明在被调用函数的前面，并且还有函数体。这种函数的使用，只适用于较小的函数。

```c
int maximum(int a,int b){
	return a > b ? a : b;
}

int main(void){
	maximum(3,10);
	return 0;
}
```

4. 递归




### 9.4 编译多文件代码

1. 假设file1.c和file2.c是两个内含C函数的文件，下面的命令将编译 两个文件并生成一个名为a.out的可执行文件：

`gcc file1.c file2.c`

另外，还生成两个名为file1.o和file2.o的目标文件。

如果后来改动了 file1.c，而file2.c不变，可以使用以下命令编译第1个文件，并与第2个文件 的目标代码合并：

`gcc file1.c file2.o`

2. 把**函数原型**和**已定义的字符常量**放在头文件中是一个良好的编程习惯。



例子：

hotel.h

```c
#define QUIT 5
#define HOTEL1 180.00
#define HOTEL2 225.00
#define HOTEL3 225.00
#define HOTEL4 355.00

#define DISCOUNT 0.95

#define STARS "**********************************"


// 显示选择列表

int menu(void);


// 返回预订天数

int getnights(void);


// 根据费率、入住天数计算费用
// 并显示结果

void showprice(double rate, int nights);
```

usehotel.c 

```c
#include <stdio.h>

#include "hotel.h"

int main(void)
{

    int nights;
    double hotel_rate;
    int code;

    while ((code = menu()) != QUIT)
        {
            switch (code)
            {
            case 1:
                hotel_rate = HOTEL1;
                break;
            case 2:
                hotel_rate = HOTEL2;
                break;
            case 3:
                hotel_rate = HOTEL3;
                break;
            case 4:
                hotel_rate = HOTEL4;
                break;

            default:
                hotel_rate = 0.0;
                printf("Oops\n");
                break;
            }

            nights = getnights();

            showprice(hotel_rate, nights);
        }

    printf("Thank you and goodbye!/n");


    return 0;


}
```

hotel.c

```c
#include <stdio.h>

#include "hotel.h"

int menu(void)
{
    int code, status;
    printf("\n%s%s\n", STARS, STARS);
    printf("Enter the number of the desired hotel :\n");
    printf("1) Fairfield Arms           2) Hotel Olympics\n");
    printf("3) Chertworthy Plaza        4) The Stockton\n");
    printf("5) Quit\n");
    printf("%s%s\n", STARS, STARS);

    while ((status = scanf("%d", &code)) != 1 || (code < 1 || code > 5))
    {
        if (status != 1)
            scanf("%*s"); // 处理非整数输入
        printf("Enter an integer from 1 to 5, please\n");
    }
    return code;
}

int getnights(void)
{
    int nights;
    printf("How many nights are needed? ");
    while (scanf("%d", &nights) != 1)
    {
        scanf("%*s"); // 处理非整数输入
        printf("Please enter an integer, such as 2.\n");
    }
    return nights;
}

void showprice(double rate, int nights)
{
    int n;
    double total = 0.0;
    double factor = 1.0;
    for (n = 1; n <= nights; n++, factor *= DISCOUNT)
        total += rate * factor;
    printf("The total cost will be $%0.2f.\n", total);
}
```

使用 gcc 编译：

`gcc usehotel.c hotel.c` 

运行后，直接生成 a.out 可执行文件。

### 9.5 &运算符

1. 一元 & 运算符，给出变量的存储地址。 如果 `pooh`是变量名，那么 `&pooh` 是变量的地址。地址就是变量在内存中的位置。假设有下面的语句：

`pooh = 24;`

假设 `pooh` 的存储地址是 `0B76`（通常使用 十六进制 表示）。那么下面的语句：

`printf("%d %p\n",pooh,&pooh);`

将输出下面内容：`24 0x16fafea98`

2. 函数调用值传递

```c
void mikado(int);

int main(void) {
    int pooh = 2, bah = 5;
    printf("In main(), pooh = %d and &pooh = %p\n", pooh, &pooh);
    printf("In main(), bah = %d and &bah = %p\n", bah, &bah);
    mikado(pooh);
    return 0;
}


void mikado(int bah) {
    int pooh = 10;
    printf("In mikado(), pooh = %d and &pooh = %p\n", pooh, &pooh);
    printf("In mikado(), bah = %d and &bah = %p\n", bah, &bah);
}
```

输出：

```c
In main(), pooh = 2 and &pooh = 0x16b316a98
In main(), bah = 5 and &bah = 0x16b316a94
In mikado(), pooh = 10 and &pooh = 0x16b316a58
In mikado(), bah = 2 and &bah = 0x16b316a5c
```

main 函数调用 mikado 函数 传递的变量 `pooh`，是值传递。

### 9.7 指针简介

从根本上看，**指针（pointer）**是一个值为内存地址 的变量（或数据对象）。

比如一个指针变量是 `ptr`，可以编写如下语句：

`ptr = &pooh;`  // 把 pooh 的地址赋给 ptr。

对于上面语句，我们说 `ptr` 指向 `pooh` 。

`ptr` 和 `&pooh` 的区别是 `ptr` 是变量，而 `&pooh` 是常量。

或者说，`ptr` 是可以修改的左值，而 `&pooh` 是右值。

与指针相关的运算符：

地址运算符：`&`

后跟一个变量名时，`&` 给出变量的地址。`&nurse` 表示变量 `nurse` 的地址变量。

地址运算符：`*`

后跟一个指针名或地址时，`*` 给出存储在指针指向地址上的值。

```c
nurse = 22;
ptr = &nurse; // 指向 nurse 的指针
val = *ptr; // 把 ptr 指向的地址上的值赋值给 val
```

### 9.7.2 声明指针

`int *pi;` // pi 是指向 int 类型变量的指针

`char *pc;` // pc 是指向 char 类型变量的指针

`float *pf,*pg;`// pf、pg 都是指向 float 类型的指针

类型说明了 指针所指向对象的类型，星号 表明声明的变量是一个指针。`int *pi;`  声明的意思是 pi 是一个指针，*pi 是 int 类型。

在大部分系统中，地址是由一个无符号整数表示，但是不要把指针认为是整数类型，一些处理整数的操作不能用来处理指针，反之亦然。所以，指针实际上是一个新类型，不是整数类型。因 此，如前所述，ANSI C专门为指针提供了`%p`格式的转换说明。

### 9.7.3 指针在函数间通信

指针在函数间通信的最好的例子就是，定义交换变量的函数。比如：

```c
void interchange(int*, int*);
int main(void) {
    int x = 5, y = 10;
    printf("Originally x = %d and y = %d.\n", x, y);
    interchange(&x, &y);
    printf("Now x = %d and y = %d.\n", x, y);
    return 0;
}

void interchange(int * x, int * y) {
    const int temp = *y;
    *y = *x;
    *x = temp;
}
```



小结：

* 如果需要在被调用函数中改变主调函数的变量，使用地址或者指针作为函数参数。
* 函数原型 是 C 的强大的工具，允许编译器验证函数调用中使用的参数个数和类型是否正确。

### 9.10 复习题

1. 实际参数和形式参数的区别是什么？

答：实际参数就是就是函数体中的变量，形参就是函数声明的参数中声明的变量，在函数调用过程中，形参只是对，实际参数的拷贝。

2. 根据下面各函数的描述，分别编写它们的ANSI C函数头。注意，只需 写出函数头，不用写函数体。

a. donut()接受一个int类型的参数，打印若干（参数指定数目）个0

`void donut(int);`

b. gear()接受两个int类型的参数，返回int类型的值

`int gear(int,int);`

c. guess()不接受参数，返回一个int类型的值

`int guess();`

d. stuff_it()接受一个double类型的值和double类型变量的地址，把第1个 值储存在指定位置

`void stuff_it(double,double*);`

3. 根据下面各函数的描述，分别编写它们的ANSI C函数头。注意，只需 写出函数头，不用写函数体。

a.n_to_char()接受一个int类型的参数，返回一个char类型的值

`char no_to_char(int);`

b.digit()接受一个double类型的参数和一个int类型的参数，返回一个int类 型的值

`int digit(double,int);`

c.which()接受两个可储存double类型变量的地址，返回一个double类型 的地址

`double* which(double*,double*);`

d.random()不接受参数，返回一个int类型的值

`int random(void);`

4. 设计一个函数，返回两整数之和。

```c
int sum(int a,int b){
  return a + b;
}

int sum(const int a, const int b){
    return a + b;
}
```

<font size=4, color='red'><b> 编译器为什么提示加 const ? const 的作用所什么？const 它有没有兼容性问题？</b></font>

5. 如果把复习题4改成返回两个double类型的值之和，应如何修改函数？

```c
double sum(double a,double b){
	return a + b;
}
```

6. 设计一个名为alter()的函数，接受两个int类型的变量x和y，把它们的值分别改成两个变量之和以及两变量之差。

答：

```c
void alter(int*,int*);
int main(void){
  int x = 10, y = 5;
	printf("x = %d , y = %d\n",x,y);
  alter(&x,&y);
  printf("计算后得到 x = %d , y = %d\n",x,y);
  return 0;
}

void alter(int* a,int* b){
  int tempA = *a;
  int tempB = *b;
  *a = tempA + tempB;
  *b = tempA - tempB;
}
```

7. 下面的函数定义是否正确？

```c
void salami(num)
{
int num, count;
for (count = 1; count <= num; num++)
printf(" O salami mio!\n");
}
```

答：

a. 函数参数声明时，没有什么参数 类型。

b. 声明的 num 变量，并没有初始化值，num 是 for 循环的判断条件，没有初始化值，可能导致在运行时系统给了一个随意的值，如果值 太大，导致循环无法退出，程序崩溃。

c. 声明的 num 实参变量 和 形参一直，导致无法运行。

8.编写一个函数，返回3个整数参数中的最大值。

```c

int maximum(int a,int b,int c){
  int maxValue = 0;
	if(a > b){
    if(a > c){
      maxValue = a;
    }else{
      maxValue = c;
    }
  }else{
    if(b > c){
      maxValue = b;
    }else{
      maxValue = c;
    }
  }
}
```

其他答案：

a. 

```c
int maximum(int a, int b, int c) {
    if (a >= b && a >= c) {
        return a;
    }
    if (b >= a && b >= c) {
        return b;
    }
    return c;
}
```

b.

```c
// 方法三：使用三元运算符 (?:)
int findMax3(int a, int b, int c) {
    int tempMax = (a > b) ? a : b; // 先比较 a 和 b
    return (tempMax > c) ? tempMax : c; // 再用 a 和 b 中的较大者与 c 比较
}
```




# 10 数组和指针

### 10.1 数组

数组由数据类型相同的一系列元素组成。如下：

```c
int main(void)
{
float candy[365]; /* 内含365个float类型元素的数组 */
char code[12]; /*内含12个char类型元素的数组*/
int states[50]; /*内含50个int类型元素的数组 */
...
}
```

方括号表明：candy、code 和 states 都是数组，方括号中的数组表明数组中的原素个数。通过数组下表（也称为索引）表明数组中的各个原素。数组元素的编号从 0 开始，所以 candy[0] 表示 candy 数组的第一个原素，candy[364] 表示第 356 个原素，也就是最后一个原素。

#### 10.1.1 初始化数组

1. 声明时初始化数组：`int powers[8] = {1,2,4,6,8,16,32,64};`

2. 使用 const 声明数组：`const int powers[8] = {1,2,4,6,8,16,32,64};`，数组只能读，不能把新值写入数组。

3. 初始化列表中的项 数量 和 数组大小不一致，即：`int somedata[4] = {1,2};`，编译器会把剩余的元素都初始化为 0 ，也就是说：如果不初始化数组，数组中的值和未初始化变量一样，其中存储的值都是垃圾值，但是如果初始化数组，剩余的元素会被初始化为 0。
4. 如果初始化列表项个数大于数组声明的大小，编译器会报错。
5. 4 这种情况，其实可以不声明数组大小来初始化数组 ：`int somedata[] = {1,2,3};`这时编译器会自动设置 数组大小，也就不会因为 数组个数大于初始化列表项个数报错了。
6. C99 标准 新增了一个特性：指定初始化器（designated initializer），利用该特性可以初始化指定的数组元素。

```c
int aar[6] = {[5] = 121};// 指定 aa[5] 为 121，没有指定的其他都是 0。
int aar2[10] = {1,2,[3]=100,4,[7]=200,8};
```

```c
int main(void) {
    int aar2[10] = {1,2,[3]=100,4,[5]=200,8,[5]=300,1,1,1};
    for (int i = 0; i < 10; i++) {
        printf("arr[%d] = %d\n", i, aar2[i]);
    }
    return 0;
}
// printf 
arr[0] = 1
arr[1] = 2
arr[2] = 0
arr[3] = 100
arr[4] = 4
arr[5] = 300
arr[6] = 1
arr[7] = 1
arr[8] = 1
arr[9] = 0
```

从上面得出指定初始化器 ： a. 如果指定初始化器后面有更多值，如上面离职中：[3]=100,4，那么后面的这些值将被用于初始指定元素后面的原生；b. 如果再次初始化指定的元素，那么后面的初始化将会取代之前的初始化。

更多的初始化器的例子

例子1：

```c
 int stuff[] = {1, [6] = 23};
 for (int i = 0; i < sizeof stuff / sizeof(int)  ; i++) {
     printf("stuff[%d] = %d\n", i, stuff[i]);
 }
    
//printf
stuff[0] = 1
stuff[1] = 0
stuff[2] = 0
stuff[3] = 0
stuff[4] = 0
stuff[5] = 0
stuff[6] = 23
```

例子2：

```c
  int staff[] = {1, [6] = 4, 9, 10};

  for (int i = 0; i < sizeof staff / sizeof(int)  ; i++) {
        printf("stuff[%d] = %d\n", i, staff[i]);
  }
//printf
stuff[0] = 1
stuff[1] = 0
stuff[2] = 0
stuff[3] = 0
stuff[4] = 0
stuff[5] = 0
stuff[6] = 4
stuff[7] = 9
stuff[8] = 10
```

