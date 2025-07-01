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

#### 复数和虚数浮点数







