## 1.c++编程基础

### 1.1如何撰写c++程序

类：用户自定义的数据类型

class机制，赋予了“增加程序内之类型抽象化层次”的能力

声明语句：declaration statement

字符常量：character literal

main返回0表示程序执行成功

命名空间是一种将库名称封装起来的办法，可以避免与应用程序发生命名冲突的问题。

using directive是让命名空间中名称曝光的而最简单方法。

using namespace std；

### 1.2对象的定义与初始化

defining and initializing a data object

用assignment操作符初始化沿袭自c语言

构造函数初始化语法用来处理“多值初始化”

### 1.3撰写表达式

writing expressions

运算符优先级

### 1.4条件语句和循环语句

writing conditional and loop statement

如果测试条件值属于整数类型，可以用switch语句取代一大堆的if-else-if

不加break，则向下执行到末尾

continue 结束此次迭代

break结束当前循环

### 1.5如何运用array和vector

how to use arrays and vector

更建议使用vector

array必须提前指定大小

for(init-statement ; condition ; expression){}

vector知道自己的大小是多少

### 1.6指针带来弹性

pointers allow for flexibility

指针为程序引入了一层间接性，我们可以操作指针，而不再直接操作对象。

解引用dereference

rand和srand（）都是标准库提供的伪随机数生成器，cstdlib

dot成员选择操作符

arrow成员选择操作符

1.7文件的读写

writing and reading files

fstream

输出用的文件对象ofstream，各种模式/输出模式，追加模式

cerr输出对象：与cout类似，不过没有缓冲buffer情形，立即显示于终端中

endl会插入一个换行符，并清除输出缓冲区



## 2.面向过程的编程风格

### 2.1如何编写函数

how to write a function

返回类型，函数名，参数列表，函数体

函数声明：函数原型，返回类型+函数名+参数列表

函数声明让编译器检查后续出现的使用方式是否正确。

函数的最后一条语句是隐式退出点

### 2.2调用函数

invoking a function

调试器 debugger，检查各个对象在执行过程中的值

按值传递，按引用传递

将参数声明为reference：直接对所传入的对象进行修改，降低复制大型对象的额外负担

指针和引用的区别：引用一定代表某个对象，指针可以指向0

作用域，生命周期

内置类型的对象，如果在全局域，默认初始化为0，local域，不指定则不初始化。

动态内存管理：程序员管理，new，delete，在堆区分配

c++没有提供语法使我们从堆区分配数组的同时为元素设定初值。

delete 【】 p

### 2.3提供默认参数值

providing default parameter values

以参数传递作为函数间的沟通方式，比直接将对象定义在file scope更合适。

reference不同于pointer，不能设置为0

默认值的解析从最右边开始，因此默认参数一定在参数列表右侧

默认值只能指定一次，声明/定义，一般在头文件，声明中

头文件为函数带来更高的可见性

### 2.4使用局部静态对象

using local static object

为了节省函数间的通信问题而将对象定义在file scope，永远都是一种冒险。

局部静态对象，作用域在局部，生命周期到程序结束，存储位置在数据段

### 2.5声明inline函数

declaring a function inline

inline：要求编译器在每个函数调用点将函数的内容展开。将函数的调用改为一份函数代码副本代替。

只是请求，编译器是否执行视编译器而定。

适合inling：体积小，常调用，计算简单。

inline函数的定义常放在头文件中。

### 2.6提供重载函数

providing overloaded functions

函数重载：传入不同类型/数量的参数给同名函数。

编译器根据实参来选择调用哪个函数。

返回类型不同不属于重载。

### 2.7定义并使用模板函数

defining and using template functions

函数模板的机制：将函数内容与参数类型绑定，将参数类型信息剥离

typename T 占位符

函数模板也可以是重载函数

### 2.8函数指针带来更大的弹性

pointers to function add flexibility

函数指针必须指明函数返回类型和参数列表

enum枚举类型，默认第一个枚举值为0，之后+1

### 2.9设定头文件

setting up a header file

头文件：为函数只用维护一个声明

声明可以有多份，定义只能一份

const object 的定义只要一出文件之外便不可见（内链接）

头文件“”，一般用户自定义头文件用，从项目目录下开始寻找。

《》，从环境变量/编译器设置路径开始寻找



## 3.泛型编程风格

generic programming

STL=容器+泛型算法

vector，list顺序性容器

map，set，关联性容器，快速查找容器中的值

泛型算法：

与操作对象类型无关，通过函数模板实现

与容器无关，通过迭代器实现

### 3.1指针的算术运算

the arithmetic of pointers 

当数组被传给函数，或是由函数中返回，仅有第一个元素的地址被传递。

下标操作就是将起始地址加索引值偏移，再解引用

指针算术运算中，会将所指对象类型大小考虑进去

在底层指针上提供一层抽象，取代程序原本的“指针直接操作”，把底层指针的处理统统放在此抽象层中，让用户无须直接面对指针--》迭代器

### 3.2了解iterator

making sence of iterators

begin（）返回一个迭代器指向第一个元素

end（）返回迭代器指向最后一个元素的后边

需要实现的操作：赋值/比较/递增/解引用

const_iterator,只读的迭代器

典型泛型算法：

搜索：find，count，adjacent_find,find_if,count_if,binary_search,find_first_of

排序：merge，partical_sort,partition,random_shuffle,reverse,rotate,sort

复制/删除/替换：copy,remove,remove_if,replace,replace_if,swap,unique

关系：equal，includes，mismatch

生成/转换：fill，for_each,generate,transform

数值：accumulate，adjacent_difference，partial_sum,inner_product

集合：set_union,set_difference

### 3.3所有容器的共通操作

operations common to all containers

==

=

empty（）

size（）

clear（）

begin（）

end（）

insert（）

erase（）

### 3.4使用顺序性容器

using the sequential containers

用来维护排列有序，类型相同的数据

vector 连续内存，随机存取，索引效率高

list 任意位置插删效率高

deque 头部插删性能也高于vector

push/pop _back/front ()

### 3.5使用泛型算法

using the generic algorithm

#include 《algorithm》

find（），在无序结构中查找是否存在某值

binary_search（），有序结构中查找某值

count（），数值相符的元素数目

search（），是否存在某子序列

max_element(),返回最大值的迭代器

### 3.6如何设计一个泛型算法

how to design a generic algorithm

