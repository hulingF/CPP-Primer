## 第一章：绪论

`程序设计语言`的发展：开始人们使用计算机能够识别的`二进制机器语言`进行程序编写，但是因为机器语言记忆困难，于是引入了`ADD`、`SUB`等`助记符`形成了`汇编语言`，当然==汇编语言和机器语言抽象程度都很低，甚至需要关注机器硬件的具体细节==，仅仅适合于编写一些简单的数值计算程序，`聪明的大佬们又在汇编语言基础上进一步抽象和封装形成了高级语言`以便于编写大型复杂软件。

`程序设计方法`的发展：程序设计方法与程序设计语言的发展相辅相成，机器语言、汇编语言、早期的高级语言都使用面向过程的设计方法，着重于设计问题的解决方案，但是这种方法很难适用于大型软件的开发例如3A游戏开发，于是面向对象的程序设计方法随之提出，它把实际世界中的任何事物都看成一个个对象，对象内部封装了自己的属性和行为以保证安全性和隐私，对象与对象之间通过消息传递通信，对象甚至可以通过继承复用已有对象，通过多态增加对象的灵活性。

`程序的开发过程`：高级语言书写的`源程序`需要通过一个`翻译程序`翻译成`计算机能够识别的目标程序`，这种翻译程序有三种：`汇编程序`、`编译程序`、`解释程序`，解释程序虽然需要边解释边执行速度较慢，但也有其存在的价值和意义，例如Java语言就是一种半编译半解释型的语言，首先通过Java编译器编译形成中间的二进制文件(字节码)，不同平台(Linux、Windows、MacOS)上的JVM解释执行字节码文件。

## 第二章：简单程序设计

```c++
#include<iostream>

using namespace std;

int main(){
    int a;
    a = 10;
    cout<<"Hello C++!"<<endl;
    return 0;
}
```

C++完全兼容C语言，因此C++不是纯面向对象的语言，同样支持面向过程的编程，我们也推荐使用C++编程时尽量使用面向对象的程序设计方法。上述最简单的C++程序为例，首先大体结构跟C很接近，main方法是程序的入口函数，main方法内部调用了iostream类的标准输出对象cout，这需要程序开始使用#include<iostream>，`include其实是一种编译预处理，仅仅是把库文件纹丝不动地拷贝至相应位置处`，而为了避免命名冲突，C++标准库的类和对象都默认位于std命名空间，使用using namespace std是一种偷懒的做法，正确的方法是使用std::cout，而变量存放的数值跟变量有没有初始化、变量位于的位置都有关，例如未初始化的全局变量会默认初始化，未初始化的局部变量存放的可能是垃圾数据。

基本数据类型：整型、浮点型、布尔类型、字符型，注意IOS C++标准没有指定每种数据类型的占用字节数和表示范围，仅仅说明了相对大小关系，具体占用字节数跟我们的实际机器环境相关，可以使用sizeof关键字查看。

C++中有常量关键字const，`常量必须定义时初始化`，书面的字符串常量和字符数组表示的字符串变量都是C风格的字符串(末尾有'\0')，建议使用C++标准库的string类型。

注意：运算符优先级不需要记忆，熟悉即可，`逻辑运算符具有短路特性`，`位运算多用于标志位操作`，而像`算术运算符等都要求运算符两边类型相同，否则发生隐式类型转换，也就是向表示数据范围更大的类型转换`，我们也可以显式类型转换，推荐使用C++的类型转换操作符例如 static_cast<int>

> 分支循环语句都很简单，这里需要注意：if-else嵌套级别需要注意匹配，else与之前最近的未匹配的if匹配对应

类型别名：typedef 旧类型名 新类型名或者using 新类型名 = 旧类型名

auto类型和decltype类型：`auto类型是编译器通过初始值自动推断变量的类型`例如auto val = val1 + val2;，`decltype类型是定义一个变量与某一表达式的类型相同，但并不用该表达式初始化变量`例如decltype(i) j = 2;

## 第三章：函数

函数的参数传递：`传值`和`传引用`两种，传值是单向传递，函数内部对形参的修改不会影响实参的值，传引用是双向传递，需要谨慎操作形参，当然`我们也经常使用常引用保证实参的安全性`。

> 传引用很多情况下是因为复杂类型空间消耗大，函数调用时影响内存开销和运算速度

#### **引用的底层实现**：

1、`引用并非对象，相反的，它只是为一个已经存在的对象所起的另外一个名字`。引用的定义`必须伴随初始化`，而且一旦定义了引用，就无法令其再绑定到另外的对象，之后每次使用这个引用都是访问它最初绑定的那个对象。

2、对于面向对象来说，对象就是类的实例，是抽象化的数据本身。更广义的来说，一个int型变量可以是对象，一个指针也可以是对象，但一个引用却又不是对象。`可以理解：在语言层面上，占用内存的变量都可以称之为对象`。

3、引用占内存吗？在语言层面上：不占内存！不信看下面的简单程序：

```c++
#include <iostream>
 
int main() {
    int x = 0;
    int &r = x;
    int *p1 = &x;
    int *p2 = &r;
    std::cout << p1 << std::endl;
    std::cout << p2 << std::endl;
    return 0;
}

输出：
0x7ffd685231dc
0x7ffd685231dc
```

4、窥视底层呢？ 再来看一段代码：

```c++
int function() {
    int x = 0;
    int* ptr = &x;
    int& r = x;
    r = 1;
    return r;
}
```

汇编代码：

```
pushq %rbp
movq %rsp, %rbp           //拷贝栈指针
movl $0, -20(%rbp)        //int x = 0
leaq -20(%rbp), %rax      
movq %rax, -8(%rbp)       //int* ptr = &x
leaq -20(%rbp), %rax
movq %rax, -16(%rbp)      //int& r = x
movq -16(%rbp), %rax
movl $1, (%rax)           //r = 1
movq -16(%rbp), %rax
movl (%rax), %eax        
popq %rbp
ret                       //return r
```

![image-20221103203420501](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221103203420501.png)

> movl $0, -20(%rbp)
>
> 首先在栈的%rbp-20的地方分配了一个4字节的空间储存变量x=0；
>
> leaq -20(%rbp), %rax
>
> movq %rax, -8(%rbp)
>
> 把x的地址保存到%rbp-8的栈空间处，int* ptr = &x;
>
> leaq -20(%rbp), %rax
>
> movq %rax, -16(%rbp)
>
> 把x的地址保存到%rbp-16的栈空间处， int& r = x;
>
> movq -16(%rbp), %rax
>
> movl $1, (%rax)
>
> 将%rbp-16栈空间处的值改写成1， r = 1;
>
> movq -16(%rbp), %rax
>
> movl (%rax), %eax
>
> popq %rbp
>
> ret
>
> 将%rbp-16栈空间处的值返回， return r;

思路很清晰了。`创建指针和创建引用的汇编代码几乎完全一样，在栈上分配空间，保存变量x的地址，所有对引用变量r的操作实际都是通过这个地址在操作x`。和指针有区别吗？没有。占用内存吗？从底层来看，确实占用了内存。

5、小结
`本质上，引用和指针没有区别。只不过在语言层面上，C++设计者将通过指针来操作引用的实现细节隐藏了`。不过，我们依然可以肯定：

- 定义一个引用就是定义一个指针，这个指针保存引用对象的地址，且`指针类型为const，不可以再指向其他对象`；
- `每次对引用变量的使用，实际都伴随着解引用，只是我们看不到符号*`；

#### **内联函数的理解**：

1、内联函数（有时称作在线函数或==编译时期展开函数==）是一种编程语言结构，c++中以关键词inline修饰，用来建议编译器对一些特殊函数进行内联扩展（有时称作在线扩展）；也就是说建议编译器将指定的函数体插入并取代每一处调用该函数的地方(上下文)，从而节省了每次调用函数带来的额外时间开支。

2、想要理解内联函数是如何作用的，先看如下一段简易的cpp代码：

```c++
#include <iostream>
using namespace std;

int Add(int a,int b)
{
	return (a + b);
}

int main()
{
	int rez1=Add(1, 2);
	int rez2=Add(2, 4);
	cout << rez1 << " " << rez2 << endl;
	return 0;
}
```

这段代码频繁调用该“较短函数”，在内存中反复压栈开空间，需要使用内联函数，在此之前我们先看一下它的汇编代码：

![image-20221103205458542](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221103205458542.png)

接下来使用内联修饰符再观察：

```c++
inline int Add(int a,int b)
{
	return (a + b);
}
```

![image-20221103205543475](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221103205543475.png)

显然这里没有再出现call的指令，而是通过寄存器完成了加法操作，并且每个add函数都如是操作，说明我们的inline起作用了，函数成功被展开。

3、内联函数的重要特点：

- 内联函数是经典的空间换时间的做法，当代码指令很多（函数内部达到10行左右的指令就不考虑使用）或是递归都不能使用内联函数。

> 在编译的角度理解：假如一个函数编译后内部有10个指令，不使用inline调用100次，那么最后会出现110条指令；而如果使用inline关键字，函数直接展开，最后出现1000条指令

- 内联函数仅仅是对编译器的建议，并不是说调用了inline，函数就一定会被展开，前提是函数不是递归或者较长函数。
- `在类中定义的函数都是内联函数`。
- `声明和定义分离的函数不建议使用内联，经常会在链接时出现找不到的错误`。因为inline展开函数后函数地址就已经消失不见。

4、引申C语言

在c++中有内联函数，`在c语言中也可以使用宏定义的办法来达到相同的目的`，就比如前文举例的简单Add函数，直接写一个宏来实现该函数，通过宏替换也能减少栈开销：

```c
#define ADD(x,y) ((x)+(y)) 
```

显然，相比于宏，c++的inline内联使用更加简单不易出错，只需要在函数前加上inline关键字即可，非常好用~nice

#### **constexpr的理解：**

1、简介：`constexpr函数指的是在编译的时候就能得到其返回值的函数`，也就是说编译器将constexpr函数直接转换成其返回值，因此，constexpr函数都是被隐式地定义为内联函数。使用constexpr关键字来修饰constexpr函数。

2、使用方法：

```c++
constexpr int myFunc()
{
  return 1;
}
constexpr int i = myFunc() * 4;
```

此时，编译器会将myFunc()函数用其返回值1来代替，在编译时就可知i的值是4。

3、constexpr的使用与具体的编译器紧密相关，以下以g++编译器为准

- constexpr类似于内联函数，函数的定义需要在函数调用之前！！

```c++
constexpr int func(int);

int main(){
    constexpr int b = func(2);
    return 0;
}

constexpr int func(int i){
    int j = 10;
    if(j>0){
        return 100;
    }
    return i;
}

错误信息：constexpr int func(int)' used before its definition
```

- ==必须明确一点，在constexpr声明中如果定义了一个指针，限定符conxtexpr仅对指针有效，与指针所指的对象无关。==

```c++
constexpr int func(int i){
    int j = 10;
    if(j>0){
        return 100;
    }
    return i;
}

int main(){
    constexpr int a = func(2);
    constexpr int *p = &a;
    *p = 5;
    cout<<a<<endl;
    return 0;
}

错误信息1：invalid conversion from 'const int*' to 'int*
这表示constexpr修饰的a是一个常量，而constexpr修饰的对象不是int而是指针p
错误信息2：const int * 类型的值不能用于初始化 int *const
这再次表明上面的结论
```

- constexpr指针所指变量必须是全局变量或者static变量(既存储在静态数据区的变量)。

> 全局变量和局部变量的存储区域不同，全局变量存放在静态数据区，局部变量存放在栈区。但还有一个小点就是存放在静态数据区的变量是由低地址向高地址存放的，但存放在栈区的变量却是由高地址向低地址存放的，存放在静态数据区的还有静态局部变量和静态全局变量。

==注意：理解C++的语法多从编译器的角度思考，很多问题便迎刃而解！==

带默认参数值的函数：有默认参数的形参必须列在形参列表的最右，即默认参数值的右面不能有无默认值的参数；如果一个函数有原型声明，且原型声明在定义之前，则默认参数值应在函数原型声明中给出。

函数重载：C++允许功能相近的函数在相同的作用域内以相同函数名声明，从而形成重载。方便使用，便于记忆。

- 重载函数的形参必须不同:个数不同或类型不同。
- 编译程序将根据实参和形参的类型及个数的最佳匹配来选择调用哪一个函数。

## 第四章：类和对象

C++中构造函数详解：

1、作用：在创建一个新的对象时，自动调用的函数，用来进行初始化工作：对这个对象内部的数据成员进行初始化。

2、构造函数的特点：

- 自动调用（在创建新对象时，自动调用）
- 构造函数的函数名，和类名相同
- 构造函数没有返回类型
- 可以有多个构造函数（即函数重载形式）

3、构造函数的种类：

- 默认构造函数（无参）
- 自定义的构造函数
- 拷贝构造函数
- 赋值构造函数

4、默认构造函数：没有参数的构造函数，称为默认构造函数。

:first_quarter_moon:合成构造函数

没有手动定义默认构造函数时，编译器自动为这个类定义一个构造函数。

1）如果数据成员使用了“类内初始值”，就使用这个值来初始化数据成员。【C++11】

2）否则，就使用默认初始化（实际上，不做任何初始化）

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;

// 定义一个“人类”
class Human {
public:  //公有的，对外的
    void eat(); //方法， “成员函数”
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();

private:
    string name;
    int age = 18;
    int salary;
};

void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}

int main(void) {
    Human  h1;  // 使用合成的默认初始化构造函数
    cout << "年龄: " << h1.getAge() << endl;     //使用了类内初始值
    cout << "薪资：" << h1.getSalary() << endl;  //没有类内初始值

    system("pause");
    return 0;
}
```

:first_quarter_moon_with_face: 手动定义的默认构造函数

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;

// 定义一个“人类”
class Human {
public:  //公有的，对外的
    Human(); //手动定义的“默认构造函数”
    void eat(); //方法， “成员函数”
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}


int main(void) {
    Human  h1;  // 使用自定义的默认构造函数
    cout << "姓名：" << h1.getName() << endl;
    cout << "年龄: " << h1.getAge() << endl;
    cout << "薪资：" << h1.getSalary() << endl;

    system("pause");
    return 0;
}
```

> 如果某数据成员使用类内初始值，同时又在构造函数中进行了初始化， 那么以构造函数中的初始化为准。相当于构造函数中的初始化，会覆盖对应的类内初始值。

:dart:自定义的重载构造函数

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);

    void eat();
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";
}


void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}


int main(void) {
    Human  h1(25, 35000);  // 使用自定义的默认构造函数

    cout << "姓名：" << h1.getName() << endl;
    cout << "年龄: " << h1.getAge() << endl;
    cout << "薪资：" << h1.getSalary() << endl;

    system("pause");
    return 0;
}
```

:diamond_shape_with_a_dot_inside:拷贝构造函数

1. 手动定义的拷贝构造函数

```c++
#include <iostream>
#include <Windows.h>
#include <string>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);
    Human(const Human&);

    void eat();
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";
}

Human::Human(const Human& man) {
    cout << "调用自定义的拷贝构造函数" << endl;
    name = man.name;
    age = man.age;
    salary = man.salary;
}


void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}


int main(void) {
    Human  h1(25, 35000);  // 使用自定义的默认构造函数
    Human  h2(h1);  // 使用自定义的拷贝构造函数

    cout << "姓名：" << h2.getName() << endl;
    cout << "年龄: " << h2.getAge() << endl;
    cout << "薪资：" << h2.getSalary() << endl;

    system("pause");
    return 0;
}
```

2. 合成的拷贝构造函数

```c++
#include <iostream>
#include <Windows.h>
#include <string>
#include <string.h>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);
    //Human(const Human&);  //不定义拷贝构造函数，编译器会生成“合成的拷贝构造函数”

    void eat();
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();
    void setAddr(const char *newAddr);
    const char* getAddr();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
    char *addr;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";

    addr = new char[64];
    strcpy_s(addr, 64, "China");
}

void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}

void Human::setAddr(const char *newAddr) {
    if (!newAddr) {
        return;
    }

    strcpy_s(addr, 64,  newAddr);
}

const char* Human::getAddr() {
    return addr;
}
int main(void) {
    Human  h1(25, 35000);  // 使用自定义的默认构造函数
    Human  h2(h1);  // 使用合成的拷贝构造函数

    cout << "h1 addr:" << h1.getAddr() << endl;
    cout << "h2 addr:" << h2.getAddr() << endl;

    h1.setAddr("长沙");

    cout << "h1 addr:" << h1.getAddr() << endl;
    cout << "h2 addr:" << h2.getAddr() << endl;

    system("pause");
    return 0;
}
```

==说明：合成的拷贝构造函数的缺点: 使用“浅拷贝”解决方案：**在自定义的拷贝构造函数中，使用深拷贝**==

```c++
#include <iostream>
#include <Windows.h>
#include <string>
#include <string.h>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);
    Human(const Human&);  //不定义拷贝构造函数，编译器会生成“合成的拷贝构造函数”

    void eat();
    void sleep();
    void play();
    void work();

    string getName();
    int getAge();
    int getSalary();
    void setAddr(const char *newAddr);
    const char* getAddr();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
    char *addr;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";

    addr = new char[64];
    strcpy_s(addr, 64, "China");
}

Human::Human(const Human &man) {
    cout << "调用自定义的拷贝构造函数" << endl;
    age = man.age;      //this是一个特殊的指针，指向这个对象本身
    salary = man.salary;
    name = man.name;
    // 深度拷贝
    addr = new char[64];
    strcpy_s(addr, 64, man.addr);
}

void Human::eat() {
    cout << "吃炸鸡，喝啤酒！" << endl;
}

void Human::sleep() {
    cout << "我正在睡觉!" << endl;
}

void Human::play() {
    cout << "我在唱歌! " << endl;
}

void Human::work() {
    cout << "我在工作..." << endl;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}

void Human::setAddr(const char *newAddr) {
    if (!newAddr) {
        return;
    }

    strcpy_s(addr, 64, newAddr);
}

const char* Human::getAddr() {
    return addr;
}

int main(void) {
    Human  h1(25, 35000);  // 使用自定义的默认构造函数
    Human  h2(h1);  // 使用自定义的拷贝构造函数

    cout << "h1 addr:" << h1.getAddr() << endl;
    cout << "h2 addr:" << h2.getAddr() << endl;

    h1.setAddr("长沙");

    cout << "h1 addr:" << h1.getAddr() << endl;
    cout << "h2 addr:" << h2.getAddr() << endl;

    system("pause");
    return 0;
}
```

:large_orange_diamond:什么时候调用拷贝构造函数?

1. 调用函数时，实参是对象，形参不是引用类型，如果函数的形参是引用类型，就不会调用拷贝构造函数。
2. 函数的返回类型是类，而且不是引用类型。
3. 对象数组的初始化列表中，使用对象。

```c++
#include <iostream>
#include <Windows.h>
#include <string>
#include <string.h>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);
    Human(const Human&);  //不定义拷贝构造函数，编译器会生成“合成的拷贝构造函数”

    string getName();
    int getAge();
    int getSalary();
    void setAddr(const char *newAddr);
    const char* getAddr();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
    char *addr;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";

    addr = new char[64];
    strcpy_s(addr, 64, "China");
}

Human::Human(const Human &man) {
    cout << "调用自定义的拷贝构造函数" << "参数：" << &man
         << " 本对象：" << this << endl;

    age = man.age;      //this是一个特殊的指针，指向这个对象本身
    salary = man.salary;
    name = man.name;
    // 深度拷贝
    addr = new char[64];
    strcpy_s(addr, 64, man.addr);
}


string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}

void Human::setAddr(const char *newAddr) {
    if (!newAddr) {
        return;
    }

    strcpy_s(addr, 64, newAddr);
}

const char* Human::getAddr() {
    return addr;
}


void test1(Human man) {
    cout << man.getSalary() << endl;
}

void test2(Human &man) { //不会调用拷贝构造函数，此时没有没有构造新的对象
    cout << man.getSalary() << endl;
}

Human test3(Human &man) {
    return man;
}

Human& test4(Human &man) {
    return man;
}


int main(void) {
    Human h1(25, 35000);  // 调用默认构造函数
    Human h2(h1);         // 调用拷贝构造函数
    Human h3 = h1;        // 调用拷贝构造函数

    test1(h1);            // 调用拷贝构造函数
    test2(h1);            // 不会调用拷贝构造函数
    test3(h1);            // 创建一个临时对象，接收test3函数的返回值，调用1次拷贝构造函数
    Human h4 = test3(h1); // 仅调用1次拷贝构造函数，返回的值直接作为h4的拷贝构造函数的参数
    test4(h1);            // 因为返回的是引用类型，所以不会创建临时对象，不会调用拷贝构造函数

    Human men[] = { h1, h2, h3 }; //调用3次拷贝构造函数
    return 0;
}
```

:koala:赋值构造函数

```c++
#include <iostream>
#include <Windows.h>
#include <string>
#include <string.h>

using namespace std;

// 定义一个“人类”
class Human {
public:
    Human();
    Human(int age, int salary);
    Human(const Human&);  //不定义拷贝构造函数，编译器会生成“合成的拷贝构造函数”
    Human& operator=(const Human &);

    string getName();
    int getAge();
    int getSalary();
    void setAddr(const char *newAddr);
    const char* getAddr();

private:
    string name = "Unknown";
    int age = 28;
    int salary;
    char *addr;
};

Human::Human() {
    name = "无名氏";
    age = 18;
    salary = 30000;
}

Human::Human(int age, int salary) {
    cout << "调用自定义的构造函数" << endl;
    this->age = age;      //this是一个特殊的指针，指向这个对象本身
    this->salary = salary;
    name = "无名";

    addr = new char[64];
    strcpy_s(addr, 64, "China");
}

Human::Human(const Human &man) {
    cout << "调用自定义的拷贝构造函数" << "参数：" << &man
         << " 本对象：" << this << endl;

    age = man.age;      //this是一个特殊的指针，指向这个对象本身
    salary = man.salary;
    name = man.name;
    // 深度拷贝
    addr = new char[64];
    strcpy_s(addr, 64, man.addr);
}


Human& Human::operator=(const Human &man) {
    cout << "调用" << __FUNCTION__ << endl;
    if (this == &man) {
        return *this; //检测是不是对自己赋值：比如 h1 = h1;
    }

    // 如果有必要，需要先释放自己的资源（动态内存）
    delete addr;
    addr = new char[64];

    // 深拷贝
    strcpy_s(addr, 64, man.addr);

    // 处理其他数据成员
    name = man.name;
    age = man.age;
    salary = man.salary;

    // 返回该对象本身的引用， 以便做链式连续处理，比如 a = b = c;
    return *this;
}

string Human::getName() {
    return name;
}

int Human::getAge() {
    return age;
}

int Human::getSalary() {
    return salary;
}

void Human::setAddr(const char *newAddr) {
    if (!newAddr) {
        return;
    }

    strcpy_s(addr, 64, newAddr);
}

const char* Human::getAddr() {
    return addr;
}


void test(Human man) {
    cout << man.getSalary() << endl;
}

void test2(Human &man) { //不会调用拷贝构造函数，此时没有没有构造新的对象
    cout << man.getSalary() << endl;
}

Human test3(Human &man) {
    return man;
}

Human& test4(Human &man) {
    return man;
}


int main(void) {
    Human h1(25, 35000);  // 调用构造函数

    // 特别注意，此时是创建对象h2并进行初始化，调用的是拷贝构造函数，
    // 不会调用赋值构造函数
    Human h2 = h1;

    h2 = h1; //调用赋值构造函数
    h2 = test3(h1); //发生拷贝构造创建临时对象，使用临时对象调用赋值构造函数

    Human h3 = test3(h1); //调用拷贝构造函数
    return 0;
}
```

==如果没有定义赋值构造函数，编译器会自动定义“合成的赋值构造函数”，与其他合成的构造函数，是“浅拷贝”（又称为“位拷贝”）。==

C++中的析构函数：

- 在对象的生存期结束的时刻系统自动调用它，然后再释放此对象所属的空间。
- 如果程序中未声明析构函数，编译器将自动产生一个默认的析构函数，其函数体为空。

类的组合：类成员可以是其他类的对象，此时需要注意组合类的构造函数的设计，往往需要在组合类的构造函数中进行组件类对象的初始化构造(如果没有显式初始化类对象，默认调用组件类的默认构造函数)

> 注意：类成员对象构造函数调用次序与它们在类中声明的顺序有关，与初始化列表的顺序无关

前向引用声明：如果需要在某个类的声明之前，引用该类，则应进行前向引用声明。前向引用声明只为程序引入一个标识符，但具体声明在其他地方。

```c++
class B;  //前向引用声明

class A {

public:

  void f(B b);

};

class B {

public:

  void g(A a);

};
```

前向引用声明注意事项

------

- 使用前向引用声明虽然可以解决一些问题，但它并不是万能的。

- 在提供一个完整的类声明之前，不能声明该类的对象，也不能在内联成员函数中使用该类的对象。

- 当使用前向引用声明时，只能使用被声明的符号，而不能涉及类的任何细节。

```c++
class Fred; //前向引用声明

class Barney {

   Fred x; //错误：类Fred的声明尚不完善

};

class Fred {

   Barney y;

};
```

C++中的结构体：结构体是一种特殊形态的类。与类的唯一区别：类的缺省访问权限是private，结构体的缺省访问权限是public

- 结构体存在的主要原因：与C语言保持兼容

- 什么时候用结构体而不用类

- - 定义主要用来保存数据、而没有什么操作的类型
  - 人们习惯将结构体的数据成员设为公有，因此这时用结构体更方便

C++中的联合体：成员共用同一组内存单元、任何两个成员不会同时有效

```c++
#include <iostream>
using namespace std;
class ExamInfo {
private:
	string name;	//课程名称
	enum { GRADE, PASS, PERCENTAGE } mode;//计分方式
	union {
		char grade;	//等级制的成绩
		bool pass;	//只记是否通过课程的成绩
		int percent;	//百分制的成绩
	};
public:
	//三种构造函数，分别用等级、是否通过和百分初始化
	ExamInfo(string name, char grade)
		: name(name), mode(GRADE), grade(grade) { }
	ExamInfo(string name, bool pass)
		: name(name), mode(PASS), pass(pass) { }
	ExamInfo(string name, int percent)
		: name(name), mode(PERCENTAGE), percent(percent) { }
	void show();
}

void ExamInfo::show() {
	cout << name << ": ";
	switch (mode) {
	  case GRADE: cout << grade;  break;
	  case PASS: cout << (pass ? "PASS" : "FAIL"); break;
	  case PERCENTAGE: cout << percent; break;
	}
	cout << endl;
}

int main() {
	ExamInfo course1("English", 'B');
	ExamInfo course2("Calculus", true);
	ExamInfo course3("C++ Programming", 85);
	course1.show();
	course2.show();
	course3.show();
	return 0;
}
运行结果：
English: B
Calculus: PASS
C++ Programming: 85
```

> 在C++中除了类中可以有构造函数和析构函数外，结构体中也可以包含构造函数和析构函数，这是因为结构体和类基本雷同，唯一区别是，类中成员变量默认为私有，而结构体中则为公有。注意，C++中的结构体是可以有析构函数和构造函数，而C则不允许。而共用体，它是不可能有析构函数和构造函数的。结构体变量所占内存长度是各成员占的内存长度之和，每个成员分别占有自己的内存单元。共用体变量所占的内存长度等于最长的成员的长度。即它是一种内存覆盖，也就是说，同一块内存在不同的时刻存储不同的值（可以是不同的类型）。

## 第五章：数据的共享与保护

对象的生存期与可见性：对象的生存期有静态生存期和动态生存期，其中静态生存期与程序的运行期相同，在文件作用域中声明的对象和带static关键字的静态变量具有静态生存期；在块作用域中声明的、没有用static修饰的对象是动态生存期的对象，结束于命名该标识符的作用域结束处。

```c++
#include<iostream>
using namespace std;

int i = 1; // i 为全局变量，具有静态生存期。
void other() {
static int a = 2;
static int b;
// a,b为静态局部变量，具有全局寿命，局部可见。
//只第一次进入函数时被初始化。
int c = 10; // c为局部变量，具有动态生存期，
//每次进入函数时都初始化。
a += 2; i += 32; c += 5;
cout<<"---OTHER---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
b = a;
}

int main() {
static int a;//静态局部变量，有全局寿命，局部可见。
int b = -10; // b, c为局部变量，具有动态生存期。
int c = 0;
cout << "---MAIN---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
c += 8; other();
cout<<"---MAIN---\n";
cout<<" i: "<<i<<" a: "<<a<<" b: "<<b<<" c: "<<c<<endl;
i += 10; other();  
return 0;
}

运行结果：
---MAIN---
i: 1 a: 0 b: -10 c: 0
---OTHER---
i: 33 a: 4 b: 0 c: 15
---MAIN---
i: 33 a: 0 b: -10 c: 8
---OTHER---
i: 75 a: 6 b: 4 c: 15
```

C++中的静态成员函数：静态成员函数主要用于处理该类的静态数据成员，可以直接调用静态成员函数。

> 静态成员函数与普通成员函数的根本区别在于：`普通成员函数有 this 指针，可以访问类中的任意成员；而静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）`。

类的友元：友元是C++提供的一种破坏数据封装和数据隐藏的机制。通过将一个模块声明为另一个模块的友元，一个模块能够引用到另一个模块中本是被隐藏的信息。可以使用友元函数和友元类。

> 为了确保数据的完整性，及数据封装与隐藏的原则，建议尽量不使用或少使用友元。重要的一点是友元是单向的！！！

#### C++的常函数详解：

作为类的成员函数，常函数不能修改任何本类的数据成员除非本类数据成员有“mutable”关键字修饰。const是函数声明的一部分，在函数的实现部分也需要加上const。

```c++
返回值   函数名（ 形参表 ）const  {函数体}
```

> 普通成员函数才有常函数。C++中构造函数，静态成员函数，析构函数，全局成员函数都不能是常成员函数。构造成员函数的用途是对对象初始化，成员函数主要是用来被对象调用的，如果构造函数被设置成const，就不能更改成员变量，失去了其作为构造函数的意义。同理析构函数。`全局成员函数和静态成员函数static其函数体内部没有this指针，所以也不能是常成员函数`

**常函数中的this指针是常指针，不能在常成员函数中修改成员变量的值**

当然，对于一个常成员函数的接口，可通过以下方法仍然可以改变成员变量的值：

- `对this指针做去常转换`

```c++
cout << const_cast< MyClass* >(this)->data++ << endl;
```

- `用关键字mutable修饰成员变量`

```c++
private：
		mutale int data;
```

```c++
#include<iostream>
using namespace std;

class ConstTest{
private:
    const int a = 10;
    int b = 20;
public:
    void print() const;
    ConstTest();
    ConstTest(int b);
    ~ConstTest();

};

ConstTest::ConstTest(){}
ConstTest::ConstTest(int b){
    this->b = b;
}
ConstTest::~ConstTest(){}

void ConstTest::print() const{
    // 越过const函数的语法检查
    const_cast<ConstTest*>(this)->b++;
    cout<<b<<endl;
}

int main(){
    ConstTest t(54);
    t.print();//输出55
    return 0;
}
```

说明几点：

1. `const关键字可以重载函数名相同但是未加const关键字的函数`
2. `非const对象可以调用常函数，也能调用非常函数。但是常对象只能调用常函数，不能调用非常函数（常对象也包括常指针和常引用）`
3. 常成员函数不能用来更新类的`任何`成员变量，也不能调用类中未用const修饰的成员函数，只能调用常成员函数。即常成员函数不能更改类中的成员状态，这与const语义相符。

#### :tada:补充知识点1：C++的顶层const和底层const

`常量指针`

常量指针值的是一个**不能改变指向的的指针**。指针是一个常量，必须初始化，一旦初始化完成，它的值就不能再改变了，即不能在中途改变指向。

```c++
int* const p;
```

`指针常量`

指针常量是一个指针，读成常量的指针，**指向一个只读变量**，也就是后面所指明的int const 和 const int，都是一个常量，可以写作

```
int const *p 
const int *p；
```

`顶层const`

**const修饰的变量本身是一个常量**（常量指针），无法修改，指的是指针，就是*号的右边

`底层const`

**const修饰的变量所指向的对象是一个常量**（指针常量），值的是所指变量，就是*号的左边

```c++
int a = 10;
int * const b1 = &a; // 顶层const，b1本身是一个常量  -- 常量指针
const int *b2 = &a;  // 底层const，b2本身可变，所指的对象是一个常量 -- 指针常量
const int b3 = 20; // 顶层const，b3本身是一个常量
const int* const b4 = &a; // 前一个const为底层const，后一个为顶层const，b4不可变；
const int& b5 = a; // 用于声明引用变量都是底层const
```

`顶层const和底层const的区分作用`

- 执行对象拷贝时有限制，常量的底层const不能赋值给非常量的普通变量

```c++
const int num = 12;
const int *p1 = &num;
// int *p2 = p1; 报错，p1显然是底层const，const规定不能通过p1改变常量的值，如果这句语句正常执行，那么num的值可能会因p2的改变而发生改变，造成程序的崩溃。
const int *p2 = p1; //正确，此时p2也是一个底层const
const int &r1 = *p1; //正确，声明引用的const都是底层const。
```

- `使用命名的强制类型转换函数const_cast时，只能改变运算对象的底层const`

#### :taco:补充知识点2：C++中const的底层原理

```c++
#include <iostream>
using namespace std;

int main(){ 
    const int i = 8;
    int *pt = (int *) &i;
    *pt=5;

    cout<<*pt<<endl;
    cout<<i<<endl;
    return 0;
}
输出内容：
5
8
```

> 在C++中，被const修饰的变量，可能为其分配存储空间,也可能不分配存储空间。有下面两种情况，会为这个变量分配存储空间：
>
> - 当const常量为全局，并且需要在其它文件中使用时（extern）
> - 当使用取地址符（&）取const常量的地址时
>
> C++编译器对const常量的处理是`当碰见常量声明时，C++不会单独的给它分配内存中，而是将它分配在符号表中，除非遇到上述两种情况！！`

如果没有为这个变量分配空间的情况下，这个变量是不可能被改变的，也就是说，这个变量是实实在在的常量。这种情况下，没有任何争议。

```c++
const int i = 8;
```

变量i=8，i放在了符号表里面，这个值永远不会变。当取i的值的时候，就从符号表里面取，永远是8。

```c++
int *pt = (int *) &i;
```

定义一个新的指针变量pt，指向变量i的地址。这时，会在栈上为变量i分配一个新的空间，存放变量i的值。然后用指针pt指向这个空间。例如这个内存空间的地址是0x00FF00AA。

```c++
*pt=5;
```

把内存地址0x00FF00AA空间的值修改为5。

```c++
cout<<*pt<<endl;//从内存0x00FF00AA中取值，等于5.
cout<<i<<endl;//从符号表中取值，等于8.
```

真是干货满满啊~:cherry_blossom::cherry_blossom::cherry_blossom::cherry_blossom::cherry_blossom::cherry_blossom:

------

#### C++中的`编译预处理`：

- \#include 包含指令
  - 将一个源文件嵌入到当前源文件中该点处
  -  \#include<文件名> ：按标准方式搜索，文件位于C++系统目录的include子目录下
  - \#include"文件名"：首先在当前目录中搜索，若没有，再按标准方式搜索
- \#define 宏定义指令
  - 定义符号常量，很多情况下已被const定义语句取代
  - 定义带参数宏，已被内联函数取代
- \#undef
  - 删除由#define定义的宏，使之不再起作用

## 第六章：数组、指针和字符串

在C++中，使用数组的时候，编译器一般会把它`转换成指针`

==数组的本质==：

- 在使用到`数组名`的地方，编译器会自动的将其`替换`为一个`指向数组首元素的指针`

> 所以，在一些情况下，数组的操作实际上是指针的操作

- 指针也是`迭代器`，vector和string支持的运算，数组的指针也全部支持

==数组的特点==：

- `当数组作为一个auto 变量的初始值时，得到的推断是指针，而不是数组`

```c++
int arr1[]={1,2,3};
auto arr2(arr1);	//arr2 是一个整型指针，指向arr1的第一个元素
arr2=4;				//错误，arr2 是指针

//编译器内部实际上发生了以下的转换
auto arr2(&arr[0]);	//所以arr2 是int* 类型

```

- 通过指针也能遍历数组中的元素，我们只需要得到数组第一个元素的指针和尾元素下一个位置的指针

> 在C++ 11 中引入了begin 和 end 函数，其功能与容器中的两个同名成员函数类似

```c++
int arr[]={1,2,3};
int* beg = begin(arr);	//指向arr首元素的指针
int* last = end(arr);	//指向arr尾元素的下一个位置指针

//使用指针遍历数组
while (beg != last) {
	cout << *beg << endl;
	beg++;
}
```

- 给指针加上某个整数，结果仍为指针
- 两个指针相减的结果是他们之间的`距离`。参与运算的两个指针必须指向`同一个数组`当中的元素

> 如果两个指针分别指向不相关的对象，则不能比较他们。

#### C++的智能指针详解：

1、智能指针的简介：

从比较简单的层面来看，智能指针是RAII(Resource Acquisition Is Initialization，资源获取即初始化)机制对普通指针进行的一层封装。`这样使得智能指针的行为动作像一个指针，本质上却是一个对象，这样可以方便管理一个对象的生命周期。`

在C++中，智能指针一共定义了4种：auto_ptr、unique_ptr、shared_ptr 和 weak_ptr。其中，auto_ptr 在 C++11已被摒弃，在C++17中已经移除不可用。

2、原始指针的问题：

原始指针的问题大家都懂，就是如果忘记删除，或者删除的情况没有考虑清楚，容易造成悬挂指针(dangling pointer)或者说野指针(wild pointer)。

```c++
objtype *p = new objtype();
p -> func();
delete p;
```

上面的代码结构是我们经常看到的。里面的问题主要有以下两点：

- 代码的最后，忘记执行delete p的操作。
- 第一点其实还好，比较容易发现也比较容易解决。比较麻烦的是，如果func()中有异常，delete p语句执行不到，这就很难办。有的同学说可以在func中进行删除操作，理论上是可以这么做，实际操作起来，会非常麻烦也非常复杂。

此时，智能指针就可以方便我们控制指针对象的生命周期。在智能指针中，一个对象什么情况下被析构或被删除，是由指针本身决定的，并不需要用户进行手动管理，是不是瞬间觉得幸福感提升了一大截，有点幸福来得太突然的意思，终于不用我自己手动删除指针了。

3、unique_ptr：

unique_ptr是独享被管理对象指针所有权(owership)的智能指针。unique_ptr对象封装一个原始指针，并负责其生命周期。当该对象被销毁时，会在其析构函数中删除关联的原始指针。

```c++
#include <iostream>
#include <string>
#include <memory>
using namespace std;

void f1() {
    unique_ptr<int> p(new int(5));
    cout<<*p<<endl;
}
```

上面的代码就创建了一个unique_ptr。`需要注意的是，unique_ptr没有复制构造函数，不支持普通的拷贝和赋值操作。`因为unique_ptr独享被管理对象指针所有权，当p2, p3失去p的所有权时会释放对应资源，此时会执行两次delete p的操作。

```c++
void f1() {
    unique_ptr<int> p(new int(5));
    cout<<*p<<endl;
    unique_ptr<int> p2(p);
    unique_ptr<int> p3 = p;
}
```

对于p2,p3对应的行，IDE会提示报错

```c++
无法引用 函数 "std::__1::unique_ptr<_Tp, _Dp>::unique_ptr(const std::__1::unique_ptr<int, std::__1::default_delete<int>> &) [其中 _Tp=int, _Dp=std::__1::default_delete<int>]" (已隐式声明) -- 它是已删除的函数
```

unique_ptr虽然不支持普通的拷贝和赋值操作，但却可以将所有权进行转移，使用std::move方法即可。

```c++
void f1() {
    unique_ptr<int> p(new int(5));
    unique_ptr<int> p2 = std::move(p);
    //error，此时p指针为空: cout<<*p<<endl; 
    cout<<*p2<<endl;
}
```

==unique最常见的使用场景，就是替代原始指针，为动态申请的资源提供异常安全保证。==前面我们分析了这部分代码的问题，如果我们修改一下:

```c++
unique_ptr<objtype> p(new objtype());
p -> func();
delete p
```

此时我们只要unique_ptr创建成功，unique_ptr对应的析构函数都能保证被调用，从而保证申请的动态资源能被释放掉。

4、shared_ptr

我们提到的智能指针，很大程度上就是指的shared_ptr，shared_ptr也在实际应用中广泛使用。`它的原理是使用引用计数实现对同一块内存的多个引用。在最后一个引用被释放时，指向的内存才释放，这也是和 unique_ptr 最大的区别。`当对象的所有权需要共享(share)时，share_ptr可以进行赋值拷贝。

> shared_ptr使用引用计数，每一个shared_ptr的拷贝都指向相同的内存。每使用它一次，内部的引用计数加1，每析构一次，内部的引用计数减1，减为0时，释放所指向的堆内存。

```c++
std::shared_ptr<int> p4 = new int(1)
```

上面这种写法是错误的，因为右边得到的是一个原始指针，前面我们讲过shared_ptr本质是一个对象，将一个指针赋值给一个对象是不行的。(shared_ptr的单元素构造函数是eplicit的)

```c++
void f2() {
    shared_ptr<int> p = make_shared<int>(1);
    shared_ptr<int> p2(p);
    shared_ptr<int> p3 = p;
}
```

以上写法都是可以的。

```c++
void f2() {
    shared_ptr<int> p = make_shared<int>(1);
    int *p2 = p.get();
    cout<<*p2<<endl;
}
```

上面的写法，可以获取shared_ptr的原始指针。

5、shared_ptr使用需要注意的点

- `不能将一个原始指针初始化多个shared_ptr`

```c++
void f2() {
    int *p0 = new int(1);
    shared_ptr<int> p1(p0);
    shared_ptr<int> p2(p0);
    cout<<*p1<<endl;
}
```

上面代码就会报错。原因也很简单，`因为p1,p2都要进行析构删除，这样会造成原始指针p0被删除两次，自然要报错。`

- 循环引用问题

```c++
struct Father
{
    shared_ptr<Son> son_;
};

struct Son
{
    shared_ptr<Father> father_;
};

int main()
{
    auto father = make_shared<Father>();
    auto son = make_shared<Son>();

    father->son_ = son;
    son->father_ = father;

    return 0;
}
```

该部分代码会有内存泄漏问题。原因是
1.main 函数退出之前，Father 和 Son 对象的引用计数都是 2。
2.son 指针销毁，这时 Son 对象的引用计数是 1。
3.father 指针销毁，这时 Father 对象的引用计数是 1。
4.由于 Father 对象和 Son 对象的引用计数都是 1，这两个对象都不会被销毁，从而发生内存泄露。

> 为避免循环引用导致的内存泄露，就需要使用 weak_ptr。weak_ptr 并不拥有其指向的对象，也就是说，让 weak_ptr 指向 shared_ptr 所指向对象，对象的引用计数并不会增加。使用 weak_ptr 就能解决前面提到的循环引用的问题，方法很简单，只要让 Son 或者 Father 包含的 shared_ptr 改成 weak_ptr 就可以了。

```c++
struct Father
{
    shared_ptr<Son> son_;
};

struct Son
{
    weak_ptr<Father> father_;
};

int main()
{
    auto father = make_shared<Father>();
    auto son = make_shared<Son>();

    father->son_ = son;
    son->father_ = father;

    return 0;
}
```

1.main 函数退出前，Son 对象的引用计数是 2，而 Father 的引用计数是 1。
2.son 指针销毁，Son 对象的引用计数变成 1。
3.Father 指针销毁，Father 对象的引用计数变成 0，导致 Father 对象析构，Father 对象的析构会导致它包含的 son_ 指针被销毁，这时 Son 对象的引用计数变成 0，所以 Son 对象也会被析构。

## 第七章：继承与派生

#### :cake: 三种继承方式的区别

类成员的访问权限由高到低依次为 public --> protected --> private，public 成员可以通过对象来访问，private 成员不能通过对象访问，protected 成员和 private 成员类似，也不能通过对象访问。但是当存在继承关系时，protected 和 private 就不一样了：基类中的 protected 成员可以在派生类中使用，而基类中的 private 成员不能在派生类中使用。

`不同的继承方式会影响基类成员在派生类中的访问权限。`

**1) public继承方式**

- 基类中所有 public 成员在派生类中为 public 属性；
- 基类中所有 protected 成员在派生类中为 protected 属性；
- 基类中所有 private 成员在派生类中不能使用。

**2) protected继承方式**

- 基类中的所有 public 成员在派生类中为 protected 属性；
- 基类中的所有 protected 成员在派生类中为 protected 属性；
- 基类中的所有 private 成员在派生类中不能使用。

**3) private继承方式**

- 基类中的所有 public 成员在派生类中均为 private 属性；
- 基类中的所有 protected 成员在派生类中均为 private 属性；
- 基类中的所有 private 成员在派生类中不能使用。

通过上面的分析可以发现：

1)`基类成员在派生类中的访问权限不得高于继承方式中指定的权限`。例如，当继承方式为 protected 时，那么基类成员在派生类中的访问权限最高也为 protected，高于 protected 的会降级为 protected，但低于 protected 不会升级。再如，当继承方式为 public 时，那么基类成员在派生类中的访问权限将保持不变。

> 也就是说，继承方式中的 public、protected、private 是用来指明基类成员在派生类中的最高访问权限的。

2)`不管继承方式如何，基类中的 private 成员在派生类中始终不能使用`（不能在派生类的成员函数中访问或调用）。

3)如果希望基类的成员能够被派生类继承并且毫无障碍地使用，那么这些成员只能声明为 public 或 protected；只有那些不希望在派生类中使用的成员才声明为 private。

4)`如果希望基类的成员既不向外暴露（不能通过对象访问），还能在派生类中使用，那么只能声明为 protected`。

> 注意，我们这里说的是基类的 private 成员不能在派生类中使用，并没有说基类的 private 成员不能被继承。`实际上，基类的 private 成员是能够被继承的，并且（成员变量）会占用派生类对象的内存，它只是在派生类中不可见，导致无法使用罢了。`private 成员的这种特性，能够很好的对派生类隐藏基类的实现，以体现面向对象的封装性。

| 继承方式/基类成员 | public成员 | protected成员 | private成员 |
| ----------------- | ---------- | ------------- | ----------- |
| public继承        | public     | protected     | 不可见      |
| protected继承     | protected  | protected     | 不可见      |
| private继承       | private    | private       | 不可见      |

==由于 private 和 protected 继承方式会改变基类成员在派生类中的访问权限，导致继承关系复杂，所以实际开发中我们一般使用 public。==

```c++
#include<iostream>
using namespace std;

//基类People
class People{
public:
    void setname(char *name);
    void setage(int age);
    void sethobby(char *hobby);
    char *gethobby();
protected:
    char *m_name;
    int m_age;
private:
    char *m_hobby;
};
void People::setname(char *name){ m_name = name; }
void People::setage(int age){ m_age = age; }
void People::sethobby(char *hobby){ m_hobby = hobby; }
char *People::gethobby(){ return m_hobby; }

//派生类Student
class Student: public People{
public:
    void setscore(float score);
protected:
    float m_score;
};
void Student::setscore(float score){ m_score = score; }

//派生类Pupil
class Pupil: public Student{
public:
    void setranking(int ranking);
    void display();
private:
    int m_ranking;
};
void Pupil::setranking(int ranking){ m_ranking = ranking; }
void Pupil::display(){
    cout<<m_name<<"的年龄是"<<m_age<<"，考试成绩为"<<m_score<<"分，班级排名第"<<m_ranking<<"，TA喜欢"<<gethobby()<<"。"<<endl;
}

int main(){
    Pupil pup;
    pup.setname("小明");
    pup.setage(15);
    pup.setscore(92.5f);
    pup.setranking(4);
    pup.sethobby("乒乓球");
    pup.display();

    return 0;
}
```

这是一个多级继承的例子，Student 继承自 People，Pupil 又继承自 Student，它们的继承关系为 People --> Student --> Pupil。Pupil 是最终的派生类，它拥有基类的 m_name、m_age、m_score、m_hobby 成员变量以及 setname()、setage()、sethobby()、gethobby()、setscore() 成员函数。

注意，在派生类 Pupil 的成员函数 display() 中，我们借助基类的 public 成员函数 gethobby() 来访问基类的 private 成员变量 m_hobby，因为 m_hobby 是 private 属性的，在派生类中不可见，所以只能借助基类的 public 成员函数 sethobby()、gethobby() 来访问。

> 在派生类中访问基类 private 成员的唯一方法就是借助基类的非 private 成员函数，如果基类没有非 private 成员函数，那么该成员在派生类中将无法访问。

:tanabata_tree:改变访问权限

使用 using 关键字可以改变基类成员在派生类中的访问权限，例如将 public 改为 private、将 protected 改为 public。

> 注意：using 只能改变基类中 public 和 protected 成员的访问权限，不能改变 private 成员的访问权限，因为基类中 private 成员在派生类中是不可见的，根本不能使用，所以基类中的 private 成员在派生类中无论如何都不能访问。

```c++
#include<iostream>
using namespace std;
//基类People
class People {
public:
    void show();
protected:
    char *m_name;
    int m_age;
};
void People::show() {
    cout << m_name << "的年龄是" << m_age << endl;
}
//派生类Student
class Student : public People {
public:
    void learning();
public:
    using People::m_name;  //将protected改为public
    using People::m_age;  //将protected改为public
    float m_score;
private:
    using People::show;  //将public改为private
};
void Student::learning() {
    cout << "我是" << m_name << "，今年" << m_age << "岁，这次考了" << m_score << "分！" << endl;
}
int main() {
    Student stu;
    stu.m_name = "小明";
    stu.m_age = 16;
    stu.m_score = 99.5f;
    stu.show();  //compile error
    stu.learning();
    return 0;
}
```

代码中首先定义了基类 People，它包含两个 protected 属性的成员变量和一个 public 属性的成员函数。定义 Student 类时采用 public 继承方式，People 类中的成员在 Student 类中的访问权限默认是不变的。

不过，我们使用 using 改变了它们的默认访问权限，如代码第 21~25 行所示，将 show() 函数修改为 private 属性的，是降低访问权限，将 name、age 变量修改为 public 属性的，是提高访问权限。

#### :gear:C++中派生类的构造与析构

默认情况下基类的构造函数不被继承，派生类需要定义自己的构造函数，C++11标准允许使用，using语句继承基类构造函数，但是只能初始化从基类继承的成员

> 建议：如果派生类有自己新增的成员，且需要通过构造函数初始化，则派生类要自定义构造函数。

1）若不继承基类的构造函数

- 派生类新增成员：派生类定义构造函数初始化；
- 继承来的成员：自动调用基类构造函数进行初始化；
- 派生类的构造函数需要给基类的构造函数传递参数。

2）单继承

- 派生类只有一个直接基类的情况，是单继承。单继承时，派生类的构造函数只需要给一个直接基类构造函数传递参数。

单继承时构造函数的定义语法：

```c++
派生类名::派生类名(基类所需的形参，本类成员所需的形参):
基类名(参数表), 本类成员初始化列表
{
	//其他初始化；
}；
```

```c++
#include<iostream>
using namespace std;
class B {
public:
    B();
    B(int i);
    ~B();
    void print() const;
private:
    int b;
};

B::B() {
    b=0;
    cout << "B's default constructor called." << endl;
}
B::B(int i) {
    b=i;
    cout << "B's constructor called." << endl;
}
B::~B() {
    cout << "B's destructor called." << endl;
}
void B::print() const {
    cout << b << endl;
}

class C: public B {
public:
    C();
    C(int i, int j);
    ~C();
    void print() const;
private:
    int c;
};
C::C() {
    c = 0;
    cout << "C's default constructor called." << endl;
}
C::C(int i,int j): B(i), c(j){
    cout << "C's constructor called." << endl;
}

C::~C() {
    cout << "C's destructor called." << endl;
}
void C::print() const {
    B::print();
    cout << c << endl;
}

int main() {
    C obj(5, 6);
    obj.print();
    return 0;
}
```

3）多继承

- 多继承时，有多个直接基类，如果不继承基类的构造函数，派生类构造函数需要给所有基类构造函数传递参数。

多继承时构造函数的定义语法：

```c++
派生类名::派生类名(参数表) : 
基类名1(基类1初始化参数表), 
基类名2(基类2初始化参数表), 
...
基类名n(基类n初始化参数表), 
本类成员初始化列表
{
        //其他初始化；
}；
```

> 注：当基类有默认构造函数时，派生类构造函数可以不向基类构造函数传递参数，构造派生类的对象时，基类的默认构造函数将被调用。

多继承且有对象成员时派生的构造函数定义语法：

```c++
派生类名::派生类名(形参表):
基类名1(参数), 基类名2(参数), ..., 基类名n(参数), 
本类成员（含对象成员）初始化列表
{
        //其他初始化
}；
```

:label:构造函数的执行顺序:

1. `调用基类构造函数`：顺序按照它们被继承时声明的顺序（从左向右）。
2. `对初始化列表中的成员进行初始化`：顺序按照它们在类中定义的顺序，对象成员初始化时自动调用其所属类的构造函数。由初始化列表提供参数。
3. `执行派生类的构造函数体中的内容。`

#### :ocean: 派生类的复制构造函数:

**派生类未定义复制构造函数的情况:**

- 编译器会在需要时生成一个隐含的复制构造函数；
- 先调用基类的复制构造函数；
- 再为派生类新增的成员执行复制。

**派生类定义了复制构造函数的情况:**

- 一般都要为基类的复制构造函数传递参数。
- 复制构造函数只能接受一个参数，既用来初始化派生类定义的成员，也将被传递给基类的复制构造函数。

- 基类的复制构造函数形参类型是基类对象的引用，实参可以是派生类对象的引用

`有关二义性问题`:

当派生类与基类中有相同成员时：

- `若未特别限定，则通过派生类对象使用的是派生类中的同名成员。`
- `如要通过派生类对象访问基类中被隐藏的同名成员，应使用基类名和作用域操作符（::）来限定。`

```c++
#include <iostream>
using namespace std;
class Base1 {   
public:
    int var;
    void fun() { cout << "Member of Base1" << endl; }
};
class Base2 {   
public:
    int var;
    void fun() { cout << "Member of Base2" << endl; }
};
class Derived: public Base1, public Base2 {
public:
    int var;    
    void fun() { cout << "Member of Derived" << endl; }
};

int main() {
    Derived d;
    Derived *p = &d;

  //访问Derived类成员
    d.var = 1;
    d.fun();    

    //访问Base1基类成员
    d.Base1::var = 2;
    d.Base1::fun(); 

    //访问Base2基类成员
    p->Base2::var = 3;
    p->Base2::fun();    

    return 0;
}
```

- 如果从不同基类继承了同名成员，但是在派生类中没有定义同名成员，“派生类对象名或引用名.成员名”、“派生类指针->成员名”访问成员存在二义性问题：用类名限定。

```c++
class A {
public:
    void  f();
};
class B {
public:
    void f();
    void g()
};
class C: public A, piblic B {
public:
    void g();
    void h();
};
```

```shell
如果定义：C  c1;
则 c1.f() 具有二义性
而 c1.g() 无二义性（同名隐藏）
例7-7  多继承时的二义性和冗余问题
```

```c++
#include <iostream>
using namespace std;
class Base0 {   //定义基类Base0
public:
    int var0;
    void fun0() { cout << "Member of Base0" << endl; }
};
class Base1: public Base0 { //定义派生类Base1 
public: //新增外部接口
    int var1;
};
class Base2: public Base0 { //定义派生类Base2 
public: //新增外部接口
    int var2;
};

// 多继承时的二义性和冗余问题
class Derived: public Base1, public Base2 {
public: 
    int var;
    void fun() 
  { cout << "Member of Derived" << endl; }
};

int main() {    //程序主函数
    Derived d;
    d.Base1::var0 = 2;
    d.Base1::fun0();
    d.Base2::var0 = 3;
    d.Base2::fun0();
    return 0;
}
```

![image-20221106145921024](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221106145921024.png)

> 问题：当派生类从多个基类派生，而这些基类又共同基类，则在访问此共同基类中的成员时，将产生冗余，并有可能因冗余带来不一致性!!

解决方案：`使用虚基类！`主要用来解决多继承时可能发生的对同一基类继承多次而产生的二义性问题

注意：在`第一级继承`时就要将`共同基类`设计为虚基类。

![image-20221106150156736](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221106150156736.png)

```c++
#include <iostream>
using namespace std;
class Base0 {
public:
    int var0;
    void fun0() { cout << "Member of Base0" << endl; }
};
class Base1: virtual public Base0 {
public: 
    int var1;
};
class Base2: virtual public Base0 {
public: 
    int var2;
};


class Derived: public Base1, public Base2 {
//定义派生类Derived 
public: 
    int var;
    void fun() {
        cout << "Member of Derived" << endl;
    }
};

int main() {    
    Derived d;
    d.var0 = 2;   //直接访问虚基类的数据成员
    d.fun0();     //直接访问虚基类的函数成员
    return 0;
}
```

**虚基类及其派生类构造函数:**

- 建立对象时所指定的类称为**最远派生类**。
- 虚基类的成员是由最远派生类的构造函数通过调用虚基类的构造函数进行初始化的。

- 在整个继承结构中，直接或间接继承虚基类的所有派生类，都必须在构造函数的成员初始化表中为虚基类的构造函数列出参数。如果未列出，则表示调用该虚基类的默认构造函数。
- 在建立对象时，只有最远派生类的构造函数调用虚基类的构造函数，其他类对虚基类构造函数的调用被忽略。

> 析构函数与构造函数的调用顺序相反

## 第八章：多态性

:ghost: 运算符重载规则：

1）双目运算符重载为成员函数

```c++
 函数类型  operator 运算符（形参）
    {
           ......
    }
    参数个数=原操作数个数-1   （后置++、--除外）
```

- 如果要重载 B 为类成员函数，使之能够实现表达式 oprd1 B oprd2，其中 oprd1 为A 类对象，则 B 应被重载为 A 类的成员函数，形参类型应该是 oprd2 所属的类型。
- 经重载后，表达式 oprd1 B oprd2 相当于 oprd1.operator B(oprd2)

案例：复数类加减法运算重载为成员函数

```c++
#include <iostream>
using namespace std;
class Complex {
public:
    Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { }
    //运算符+重载成员函数
    Complex operator + (const Complex &c2) const;
    //运算符-重载成员函数
    Complex operator - (const Complex &c2) const;
    void display() const;   //输出复数
private:
    double real;    //复数实部
    double imag;    //复数虚部
};
//复数类加减法运算重载为成员函数
Complex Complex::operator+(const Complex &c2) const{
  //创建一个临时无名对象作为返回值 
  return Complex(real+c2.real, imag+c2.imag); 
}

Complex Complex::operator-(const Complex &c2) const{
 //创建一个临时无名对象作为返回值
    return Complex(real-c2.real, imag-c2.imag); 
}

void Complex::display() const {
    cout<<"("<<real<<", "<<imag<<")"<<endl;
}
//复数类加减法运算重载为成员函数
int main() {
    Complex c1(5, 4), c2(2, 10), c3;
    cout << "c1 = "; c1.display();
    cout << "c2 = "; c2.display();
    c3 = c1 - c2;   //使用重载运算符完成复数减法
    cout << "c3 = c1 - c2 = "; c3.display();
    c3 = c1 + c2;   //使用重载运算符完成复数加法
    cout << "c3 = c1 + c2 = "; c3.display();
    return 0;
}
```

2）前置单目运算符重载为成员函数

- 如果要重载 U 为类成员函数，使之能够实现表达式 U oprd，其中 oprd 为A类对象，则 U 应被重载为 A 类的成员函数，无形参。
- 经重载后，表达式 U oprd 相当于 oprd.operator U()

3）后置单目运算符 ++和--重载为成员函数

- 如果要重载 ++或--为类成员函数，使之能够实现表达式 oprd++ 或 oprd-- ，其中 oprd 为A类对象，则 ++或-- 应被重载为 A 类的成员函数，且具有一个 int 类型形参。
- 经重载后，表达式 oprd++ 相当于 oprd.operator ++(0)

案例：重载前置++和后置++为时钟类成员函数

```c++
#include <iostream>
using namespace std;
class Clock {//时钟类定义
public: 
    Clock(int hour = 0, int minute = 0, int second = 0);
    void showTime() const;
  	//前置单目运算符重载
    Clock& operator ++ ();
  	//后置单目运算符重载
    Clock operator ++ (int);    
private:
    int hour, minute, second;
};

Clock::Clock(int hour, int minute, int second) {    
    if (0 <= hour && hour < 24 && 0 <= minute && minute < 60
        && 0 <= second && second < 60) {
        this->hour = hour;
        this->minute = minute;
        this->second = second;
    } else
        cout << "Time error!" << endl;
}
void Clock::showTime() const {  //显示时间
    cout << hour << ":" << minute << ":" << second << endl;
}

//重载前置++和后置++为时钟类成员函数
Clock & Clock::operator ++ () { 
    second++;
    if (second >= 60) {
        second -= 60;  minute++;
        if (minute >= 60) {
          minute -= 60; hour = (hour + 1) % 24;
        }
    }
    return *this;
}

Clock Clock::operator ++ (int) {
    //注意形参表中的整型参数
    Clock old = *this;
    ++(*this);  //调用前置“++”运算符
    return old;
}
//重载前置++和后置++为时钟类成员函数
int main() {
    Clock myClock(23, 59, 59);
    cout << "First time output: ";
    myClock.showTime();
    cout << "Show myClock++:    ";
    (myClock++).showTime();
    cout << "Show ++myClock:    ";
    (++myClock).showTime();
    return 0;
}
```

4）运算符重载为非成员函数

> 有些运算符不能重载为成员函数，例如二元运算符的左操作数不是对象，或者是不能由我们重载运算符的对象

- 函数的形参代表依自左至右次序排列的各操作数。
- 参数个数=原操作数个数（后置++、--除外）
- 至少应该有一个自定义类型的参数。
- 后置单目运算符 ++和--的重载函数，形参列表中要增加一个int，但不必写形参名。
- 如果在运算符的重载函数中需要操作某类对象的私有成员，可以将此函数声明为该类的友元。

> 双目运算符 B重载后：表达式oprd1 B oprd2等同于operator B(oprd1,oprd2 )
>
> 前置单目运算符 B重载后：表达式 B oprd等同于operator B(oprd )
>
> 后置单目运算符 ++和--重载后：表达式 oprd B等同于operator B(oprd,0 )

案例：重载Complex的加减法和“<<”运算符为非成员函数

`注意`：将+、-（双目）重载为非成员函数，并将其声明为复数类的友元，两个操作数都是复数类的常引用。 将<<（双目）重载为非成员函数，并将其声明为复数类的友元，它的左操作数是std::ostream引用，右操作数为复数类的常引用，返回std::ostream引用，用以支持下面形式的输出：

```c++
cout << a << b;
```

该输出调用的是：

```c++
operator << (operator << (cout, a), b);
```

```c++
#include <iostream>
using namespace std;

class Complex {
    public:
    Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { }  
    friend Complex operator+(const Complex &c1, const Complex &c2);
    friend Complex operator-(const Complex &c1, const Complex &c2);
    friend ostream & operator<<(ostream &out, const Complex &c);
    private:    
    double real;  //复数实部
    double imag;  //复数虚部
};

Complex operator+(const Complex &c1, const Complex &c2){
    return Complex(c1.real+c2.real, c1.imag+c2.imag); 
}
Complex operator-(const Complex &c1, const Complex &c2){
    return Complex(c1.real-c2.real, c1.imag-c2.imag); 
}

ostream & operator<<(ostream &out, const Complex &c){
    out << "(" << c.real << ", " << c.imag << ")";
    return out;
}

int main() {    
    Complex c1(5, 4), c2(2, 10), c3;    
    cout << "c1 = " << c1 << endl;
    cout << "c2 = " << c2 << endl;
    c3 = c1 - c2;   //使用重载运算符完成复数减法
    cout << "c3 = c1 - c2 = " << c3 << endl;
    c3 = c1 + c2;   //使用重载运算符完成复数加法
    cout << "c3 = c1 + c2 = " << c3 << endl;
    return 0;
}
```

#### :deer: 虚函数解析

- 用virtual关键字说明的函数
- 虚函数是实现运行时多态性基础
- C++中的虚函数是`动态绑定`的函数
- 虚函数必须是`非静态的成员函数`，虚函数经过派生之后，就可以实现运行过程中的多态
- 一般成员函数可以是虚函数
- 构造函数不能是虚函数
- 析构函数可以是虚函数
- `虚函数声明只能出现在类定义中的函数原型声明中，而不能在成员函数实现的时候`
- ==虚函数一般不声明为内联函数，因为对虚函数的调用需要动态绑定，而对内联函数的处理是静态的==

```c++
#include <iostream>
using namespace std;

class Base1 {
public:
    virtual void display() const;  //虚函数
};
void Base1::display() const {
    cout << "Base1::display()" << endl;
}

class Base2::public Base1 { 
public:
     virtual void display() const;
};
void Base2::display() const {
    cout << "Base2::display()" << endl;
}
class Derived: public Base2 {
public:
     virtual void display() const; 
};
void Derived::display() const {
    cout << "Derived::display()" << endl;
}

void fun(Base1 *ptr) { 
    ptr->display(); 
}

int main() {    
    Base1 base1;
    Base2 base2;
    Derived derived;    
    fun(&base1);
    fun(&base2);
    fun(&derived);
    return 0;
}
```

`虚析构函数`

为什么需要虚析构函数？ - 可能通过基类指针删除派生类对象； - 如果你打算允许其他人通过基类指针调用对象的析构函数（通过delete这样做是正常的），就需要让基类的析构函数成为虚函数，否则执行delete的结果是不确定的。

```c++
#include <iostream>
using namespace std; 
class Base {
public:
    ~Base(); //不是虚函数
};
Base::~Base() {
    cout<< "Base destructor" << endl;
 }
class Derived: public Base{
public:
    Derived();
    ~Derived(); //不是虚函数
private:
    int *p;
};

#include <iostream>
using namespace std;
class Base {
public:
    virtual ~Base();
};
Base::~Base() {
    cout<< "Base destructor" << endl;
 }
class Derived: public Base{
public:
    Derived();
    virtual ~Derived();
private:
    int *p;
};
```

#### :zap:虚表与动态绑定

虚表：`每个多态类有一个虚表（virtual table），虚表中有当前类的各个虚函数的入口地址，每个对象有一个指向当前类的虚表的指针（虚指针vptr）。`

动态绑定的实现：`构造函数中为对象的虚指针赋值，通过多态类型的指针或引用调用成员函数时，通过虚指针找到虚表，进而找到所调用的虚函数的入口地址，通过该入口地址调用虚函数。`

![8-1.png](http://sc0.ykt.io/ue_i/20200305/1235400306867703808.png)

纯虚函数

- 纯虚函数是一个在基类中声明的虚函数，它在该基类中没有定义具体的操作内容，要求各派生类根据实际需要定义自己的版本，纯虚函数的声明格式为：

  virtual 函数类型 函数名(参数表) = 0;

- `带有纯虚函数的类称为抽象类`

> 抽象类为抽象和设计的目的而声明，将有关的数据和行为组织在一个继承层次结构中，保证派生类具有要求的行为，对于暂时无法实现的函数，可以声明为纯虚函数，留给派生类去实现。

`注意：不能定义抽象类的对象。`

```c++
#include <iostream>
using namespace std;

class Base1 { 
public:
    virtual void display() const = 0;   //纯虚函数
};

class Base2: public Base1 { 
public:
    virtual void display() const; //覆盖基类的虚函数
};
void Base2::display() const {
    cout << "Base2::display()" << endl;
}

class Derived: public Base2 { 
public:
     virtual void display() const; //覆盖基类的虚函数
};
void Derived::display() const {
    cout << "Derived::display()" << endl;
} 
void fun(Base1 *ptr) { 
    ptr->display(); 
}
int main() {    
    Base2 base2;    
    Derived derived;    
    fun(&base2);    
    fun(&derived);  
    return 0;
}
```

#### C++11新标准：override与final

**override**

- 多态行为的基础：基类声明虚函数，继承类声明一个函数覆盖该虚函数
- 覆盖要求： 函数签名（signatture）完全一致
- 函数签名包括：函数名 参数列表 const

下列程序就仅仅因为疏忽漏写了const，导致多态行为没有如期进行：

![8-2.png](http://sc0.ykt.io/ue_i/20200305/1235404318748839936.png)

显式函数覆盖

- C++11 引入显式函数覆盖，在编译期而非运行期捕获此类错误
- 在虚函数显式重载中运用，编译器会检查基类是否存在一虚函数，与派生类中带有声明override的虚函数有相同的函数签名（signature）；若不存在，则会回报错误。

final：C++11提供的final，用来避免类被继承，或是基类的函数被改写。

```c++
struct Derived1 : Base1 { }; // 编译错误：Base1为final，不允许被继承
struct Base2 { virtual void f() final; };
struct Derived2 : Base2 { void f(); };// 编译错误：Base2::f 为final，不允许被覆盖 
```

`虚函数与虚基类不是一个概念，虚基类是为了消除类成员标识的二义性和信息冗余，虚函数是实现动态多态性的基础`

## 第九章：泛型程序设计与C++标准模板库

**什么是泛型程序设计？**

- 编写不依赖于具体数据类型的程序
- 将算法从特定的数据结构中抽象出来，成为通用的
- C++的模板为泛型程序设计奠定了关键的基础

**术语：概念**

- 用来界定具备一定功能的数据类型。例如：
  - 将“可以比大小的所有数据类型（有比较运算符）”这一概念记为**Comparable**
  - 将“具有公有的复制构造函数并可以用‘=’赋值的数据类型”这一概念记为**Assignable**
  - 将“可以比大小、具有公有的复制构造函数并可以用‘=’赋值的所有数据类型”这个概念记作**Sortable**
- 对于两个不同的概念A和B，如果概念A所需求的所有功能也是概念B所需求的功能，那么就说概念B是概念A的子概念。例如：
  - Sortable既是Comparable的子概念，也是Assignable的子概念

**术语：模型**

- 模型（model）：符合一个概念的数据类型称为该概念的模型，例如：
  - int型是Comparable概念的模型。
  - 静态数组类型不是Assignable概念的模型（无法用“=”给整个静态数组赋值）

**用概念做模板参数名**

- 很多STL的实现代码就是使用概念来命名模板参数的。
- 为概念赋予一个名称，并使用该名称作为模板参数名。

表示insertionSort这样一个函数模板的原型：

```c++
template <class Sortable>
void insertionSort(Sortable a[], int n);
```

**STL简介**

- 标准模板库（Standard Template Library，简称STL）定义了一套概念体系，为泛型程序设计提供了逻辑基础
- STL中的各个类模板、函数模板的参数都是用这个体系中的概念来规定的。
- 使用STL的模板时，类型参数既可以是C++标准库中已有的类型，也可以是自定义的类型——只要这些类型是所要求概念的模型。

**STL的基本组件**

- 容器（container）
- 迭代器（iterator）
- 函数对象（function object）
- 算法（algorithms）

**STL的基本组件间的关系**

- Iterators（迭代器）是算法和容器的桥梁。
  - 将迭代器作为算法的参数、通过迭代器来访问容器而不是把容器直接作为算法的参数。

- 将**函数对象**作为算法的参数而不是将函数所执行的运算作为算法的一部分。
- 使用STL中提供的或自定义的迭代器和函数对象，配合STL的算法，可以组合出各种各样的功能。

![10-1.png](http://sc0.ykt.io/ue_i/20200308/1236659027212111872.png)

```c++
template <class InputIterator, class OutputIterator, class UnaryFunction>
OutputIterator transform(InputIterator first, InputIterator last, OutputIterator result, UnaryFunction op) {
    for (;first != last; ++first, ++result)
        *result = op(*first);
    return result;
}
```

:airplane: 迭代器

输入流迭代器和输出流迭代器:

- 输入流迭代器:istream_iterator<T>
  - 以输入流（如cin）为参数构造
  - 可用*(p++)获得下一个输入的元素

- 输出流迭代器:ostream_iterator<T>
  - 构造时需要提供输出流（如cout）
  - 可用(*p++) = x将x输出到输出流

```c++
#include <iterator>
#include <iostream>
#include <algorithm>
using namespace std;

//求平方的函数
double square(double x) {
    return x * x;
}
int main() {
    //从标准输入读入若干个实数，分别将它们的平方输出
    transform(istream_iterator<double>(cin), istream_iterator<double>(),
        ostream_iterator<double>(cout, "\t"), square);
    cout << endl;
    return 0;
}
```

![10-2.png](http://sc0.ykt.io/ue_i/20200308/1236659800931176448.png)

迭代器支持的操作:

- 迭代器是泛化的指针，提供了类似指针的操作（诸如++、*、->运算符）

- 输入迭代器

- - 可以用来从序列中读取数据，如输入流迭代器

- 输出迭代器

- - 允许向序列中写入数据，如输出流迭代器

- 前向迭代器

- - 既是输入迭代器又是输出迭代器，并且可以对序列进行单向的遍历

- 双向迭代器

- - 与前向迭代器相似，但是在两个方向上都可以对数据遍历

- 随机访问迭代器

- - 也是双向迭代器，但能够在序列中的任意两个位置之间进行跳转，如指针、使用vector的begin()、end()函数得到的迭代器

```c++
#include <algorithm>
#include <iterator>
#include <vector>
#include <iostream>
using namespace std;

//将来自输入迭代器的n个T类型的数值排序，将结果通过输出迭代器result输出
template <class T, class InputIterator, class OutputIterator>
void mySort(InputIterator first, InputIterator last, OutputIterator result) {
    //通过输入迭代器将输入数据存入向量容器s中
    vector<T> s;
    for (;first != last; ++first)
        s.push_back(*first);
    //对s进行排序，sort函数的参数必须是随机访问迭代器
    sort(s.begin(), s.end());  
    copy(s.begin(), s.end(), result);   //将s序列通过输出迭代器输出
}

int main() {
    //将s数组的内容排序后输出
    double a[5] = { 1.2, 2.4, 0.8, 3.3, 3.2 };
    mySort<double>(a, a + 5, ostream_iterator<double>(cout, " "));
    cout << endl;
    //从标准输入读入若干个整数，将排序后的结果输出
    mySort<int>(istream_iterator<int>(cin), istream_iterator<int>(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}
/*
运行结果：
0.8 1.2 2.4 3.2 3.3
2 -4 5 8 -1 3 6 -5
-5 -4 -1 2 3 5 6 8
*/
```

> 这部分内容先留着，等后面看《STL源码剖析》再详细说明

## 第十章：流类库与输入输出

程序与外界环境的信息交换：当程序与外界环境进行信息交换时，存在着两个对象：程序中的对象、文件对象。 

流：一种抽象，负责在数据的生产者和数据的消费者之间建立联系，并管理数据的流动。

流对象与文件操作：

- 程序建立一个流对象
- 指定这个流对象与某个文件对象建立连接
- 程序操作流对象
- 流对象通过文件系统对所连接的文件对象产生作用。

流类库结构：

![image-20221108093937068](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108093937068.png)

流类列表：

![image-20221108093959480](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108093959480.png)

**输出流概述**

最重要的三个输出流：

- ostream
- ofstream
- ostringstream

预先定义的输出流对象：

- cout 标准输出
- cerr 标准错误输出，没有缓冲，发送给它的内容立即被输出。
- clog 类似于cerr，但是有缓冲，缓冲区满时被输出。

标准输出换向：

```c++
ofstream fout("b.out");
streambuf* pOld =cout.rdbuf(fout.rdbuf());
//…
cout.rdbuf(pOld); 
```

构造输出流对象：

- ofstream类支持磁盘文件输出
- 如果在构造函数中指定一个文件名，当构造这个对象时该文件是自动打开的 

```c++
ofstream myFile("filename");
```

- 可以在调用默认构造函数之后使用open成员函数打开文件

```c++
ofstream myFile; //声明一个静态文件输出流对象
myFile.open("filename"); //打开文件，使流对象与文件建立联系
```

- 在构造对象或用open打开文件时可以指定模式

```c++
ofstream myFile("filename", ios_base::out | ios_base::binary);
```

文件输出流成员函数的三种类型:

- 与操纵符等价的成员函数。
- 执行非格式化写操作的成员函数。
- 其它修改流状态且不同于操纵符或插入运算符的成员函数。

文件输出流成员函数:

- open函数：把流与一个特定的磁盘文件关联起来。 需要指定打开模式。
- put函数：把一个字符写到输出流中。
- write函数：把内存中的一块内容写到一个文件输出流中。
- seekp和tellp函数：操作文件流的内部指针。
- close函数：关闭与一个文件输出流关联的磁盘文件。
- 错误处理函数

**输入流概述**

重要的输入流类：

- istream类最适合用于顺序文本模式输入。cin是其实例。
- ifstream类支持磁盘文件输入。
- istringstream

构造输入流对象：

- 如果在构造函数中指定一个文件名，在构造该对象时该文件便自动打开。

```c++
ifstream myFile("filename");
```

- 在调用默认构造函数之后使用open函数来打开文件。

```c++
ifstream myFile; //建立一个文件流对象
myFile.open("filename"); //打开文件"filename”
```

- 打开文件时可以指定模式

```c++
ifstream myFile("filename", ios_base::in | ios_base::binary);
```

使用提取运算符从文本文件输入：

- 提取运算符(>>)对于所有标准C++数据类型都是预先设计好的。
- 是从一个输入流对象获取字节最容易的方法。
- ios类中的很多操纵符都可以应用于输入流。但是只有少数几个对输入流对象具有 实际影响，其中最重要的是进制操纵符dec、oct和hex。

输入流相关函数：

- open 把该流与一个特定磁盘文件相关联。
- get 功能与提取运算符（>>）很相像，主要的不同点是get函数在读入数据时包括空白字符。
- getline 功能是从输入流中读取多个字符，并且允许指定输入终止字符，读取完成后，从读取的内容中删除终止字符。
- read 从一个文件读字节到一个指定的内存区域，由长度参数确定要读的字节数。 当遇到文件结束或者在文本模式文件中遇到文件结束标记字符时结束读取。
- seekg 用来设置文件输入流中读取数据位置的指针。
- tellg 返回当前文件读指针的位置。
- close 关闭与一个文件输入流关联的磁盘文件。

**输入/输出流**

两个重要的输入/输出流：

- 一个iostream对象可以是数据的源或目的。
- 两个重要的I/O流类都是从iostream派生的，它们是fstream和stringstream。这些类继承了前面描述的istream和ostream类的功能。

fstream 类：

- fstream类支持磁盘文件输入和输出。
- 如果需要在同一个程序中从一个特定磁盘文件读并写到该磁盘文件，可以构造一个 fstream对象。
- 一个fstream对象是有两个逻辑子流的单个流，两个子流一个用于输入，另一个用 于输出。

stringstream 类:

- stringstream类支持面向字符串的输入和输出
- 可以用于对同一个字符串的内容交替读写，同样是由两个逻辑子流构成。

第十一章：异常处理

异常处理的基本思想：

![image-20221108103556028](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108103556028.png)

异常处理的语法：

![image-20221108103622918](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108103622918.png)

```c++
#include <iostream>
using namespace std;
int divide(int x, int y) {
 if (y == 0)
 throw x;
 return x / y;
}
int main() {
 try {
 cout << "5 / 2 = " << divide(5, 2) << endl;
 cout << "8 / 0 = " << divide(8, 0) << endl;
 cout << "7 / 1 = " << divide(7, 1) << endl;
 } catch (int e) {
 cout << e << " is divided by zero!" << endl;
 }
 cout << "That is ok." << endl;
 return 0;
} 
```

异常接口声明:

- 一个函数显式声明可能抛出的异常，有利于函数的调用者为异常处理做好准备 
- 可以在函数的声明中列出这个函数可能抛掷的所有异常类型。 例如：void fun() throw(A，B，C，D); 
- 若无异常接口声明，则此函数可以抛掷任何类型的异常。 
- 不抛掷任何类型异常的函数声明如下： void fun() throw();

自动的析构:

找到一个匹配的catch异常处理后:

- 初始化异常参数。 
- 将从对应的try块开始到异常被抛掷处之间构造（且尚未析构）的所有自动对象进行析构。
- 从最后一个catch处理之后开始恢复执行。 

```c++
#include <iostream>
#include <string>
using namespace std;
class MyException {
public:
 MyException(const string &message) : message(message) {}
 ~MyException() {}
 const string &getMessage() const { return message; }
private:
 string message;
};
class Demo {
public:
 Demo() { cout << "Constructor of Demo" << endl; }
 ~Demo() { cout << "Destructor of Demo" << endl; }
};
void func() throw (MyException) {
 Demo d;
 cout << "Throw MyException in func()" << endl;
 throw MyException("exception thrown by func()");
}
int main() {
 cout << "In main function" << endl;
 try {
 func();
 } catch (MyException& e) {
 cout << "Caught an exception: " << e.getMessage() << endl;
 }
 cout << "Resume the execution of main()" << endl;
 return 0; 
}

运行结果：
In main function
Constructor of Demo
Throw MyException in func()
Destructor of Demo
Caught an exception: exception thrown by func()
Resume the execution of main() 
```

标准异常类的继承关系:

![image-20221108103925815](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108103925815.png)

C++标准库各种异常类所代表的异常 :

![image-20221108103951350](C:\Users\lan\AppData\Roaming\Typora\typora-user-images\image-20221108103951350.png)

标准异常类的基础:

- exception：标准程序库异常类的公共基类
- logic_error表示可以在程序中被预先检测到的异常 
- runtime_error表示难以被预先检测的异常
