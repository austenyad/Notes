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

## 数组

数组的大小一般都需要自己维护在 C++ 中。

C++ 11 引入 std::array ，使用 std::array 比原生数组安全的多，相对的会产生多余的性能。

## 字符串

字符串本质上是一个接一个字符的数组。

```c
const char* name = "austenYang";
char name2[11] = {'a','u','s','t','e','n','Y','a','n','g',0};
```

C 语言风格的字符串声明。

C++ 中引入了 std::string 字符串类型，实际上 std::string 就是 const char * 类型。



字符宽度概念

```c++
   const char* name = "austenYang";

    const wchar_t* name2 = L"austenYang";

    const char16_t* name3 = u"austenYang";
    const char32_t* name4 = U"austenYang";
```

## 字符串字面量



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

### 2. 构造函数

**构造函数式**一种特殊的方法，一种每次你构造一个对象时都会调用的方法。

在不声明构造函数时，会默认有一个无参构造函数，只不过是这个构造函数什么也没有做。

不像 Java 语言，**C++ 基本类型的变量在初始化时都需要手动进行**。一般构造函数中进行初始化时最好的做法。

### 3. 析构函数

**析构函数** 正好与构造函数相反，当一个对象要被销毁时它将被调用。

析构函数是你卸载变量等东西并清理使用过的内存的地方。

析构函数 当在栈中创建的对象，当对象的作用域结束时析构函数会别调用。

析构函数 当在堆中创建的对象，当你调用 delete 时它将会被调用。

### 4. 继承

继承父类所有 public 的东西。

### 5.虚函数

在 父类中定义且可以在子类 override 的函数，称为虚函数。

使用虚函数要创建 虚函数表，在调用时也要遍历函数找到虚函数对应的被重新的函数。

```c++
class Entity {
public:
    virtual std::string GetName() { return "Entity"; }
};

class Player : public Entity {
private:
    std::string m_Name;

public:
    Player(const std::string &name) : m_Name(name) {
    }

    std::string GetName() { return m_Name; }
};

void Function(Entity *entity) {
    std::cout << entity->GetName() << std::endl;
}

int main() {
    Entity *e = new Entity();
    Function(e);
    Player *player = new Player("Player");
    Function(player);
    return 0;
}
// 打印结果
Entity
Player
```

调用 `Funciton函数`，不同的对象因为继承的原因都指向 `Entity` 类型，只有当 父类中的函数为虚函数时，多态的形式才会体现出来。

在 C ++ 11 中，对于在子类重写父类的虚函数特性中加入了 `override` 关键字，加入后编译器更能知道是在对 父类的虚函数进行重新，更能在编译期提前知道对父类函数重新的语法错误。

### 6. 纯虚函数

纯虚函数本地上和 Java 的 接口 或者 抽象方法 一样，允许在父类定义 完全没有函数体的虚函数强制让子类去实现。

比如 C++ 中的接口，在 C ++ 中没有单独的接口 interface 关键字，都是通过 Class 来实现 接口的，代码如下：

```c++

class Printable {
public:
    virtual std::string getClassName() = 0; // 接口函数定义 = 0 是必须的

    virtual ~Printable() = default; 
};

```

实现接口

```c++
class Printable {
public:
    virtual std::string getClassName() = 0;

    virtual ~Printable() = default;
};


class Entity : public Printable {
public:
    std::string getClassName() override {
        return "Entity";
    }

    ~Entity() override {
        std::cout << "Destroying entity" << std::endl;
    }
};

int main() {
    Printable *entity = new Entity();
    std::cout << entity->getClassName() << std::endl;
  	
  	delete entity; // 释放内存是，由于是多态，如果没有 virtual ~Printable() = default; 当调用 	delete entity; 只是调用 Printable 的析构函数，并不会调用 Entity 的析构函数
    return 0;
}

```

### 7. 可见性

**private**

类默认私有，结构体默认 public。

私有就是只有当前类可以访问到，其他继承或外部调用时都获取不到。

**protected**

子类可以访问到。

**public**

### 8. 成员初始化列表

在构造函数中初始化变量的一种方法，代码如下：

```c++
class Entity {
    int m_Score;
    std::string m_Name;
 
public:
    Entity():m_Score(100),m_Name("Unknow") {

    }
		
    const std::string &GetName() const { return m_Name; }
};
```

非常重要的点时：**初始化列表顺便必须和变量在类中声明的顺序一致**。

Why? 为什么我们需要初始化列表，单纯就是代码风格问题吗？

在你声明的类属性越多的时候就会体现出 初始化列表的优点，比如我有很多类属性都要在构造方法中初始化，代码如下：

```c++
class Entity {
    int m_Score;
    int x;
    int y;
    int z;
    int a;
    int b;
    int c;
    int d;
    int e;
    int f;
    std::string m_Name;

public:
    Entity() {
     x = 0;
     y = 0;
     z = 0;
     a = 0;
     b = 0;
     c = 0;
     d = 0;
     e = 0;
     f = 0;


        // 再做别的事情
 // 再做别的事情
       // 再做别的事情
    }
   

    const std::string &GetName() const { return m_Name; }
};
```

如果像上面代码一样类的构造函数就很凌乱，初始化 和 一些其他逻辑 都混杂在构造函数中。

所以初始化列表可以将 初始化变量 和 初始化的其他逻辑分开，让代码更加清晰、可读。



还有一点要注意的是，当类中使用到其他对象，如果在构造函数中初始化这个对象，如下：

```c++
class A {
public:
    A() {
        std::cout << "Created A" << std::endl;
    }
    A(const int x) {
        std::cout << "Created A with x " << x << std::endl;
    }
};

class Entity {
    std::string m_Name;
    A m_A;
public:
    Entity() {
        m_Name = "Unknown";
        m_A = A(8);
    }
    const std::string &GetName() const { return m_Name; }
};

int main() {
    Entity entity;
    return 0;
}

// 控制台打印
// Created A
// Created A with x 8
```

控制台会打印上面的结果，表明 A 对象被构造了两次。第二次构造的对象覆盖了第一次构造的对象。

要注意上面在某些情况下可能导致错误。

如果上面的情况使用初始化列表就不会有问题：

```c++
class A {
public:
    A() {
        std::cout << "Created A" << std::endl;
    }

    A(const int x) {
        std::cout << "Created A with x " << x << std::endl;
    }
};

class Entity {
    std::string m_Name;
    A m_A;

public:
    Entity() : m_A(A(8)) {
        m_Name = "Unknown";
    }

    const std::string &GetName() const { return m_Name; }
};

int main() {
    Entity entity;
    return 0;
}
```

上面说的是不使用初始化列表时造成对象对此被构造，但基本变量不会进行初始化除非你用赋值来初始化它们。

所以为了统一，我们应该使用初始列表来初始化值，不管事基本类型还是 Class 类型。

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

#### 3. 静态局部变量 

静态局部变量理解它的方式最好是用一个代码的例子：

在一个 main.cpp 文件中我们什么如下:

```c++
int i = 0;

void Function() {
    i++;
    std::cout << i << std::endl;
}
int main() {
    Function();
    Function();
    Function();
    Function();
    Function();
}
//打印结果：
1
2
3
4
5
```

 声明一个全局变量，在多次调用 `Fuction()` 的时候进行 i++ 。**如果我不想让这个 i 变量被其他函数访问到**，那么可以使用静态局部变量也可是实现上面效果。

```c++
void Function() {
   static  int i = 0;
    i++;
    std::cout << i << std::endl;
}
int main() {
    Function();
    Function();
    Function();
    Function();
    Function();
}
```

只要把全局变量放到函数局部并声明 `static` 就可以实现。

## 枚举

它只是给特定的值的一种命名方式



```c++
enum Level{
	Error,Warning,Info
}
```

枚举的背后就是整数。

设置枚举的大小，为 8 位无符号整数，使用这个可以节省内存大小。

```c++
enum Level :unsigned char {
    LevelError,LevelWarning,LevelInfo
};
```

## Const

1. const 修饰变量

```c++
const int MAX = 5; // 被 const 修饰称为常量：
MAX = 6; // Error




const int* a = new int; // 常量指针
*a = MAX; // Error

int const* b = new int; // 指针常量
*b = MAX; // 成立
b = &MAX; // Error 不成立



```

2. Const  修饰类中方法，无法修类中属性，只能读。

```c++
class Entity {
private:
    int m_X,m_Y;

public:
    int GetX() const {
        m_X = 1; // 无法修改类中 属性
        return m_X; 
    }
};

```

const 修改方法，当 m_X = 1 时报错，但是如果真的是要想修改 ，可以使用 `mutable` 修饰变量，`mutable` 允许方法是常量方法，但常量方法可以修改属性变量。

```c++
class Entity {
private:
    int m_X, m_Y;
    mutable int var ;
public:
    int GetX() const  {
        var = 1;
        return m_X;
    }
};
```



3. Const 修改类中方法，返回值。

```c++
class Entity {
private:
   int *m_X, * m_Y;

public:
    const int * const GetX() const {
        return m_X;
    }
};

int main() {
    Entity entity;
    const int *a = entity.GetX(); // 返回指针常量
    std::cout << a << std::endl;
    return 0;
}
```

4. 

```c++

class Entity {
private:
    int m_X, m_Y;

public:
    int GetX()  {
        return m_X;
    }
};

void PrintEntity(const Entity &e) {
								// 异常 : 
								// Cannot convert this argument from type const Entity to type Entity: function is missing const qualifier
    std::cout << e.GetX() << std::endl; 
}

int main() {
    Entity entity;
    return 0;
}

```

下面代码却正常：

```c++
class Entity {
private:
    int m_X, m_Y;

public:
    int GetX() const {
        return m_X;
    }
};

void PrintEntity(const Entity &e) {
    std::cout << e.GetX() << std::endl; 
}

int main() {
    Entity entity;
    return 0;
}
```

所以 记住，总是标记你的方法为 const ，如果它们实际上没有修改类，或者不应该修改类。

## mtable

两种使用情况

1. 在类中使用
2. 在 Lambda 中使用

```c++
// lambda 值传递
int main() {
    int x = 8;
    auto f = [=]() { // = 是值传递
        x++; // 这里出现异常
        std::cout << x << std::endl; // 9
    };
    f();
    std::cout << x << std::endl; // 8
    return 0;
}
```

```c++
// lambda 值传递
int main() {
    int x = 8;
    auto f = [&]() { // & 引用传递
        x++; // 正常运行
        std::cout << x << std::endl; // 9
    };
    f();
    std::cout << x << std::endl; // 8
    return 0;
}
```

## 三元操作

if ....else ... 语法糖：

```c++
int level = a > 5 ? 10 : 5;

std::string rank = s_Level > 10 ? "Master" : "Beginner";
```







## C++ 中创建对象

两种选择

1. 栈

```c++
class Entity {
    std::string m_Name;
public:
    Entity() : m_Name("Unknown"){}
    Entity(const std::string &name) : m_Name(name) {}
    const std::string &GetName() const { return m_Name; }
};
int main() {
    Entity entity; // 调用空构造方法，在栈上创建对象
    // Entity entity = Entity(); 与上面写法等价
    Entity entity2("Unknown"); // 调用一个参数的构造方法 在栈上创建对象
    // Entity entity2 = Entity("Unknown"); 与上面写法等价
    return 0;
}

```

一般情况下都会使用上面的方式创建对象，但是有两种情况不会创建栈对象。

a. 如果对象要 超出自己的**作用域** 或者 **栈帧**，这个作用域其实也是栈帧，如下：

```c++
class Entity {
    std::string m_Name;
public:
    Entity() : m_Name("Unknown") {}
    Entity(const std::string &name) : m_Name(name) {}
    const std::string &GetName() const { return m_Name; }
};

int main() {
    Entity *e; 
    {
        Entity entity("Unknown");
        e = &entity;
        std::cout << entity.GetName() << std::endl;
    }
   std::cout << e->GetName() << std::endl;
        return 0;
}

```

教程上讲的这个说  entity 超出 { } 作用域后就会被释放掉，但是从实际操作看并没有释放掉，为什么？



b. 还有一种情况不会在栈上创建对象，那就是栈上的内存分配太少了，可能就 1~2 MB ，但是创建的对象太多可能栈内存不够了，所有要在堆上创建对象。

2. 堆

```c++
 Entity *entity = new Entity();

delete entity;// 必须手动释放在堆中创建的内存
```

在堆上创建对象和栈上创建对象最大的区别就是 new 关键字，并且它返回了 entity 在堆上被分配的内存地址。上面的创建对象的方式就和 Java 一样了，Java 所有的 new 都是在堆上创建对象，这里要注意：习惯了 Java 创建对象都要使用 new 关键字，但是在 C++ 中不要创建对象都使用 new ，应用在合适的场景下才使用 new 来在堆上创建对象。简单来说就是 性能问题：

* 在堆上创建对象时间会更长；
* 在堆上创建对象你必须手动释放分配的内存。

### new 关键字

```c++
    Entity* e = new Entity(); // 1
    Entity* e = (Entity*)malloc(sizeof(Entity)); // 2 c语言风格

		delete e;//3
		free(e); //4 c语言风格

		int* b = new int[50];
		delete[] b;
```

* 上面1、2 两行代码做的事情是一样的，只不过 new 那一行还调用了 构造函数。
* 上面 3、4 都是释放内存，delete 那一行还调用了 析构函数。

### pleacement new ?

### 隐式构造函数

下面实例代码就是隐式转换：即隐式转换为构造函数。

```c++
class Entity {
    char *m_Name;
    int m_Age;

public:
    Entity(char *name): m_Name(name), m_Age(-1) {
        std::cout << "Entity 1" << std::endl;
    }
    Entity(int age): m_Name("Unknown"), m_Age(age) {
        std::cout << "Entity 2" << std::endl;
    }
};


int main() {
    Entity a = "austenYang"; // 隐式调用了 Entity(char *name) 构造函数
    Entity b = 22; // 隐式调用了 Entity(int age)构造函数 
    return 0;
}

```

隐式转换只允许转换一次

```c++

class Entity {
    std::string m_Name;
    int m_Age;

public:
    Entity(const std::string name): m_Name(name), m_Age(-1) {
        std::cout << "Entity 1" << std::endl;
    }

    Entity(int age): m_Name("Unknown"), m_Age(age) {
        std::cout << "Entity 2" << std::endl;
    }
};


int main() {
    Entity a = "austenYang"; // 异常
    Entity b = 22;
    return 0;
}
```

` Entity a = "austenYang";` 这行代码会异常因为 隐式转换了两次：

第一次："austenYang" 是 char* 类型 将 char* 转换为 std::string

第二次：将 std::string 隐式转换为 Entity

#### 关键字 explicit 禁止隐式转换

## 运算符+运算符重载

### 运算符

### 运算符重载

允许在程序中定义或更改运算符的行为，例子如下：

```c++
struct Vector2 {
    float x, y;

    Vector2(float x, float y) : x(x), y(y) {
    }


    Vector2 Add(const Vector2 &other) const {
        return Vector2(x + other.x, y + other.y);
    }
    // 重载操作符
    Vector2 operator+(const Vector2 &other) const {
        return Add(other);
    }

    Vector2 Multiply(const Vector2 &other) const {
        return Vector2(x * other.x, y * other.y);
    }
    // 重载操作符
    Vector2 operator*(const Vector2 &other) const {
        return Multiply(other);
    }
};


int main() {
    Vector2 position(4.0f, 5.0f);
    Vector2 speed(0.5f, 1.5f);
    Vector2 powerup(1.1f, 1.1f);

    Vector2 result = position.Add(speed.Multiply(powerup));
    Vector2 result2 = position + speed * powerup;

    return 0;
}
```

上面代码重载操作符的变形：

```c++
    Vector2 Add(const Vector2 &other) const {
        return *this + other;
    }

    // 重载操作符
    Vector2 operator+(const Vector2 &other) const {
        return Vector2(x + other.x, y + other.y);
    }
```

上面代码 *this 代表当前 Vector2 实例对象，this 就是 当前 Vector2 实例对象在内存中地址位置。

```c++
  Vector2 Add(const Vector2 &other) const {
        return operator+(other); // 
    }

    // 重载操作符
    Vector2 operator+(const Vector2 &other) const {
        return Vector2(x + other.x, y + other.y);
    }
```

调用已经定义好的 重载操作符方法。

重载操作符 << 为其添加新支持的类型：

```c++
std::ostream &operator<<(std::ostream &os, const Vector2 &v) {
    os << "(" << v.x << ", " << v.y << ")";
    return os;
}
```

## this 关键字

this 在类中是指向当前对象的指针。

## C++对象的生存周期（栈作用域生存期）

### 基于栈的变量一出作用域就会被释放、摧毁了。

一个很具有代表的例子是当你什么一个函数，返回在这个函数中什么的数组时：

```c++
int* CreateArray() {
    int arr[10];
    return arr;
}
int main() {
    int * a = CreateArray();
    return 0;
}
```

CreateArray 函数调用后 ，使用 `int arr[10]`  栈上创建数组，当函数执行完后函数栈帧退出，那么 arr 数组一定也会被释放掉，所以返回的 a 数字的地址不一定是正确的。

### 基于栈的特性更好的利用栈

举例：模拟类似的 智能指针，自动释放在堆上面创建的内存。

```c++
class Entity {
public:
    int x, y;
    Entity() {
        std::cout << "Created Entity" << std::endl;
    }
    ~Entity() {
        std::cout << "Destroyed Entity" << std::endl;
    }
};

class ScopePtr {
    Entity *m_Ptr;

public:
    ScopePtr(Entity *ptr) : m_Ptr(ptr) {
    }

    ~ScopePtr() {
        delete m_Ptr;
    }
};

int main() { {
        // ScopePtr ptr(new Entity());
        ScopePtr ptr2 = new Entity(); // 构造函数隐式转换
    }

    return 0;
}

```

上面代码我们使用 栈的特性 在 堆上创建了 new Entity()，把对象交付给栈上创建的 ScopePtr 对象，就实现了 堆上对象的 自动内存销毁。

## 智能指针

智能指针是 实现 创建 和 销毁 这个过程自动化的一种方式。智能指针本质上是一个原始指针的包装，当你创建一个智能指针时，它会调用 new 并为你分配内存，然后基于你使用的智能指针在某一时刻自动释放内存。

1. `unique_ptr`

unique_ptr 是作用域指针，超出作用域时，它会被销毁然后调用 delete。之所以叫做 unique 是因为你不能复制一个 unique_ptr ，一旦复制 就会有两个 unique_ptr 指向同一内存地址，如果其中一个释放了，另一个指向这块内存块也就释放掉了，出现野指针，所以不能复制 unique_ptr。**使用 unique_ptr 时唯一的参考就是当你需要一个作用域指针时使用的 智能指针。**

```c++
#include <memory>

class Entity {
public:
    Entity() {
        std::cout << "Entity created" << std::endl;
    }

    ~Entity() {
        std::cout << "Entity destroyed" << std::endl;
    }

    void Print() {
        std::cout << "Entity printed" << std::endl;
    }
};

int main() {

    {
        std::unique_ptr<Entity> entity(new Entity()); // 方式 1
        std::unique_ptr<Entity> entity2 = std::make_unique<Entity>(); // 方式2
        entity->Print();
        entity2->Print();
    }
    
}

```

上面提供两种使用 unique_ptr 智能指针的方式，方式二是最推荐的 因为它在出现异常时，它最终不会得到一个没有引用的悬空指针从而造成内存泄漏。

![](https://p.ipic.vip/baw08e.png)

这里 无法将 enity 赋值 给 entity3，也就是无法进行复制拷贝，在 unique_ptr 实现中，删除了拷贝和赋值函数。

![](https://p.ipic.vip/0n9qvt.png)

2. `shared_ptr` 

shared_ptr 可以进行共享的智能执行，它的实现方式取决于编译器和你所使用编译器中使用的标准库，实现方式大多是使用 引用计数

```c++
int main() {
    {
        std::shared_ptr<Entity> shared0;
        {
            std::shared_ptr<Entity> shared = std::make_shared<Entity>(); // 创建 shared_ptr
            shared0 = shared;
        }
    }
  
  	// 离开这个作用域对象才会摧毁
    return 0;
}

```

## 赋值与拷贝函数

1. 什么时复制？

```c++
int a = 2;
int b = a;
```

```c++
struct Vector2{
	 float x,y;
}

Vector2 a = {2,3};
Vector2 b = a;
```

像上面这种声明一个变量 a ，然后赋值给 b 变量，就是 值赋值，也叫浅拷贝。因为你修改 b 变量并不会影响 a 的值，实际就是在 a 和 b 两个不同的内存地址上，有相同的值或者数据结果。

```c++
struct Vector2{
	 float x,y;
}

Vector2* a = new Vector2();
Vector2* b = a;
b->x = 2;
```

当你编写一个变量给这个变量赋值时，你总是在复制。在值的情况下复制的是值；在指针的情况下，复制的是指针即内存地址，内存地址的数字，就是数字而已，而不是指针指向的实际内存



String 类的实现

```c++
class String {
private:
    char *m_Buffer;
    unsigned int m_Size;

public:
    String(const char *string) {
        m_Size = strlen(string);
        m_Buffer = new char[m_Size + 1];
        memcpy(m_Buffer, string, m_Size);
        m_Buffer[m_Size] = 0; // 字符串终止符 0/null
    }

    ~String() {
        delete[] m_Buffer;
    }

    // 声明友元函数声明
    friend std::ostream &operator<<(std::ostream &os, const String &string);
};
// 声明友元函数: 在类的外部作用域可以访问类中的私有成员
std::ostream &operator<<(std::ostream &os, const String &string) {
    os << string.m_Buffer;
    return os;
}

int main() {
    String first = "Hello World!"; // 隐式构造函数
    String second = first; // 赋值 即 赋值操作

    std::cout << first << std::endl;
    std::cout << second << std::endl;
    return 0;
}

```

上面代码运行会报错：

```c++
H(1229,0x1eefa60c0) malloc: *** error for object 0x6000001a0030: pointer being freed was not allocated
H(1229,0x1eefa60c0) malloc: *** set a breakpoint in malloc_error_break to debug
```

报错的意思是释放了没有申请的内存，这是因为：`String second = first;` 的操作是复制，复制了实例对象的所有，像上面这种称为 浅拷贝，由于复制了所有，那么 `second` 实例中的 `m_Buffer` 指向了同一个地址，当程序执行完后会依次调用析构函数，`first` 和 `second` 实例对象的析构函数都会调用，`first` 实例对象调用完后 `m_Buffer` 指向的内存已经被释放掉了，当调用 `second` 的析构函数时 对象拷贝过来的 `m_Buffer` 保存的内存地址还是指向 `first` 中 `m_Buffer` 已经释放的内存，那么这就造成了对已经释放的内存再此释放内存就出现了：`pointer being freed was not allocated`，对于计算机来说 已经释放的内存它就认为是 没有被申请使用的内存所以出现了：释放了没有申请的内存

以上面的例子为基础，如果想要不出现上面的报错，那么就要在拷贝时将让像 `m_Buffer` 这样的指针变量有它们自己的内存块，同时内容也被拷贝过来，像这样的拷贝叫做 深拷贝，也就是说复制了整个对象，而不是上面的那种 浅拷贝



## 拷贝构造函数

C++ 默认提供了一个拷贝函数，它的默认行为就是进行 浅拷贝，如果你决定不需要拷贝函数，不运行赋值那么可以声明：

```c++
 String(const String &other) = delete;
```

如果进行深拷贝那么我们改变默认构造函数的行为即重新这个默认拷贝构造函数：

```c++
// 拷贝函数的语意感觉很奇怪,我们将上面的赋值语句拿下来

String first = "Hello World!"; // 隐式构造函数
String second = first; // 赋值 即 赋值操作 ：我们新需要的知识知道 这个其实是调用 拷贝构造函数 来进行赋值语句

String(const String &other): m_Size(other.m_Size) {
    m_Buffer = new char[m_Size + 1];
    memcpy(m_Buffer, other.m_Buffer, m_Size + 1);
}
```

解释上面的 拷贝构造函数 ：

1. 参数 `&other`  其实对应的是 ：`first` 实例。
2. `:m_Size(other.m_Size)`  对于基本变量我们使用浅拷贝，这里实现方式是使用初始化列表来拷贝。
3.   `m_Buffer = new char[m_Size + 1];` 上面拷贝完 `m_Size` 那么 `m_Size` 就有值了，这里就开始创建新的内存块来对字符串进行深拷贝。
4. `memcpy(m_Buffer, other.m_Buffer, m_Size + 1);`  调用函数来进行字符拷贝，其中 m_Buffer 是 dest ，`other.m_Buffer` 是 `first.m_Buffer` 即 src ，字符的完整长度就是 `m_Size + 1` 因为还有 字符串终止符。
5. 这里没有像构造函数中执行：`m_Buffer[m_Size] = 0;` 来给字符串加上终止符，因为我们是拷贝字符串，那么就说明要被拷贝的字符已经有了终止符，即 `first` 实例中的 `m_Buffer` 已经有了终止符所有这里就不需要再进行特意对字符串再进行终止符赋值了。

需要注意的在有些时候为了性能我们要避免拷贝，比如函数调用：

```c++
void PrintString(String string) {
    std::cout << string << std::endl;
}

int main() {
    String first = "Hello World!";
    String second = first;

    PrintString(first);
    PrintString(second);
    return 0;
}
```

上面调用 `PrintString` 打印 String ，运行程序正常打印，但是这里出现了不必要的拷贝，这是我们在 String 的拷贝函数中添加测试打印语句：

```c++
  String(const String &other): m_Size(other.m_Size) {
        m_Buffer = new char[m_Size + 1];
        memcpy(m_Buffer, other.m_Buffer, m_Size + 1);
        std::cout << "Copied" << std::endl;
    }

```

会发现打印如下：

```c++
Copied // 这一次是  String second = first; 赋值语句触发的拷贝
Copied // 调用函数 PringString 触发拷贝
Hello World!
Copied // 调用函数 PringString 触发拷贝
Hello World!
```

调用函数将参数传递给 `PringString`，其实传递的过程中出现的拷贝，像这种拷贝显然是不必要的，那么避免不必要的拷贝，那么我们使用 引用或者指针就能解决：

```c++
void PrintString(const String &string) { 	
    std::cout << string << std::endl;
}
```

所有在编写代码时要注意什么时候该拷贝，该使用什么类型的拷贝，以及如何避免不必要的拷贝是非常重要的。

**传递对象时使用 const 修改函数形参这个是非常必要的，它可能在某些情况下复制的更快，所以先记住在传递对象时总是标记 const** 

## 箭头运算符

### 箭头运介绍

```c++
class Entity {
public:
    void Print() const {
        std::cout << "Hello World!" << std::endl;
    }
};

int main() {
    Entity e;
    e.Print();

    Entity* entity = &e;
    (*entity).Print();
    return 0;
}
```

上面代码 指针变量 entity 调用 类中的方法 Print 函数，在没有箭头运算符时需要 逆向引用 来调用类的 Print 方法。上面代码声明的 enity 地址位置，也就是一个地址位置的数值，一个整数，那么想要调用类的对象方法就需要先使用逆向引用将地址转换为对象，然后才能调用方法。想要使用 entity 这个数值直接调用类方法，那么就使用箭头运算符，**箭头运算符就是上面所说的 逆向引用再调用类对象方法的 便捷方式。**

```c++
 entity->Print();
```

### 箭头运算符的使用

重写箭头操作符的列子：

1. 重写箭头操作符：智能指针的内部实现原理

```c++
class Entity {
    float x, y;

public:
    void Print() const {
        std::cout << x << " " << y << std::endl;
    }
};

class ScopePtr {
    Entity *m_Entity;

public:
    ScopePtr(Entity *entity) : m_Entity(entity) {
    }

    ~ScopePtr() {
        delete m_Entity;
    }

    Entity *operator->() {
        return m_Entity;
    }
};

int main() {
    ScopePtr entity = new Entity();
    entity->Print();


    return 0;
}
```

重写后直接调用 entity 的方法。

2. 使用特殊办法获取 变量在内存中的偏移量

```c++
struct Vector3 {
    int x, y, z;
};

int main() {
    int offset = (size_t) &((Vector3 *) nullptr)->z;
    // int offset=(int)&(((Vector3*)nullptr)->x); 32 位操作系统中成立
    std::cout << offset << std::endl;
    return 0;
}

```

