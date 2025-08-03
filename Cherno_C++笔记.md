## 循环

循环

1. for
2. while
3. do ... while

控制循环

1.  continue
2. break
3. return

## 指针

指针就是一个整数，一种存储内存地址的数字。指针本身也是变量，这些变量也存储在内存中，这也就是我们可以得到 双指针或三指针，意思是指针的指针。

```c++
   char* buffer = new char[8];
   memset(buffer, 0, 8);
   
   char** ptr = &buffer;
   delete[] buffer;
```



## 引用

引用本质上只是指针的伪装，是在指针上的语法糖，让它更容易理解和阅读。

```c++
 int a = 5;
 int &ref = a;
 ref = 2;
 LOG(a);

// 2
```

看起来就是给 a 变量起来个**别名**，但引用不是类似于指针的东西，编译器不需要实际创建一个新的变量，编译后的代码实际就是 `a = 2`。引用的操作就只存在于编写源代码中，只是为了让生活边的更简单。

下面是**使用指针实现调用函数使变量自增** 和 **使用引用实现调用函数使变量自增** 的比较。

```c++
void Increment(int* value) {
    (*value)++; // ++ 优先级 比 地址取值运算（即*）高，所有这里使用()来提升优先级。
}

int main() {
    int a = 5;
    Increment(&a);
    LOG(a);
}
// 6
```

```c++
void Increment(int& value) {
    value++;
}

int main() {
    int a = 5;
    Increment(a);
    LOG(a);
}
// 6
```

可以看出，实际两者的结果一模一样，只是使用 引用 的方式看起来更简单，但 最重要的是：**引用方式的写法编译后和指针方式是一模一样的**，**:smile:** 只是为了生活更简单。



引用和指针的区别

* 引用在声明时就需要初始化，不初始化编译器就会报错。

<img src="https://p.ipic.vip/cqbd06.png" style="zoom:50%;" />

* 声明了引用之后，不能改变它引用的东西，像下面代码给 ref 赋值后只是改变 a 的值，并不会将 b 的引用赋值给 ref 。

```c++
    int a = 5;
    int b = 8;
    int &ref = a; // 声明 a 变量的引用
    ref = b; // 重点：将 a 的引用 ref 赋值 b,不是将 b 的引用赋值给 ref。这里只是将 b 的值赋值给 a，所以 a 现在的值变成了 8
    a++;
    LOG(a)
    // 9 : 对 a进行++，所以是 9
```

说以 ref 它的引用是不能改变的，一点声明好之后，如果要实现引用的改变就只能使用指针实现类似的效果，代码如下：

```c++
 		int a = 5;
    int b = 8;
    int* ref = &a; // 将 a 变量的引用赋值指针变量 ref
    *ref = 2;  // 改变地址指向位置的变量中的值 为 2，即 a = 2
    ref = &b; // 将 b 变量的引用赋值给指针变量 ref
    *ref = 1; // 改变地址指向位置的变量中的值 为 1，即 b = 1
    LOG(a)
    LOG(b)
    // 2
    // 1
```

## 类

### 1. 类的基本概念

类是对数据（data）和功能（methods）组合起来的一种方法。  

类中的数据默认是私有的，只要类本身中的方法可以访问。

code_1 

```c++
class Player {
public:
    int x = 0, y = 0;
    int speed = 1;

    void Move(const int xa, const int ya) {
        x = xa * speed;
        y = ya * speed;
    }
};

int main() {
    Player player;
    LOG(player.x)
    player.Move(1, -1);
    LOG(player.x)
}

```

**类不会给你任何新的功能**，你可以用类搞定的事情，不用类一样也能搞定，类只是数据和功能的组合方法，在某些情况下可以**让代码更简洁、具有封装性，可以面向对象的思维编程，代码更容易维护**，仅此而已。这也就的像 C 语言这样的语言存在且完全可用的原因，即使没有面向对象编程。



## 结构体

谈到结构体，先要思考 结构体和类的区别，什么情况下使用 结构体 ？什么情况下使用 类？

答案是 基本上没有什么区别！只要关于可见度的小区别。

在类中我们说过，不写任何 可见性的修饰，类中的成员都是默认私有的，但在 结构体中 成员默认都是 public 的，**从技术层面讲，这是 类 与 结构体的唯一区别**。在类的章节中演示的示例代码 code_1 中，先删除 public 可见性修饰符，然后将 class 改为 struct。代码示例如下：

code_1

```c++

struct Player {

    int x = 0, y = 0;
    int speed = 1;

    void Move(const int xa, const int ya) {
        x = xa * speed;
        y = ya * speed;
    }
};

int main() {
    Player player;
    LOG(player.x)
    player.Move(1, -1);
    LOG(player.x)
}

```

上面代码就改了这些，在编译器下完全没问题，这就是 结构体 和 类 的区别，当然如果你想让 结构体中 这些成员边为私有加上可以修饰符 private 就可以，代码如下：

code_2

```c++
struct Player {
private:
    int x = 0, y = 0;
    int speed = 1;

    void Move(const int xa, const int ya) {
        x = xa * speed;
        y = ya * speed;
    }
};

int main() {
    Player player;
    LOG(player.x) // 编译器报错 ：无法访问 x
    player.Move(1, -1); // 编译器报错 ：无法访问 Move
    LOG(player.x) // 编译器报错 ：无法访问 x
}

```

从技术层面它们没有太大区别，但实际在编程使用时是有所不同的。

结构体在 C++ 中继续存在的唯一原因是它希望与 C 保持向后兼容性，C 中没有类但又结构体，在 C++ 取代结构体会失去对于 C 的向后兼容性，如果 C++ 中真的有没有结构体，编译不知道 struct 的存在，我们可以定义一个 `#define struct class` 这样的宏，来解决语法上的问题，但是要手动将可见性改为 public，才能和结构体的行为保持一致。:smiling_imp: 所以 C++ 还是选择保留了 struct ，保证与 C 的向后兼容性。

```c++
#define struct class
struct Player {
public:
    int x = 0, y = 0;
    int speed = 1;

    void Move(const int xa, const int ya) {
        x = xa * speed;
        y = ya * speed;
    }
};

int main() {
    Player player;
    LOG(player.x)
    player.Move(1, -1);
    LOG(player.x)
}
```

那么在什么情况下使用 class ，在什么情况下使用 struct ? 

观点1：

在对于一些只是的 数据 、再加上对数据的操作使用 struct 。

```c++
struct Vec2 {
    float x = 0, y = 0;

    void Add(const Vec2& other) {
        x = other.x;
        y = other.y;
    }
};
```

对于要实现大量功能，可能还包括 继承 使用 class 。



## 静态变量/函数

C / C++ 中什么静态变量分为两种情况来讨论：

1. 声明在类或结构体外的静态变量/函数。
2. 声明在类或结构体内部的静态变量/函数。

#### 1. 声明在类或结构体外的静态变量/函数。

一旦声明为静态的，只会在各自的翻译单元可见，可以理解在各自的翻译单元是私有的。

举例：

我们在 main.cpp 和 log.cpp 中分别声明静态变量 s_variable，如下：

```c++
// main.cpp
#include <iostream>


void Log();

static int s_variable = 10;

int main() {
    std::cout << s_variable << std::endl;
    Log();
}

```

```c++
// Log.cpp
#include <iostream>
static int s_variable = 5;


void Log() {
    std::cout << s_variable << std::endl;
}
```

编译、链接、运行打印 ：

10 

5

即 `s_variable` 在各自翻译单元是私有的，各自只能访问到自己定义的 `s_variable` 静态变量，函数也是同样的性质。

##### 1.1 全局变量

从上面看出，既然 静态声明在各自的翻译单元室私有的，那么普通的声明变量就是 **全局变量**，在上面的列子我们直接去掉 main.cpp 和 log.cpp 中的 static 关键字，运行发现在链接时异常：

![](https://p.ipic.vip/lm52yg.png)

明显是说：重复定义 s_variable 变量。

##### 1.2 使用 external 关键字 链接其他翻译单元的全局变量

这里如果想要使用 log.h 定义的全局变量 s_variable，可以使用 `external` 关键字，并删除 main.cpp 中赋值的操作，那么在链接时就会找到 log.h 中定义的全局变量 `s_variable`，代码如下：

```c++
void Log();

extern int s_variable;

int main() {
    std::cout << s_variable << std::endl;
    Log();
}
// 5
// 5
```

##### 1.3 在头文件中声明 静态变量

在头文件中声明 静态变量，分别将头文件引入 cpp 文件中，那么得到的效果和各自声明静态变量是一样的，因为预处理就是将其赋值到引入的 cpp 文件中，同理函数也一样。

#### 2. 声明在类或结构体内部的静态变量/函数。

这个概念基本就和 Java 类的静态变量和静态函数一致，

* 在 结构体/类 中的静态 是所有实例共享的块内存，在任何地方改变静态的值，每个实例中的静态值都将被改变。

* 静态方法无法访问非静态变量。
* 非静态方法，实际编译后是会有一个隐藏的本实例的参数，而静态方法没有这个隐藏参数。

