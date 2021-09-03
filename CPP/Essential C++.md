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

函数对象 function object，某种class的实例对象，对（）函数调用操作符进行了重载，所以可以像用函数一样用对象。

标准库预定义的函数对象：#include<functional>

六个算术：plus，minus，negate，multiplies，divides，modules

六个关系：less，less_equal,greater,greater_equal,equal_to,not_equal_to

三个逻辑：logical_and,logial_or,logical_not

函数对象适配器，function object adaptor

绑定适配器bind1st，bind2nd，可以将指定值绑定到第1/2操作数

去除容器影响：传入迭代器范围

函数对象比函数指针效率高

### 3.7使用Map

using a map

key-value

a【key】，如果key不在map内，会将key置于map，并value初始化0

所以map的查询最好使用find/count （直接用【】，会改变map）

### 3.8使用Set

using a set

只存key

set默认按照所属类型的less-than关系排列

### 3.9如何使用iterator inserter

how to use iterator inserters

不想使用容器的=，怎么使用inserter 迭代器？三种

back_inserter(container);以pushback代替=

inserter（container,start）;以insert替代=

front_inserter(container);以pushfront替代=，适用于list，deque

#include 《iterator》

### 3.10使用iostream iterator

using the iostream iterator

#include<iterator>

istream_iterator<string> is(cin); //将迭代器绑定到标准输入设备，也就是first 迭代器

istream_iterator<string> eof;//不指定参数则代表end of line

copy（is，eof，back_inserter(text)）;//输入完成

oftream_iterator<string> os(cout," " );//第二个参数是每个元素的间隔

copy(text.begin(),text.end(),os);//输出完成

文件操作将cin/cout换成fstream对象即可



## 4.基于对象的编程风格

接口与实现

### 4.1如何实现一个class

how to implement a class

class的前置声明，

class stack；//有了前置声明才能进行* /& 的操作

实现：

class stack{

public：//接口

private：

}；

在classs定义里定义的函数，会被视为inline函数

：： 类作用域解析运算符

class定义和inline定义一般放在头文件中

### 4.2什么是构造/析构函数

what are class constructors and class deconstructor？

构造函数，类的初始化函数，与类同名，可以被重载

成员初始化列表，member initialization list，主要用于将参数传递给构造函数

析构函数：~类名，参数列表空，不能被重载，进行资源释放

拷贝构造函数/copy assignment 与 深/浅复制

### 4.3何谓mutable可变与const不变

what are mutable and const？

const成员函数，加在函数参数列表后，暗示此函数内不会修改对象的内容

将成员变量声明为mutable，则在const函数中可以修改此变量，可通过编译

### 4.4什么是this指针

what is the this pointer？

this指针是在成员函数内用来指向调用者(一个对象)

底层实现：编译器将this指针加到参数列表中表示调用者

### 4.5静态类成员

static class members

static data member，唯一的可共享的member，只有一份实体

静态成员函数：当函数只操作static数据时，可以将成员函数声明为static，之后函数可以不依托于具体对象，通过类名：：函数名，来使用

### 4.6打造一个iterator class

building an iterator class

运算符函数很像普通函数，区别是不用指定名称，只要在运算符前边加关键字operator即可

bool operator==（const classA&）const；

运算符重载的规则：

不可以引入新的运算符，	.  	 .* 	::  	?:  四个运算符不能重载

运算符操作数个数不能改变

运算符优先级不能改变

运算符函数的参数列表至少有一个class类型。也就是说不能重载nonclass的运算符。

++的前置后置，后置加一个参数，不用使用默认置为0

typedef 将某个类型（内置/复合/class）设定为另一个不同名称

### 4.7合作关系必须建立在友谊的基础上

collaboration sometimes requires friendship

将其他class/函数指定为friend，则friend就有了成员函数的权限

友元的声明不受public/private的影响

友谊的建立，通常是为了效率考虑，即使用public接口的函数可代替。

### 4.8实现一个copy assignment operator

implementing a copy assignment operator

深复制/浅赋值

### 4.9实现一个function object

implement a function object

function object：提供有（）运算符的class

通常把function object 当作参数传递给泛型算法

### 4.10重载iostream运算符

providing class instances of the iostream operators

ostream& operator<<(ostream &os, const classA &a ){

...

return os;}//连续 《《

input运算符的实现比较复杂，因为要考虑读入的数据有问题。

### 4.11指针，指向class member function

pointers to class member function

与普通的函数指针相似，不过还要声明所指的是哪一个class

void （class：：*pm）（int）=0；

//返回类型，参数列表，初始值

可以用typedef简化使用

使用时，对想指定的函数取址赋值给pm

maximal munch 编译规则：每个符号序列总是以‘合法序号序列’最长的那个解释

a+++p 会被解释为 a++ +p

成员函数指针与普通函数指针的区别，前者要通过同一类的对象加以调用，此对象就是成员函数中this所指之物。

## 5.面向对象编程风格

class的主要用途在于引入一个新的数据类型来表现我们想表现的实体。

类间的关系通过“面向对象编程模型”加以设定

### 5.1面向对象编程概念

object-oriented programming concepts

继承 inheritance：将相关的类组织起来，分享共通数据和操作

 多态 polymorphism：在类上编程时如同操控单一实体（类型无关的方式来操作），赋予更多弹性

继承：父子关系，基类/派生类，父类定义了子类的公共接口和私有实现，子类可以增加/覆盖继承来的东西

我们常用指向基类的*/&来操作对象，而不是直接操作各个对象。（多态，父类指针指向子类对象，运行时才能确定）（可以方便的加入/移除派生类）

静态绑定：程序执行前就解析出应该调用哪个函数

动态绑定：运行期确定

### 5.2漫游:面向对象编程思维

a tour of object-oriented programming

默认情况下，成员函数的解析在编译器静态进行。

虚函数在运行期进行。

定义派生类时，基类和派生类的构造函数都会执行。析构也都会执行，顺序相反。（基类的构造函数初始化公共元素，派生类构造函数初始化特有元素）

定义派生：

class derived ：public base{}；

使用派生类时不必刻意区分继承而来/自身定义的成员

### 5.3不带继承的多态

polymorphism without inheritance

在类对象存储一个变量，表示类型（enum）

还要检查类型是否合法

编程负担大，维护麻烦

### 5.4定义一个抽象基类

defining an abstract base class

静态成员函数无法被声明为虚函数

第一步，找出所有子类共通的操作行为，公共接口

第二步，找出哪些操作行为与类型相关（依赖类型），虚函数，派生类自己实现一份

第三步，找出每个行为的访问层级，public/private/protected

类里有纯虚函数则为抽象类，不能生成具体对象。必须派生后并定义虚函数再使用。

### 5.5定义一个派生类

defining a derived class

类进行继承声明前，基类的定义必须已经存在

在类之外对虚函数进行定义时，不必指明关键字virtual

通过class scope ：： 运算符，我们可以明确告诉编译器想调用哪一个函数实例，跳过虚函数机制

派生类里的同名函数会掩盖基类里的函数，可以用：：找到并调用。

### 5.6运用继承体系

using an inheritance hierarchy

继承和与函数等机制，帮助我们简化了修改和拓展的负担。

### 5.7基类型该多么抽象

how abstract should a base class be？

将派生类公用的实现内容放在基类里实现，简化新增派生类的成本。

### 5.8初始化、析构、复制

initialization ，destruction，and copy

派生类的构造函数不仅要为派生类的数据成员初始化，还要为基类的数据成员提供适当的值（调用基类的构造函数）

通过成员初始化列表传入

基类的析构会在派生类析构之后自动调用。

### 5.9在派生类之中定义一个虚函数

defining a derived class virtual function

当派生类继承了（没有自己覆盖掉）纯虚函数，自己也成为了抽象类，不能定义出对象。

覆盖时，函数原型要完全相同。

虚函数的静态解析：

两种情况，虚函数不会出现预期行为

1，基类的构造/析构内，调用某个虚函数，此刻派生类数据成员还没初始化，因此不会调用派生类里的虚函数。

2，使用的是基类对象，而不是指针/引用。内存空间（实际的基类对象的内存空间无法装入派生类对象）

### 5.10运行时的类型鉴定机制

run-time type identification

typeid运算符，运行时类型鉴定机制（rtti，run time  type identification ）

查询多态化的指针和引用，获得其所指对象的实际类型

#include 《typeinfo》

typeid（），返回一个type_info对象，包含类型相关的种子信息。

.name()返回 const char* ，表示类名

支持 == /  ！=操作

static/dynamic _cast<>



## 6.以template进行编程































 
