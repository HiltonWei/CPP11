# cpp11



## C++的主要特性


### 写在前面


C++17编译出的程序能否运行在xp系统上？[(配置XP程序)](https://docs.microsoft.com/zh-cn/cpp/build/configuring-programs-for-windows-xp?view=vs-2017)  
 Visual Studio 支持多个平台工具集，可面向的操作系统和不受默认工具集的运行时库。 例如，通过切换平台工具集，您可以使用 C + + 11、 C + + 14 和支持的 Visual Studio 中的 Visual c + + 编译器的 C + + 17 语言增强功能来创建面向 Windows XP 和 Windows Server 2003 的应用。 可以还使用较旧的平台工具集来维护二进制兼容旧代码，并仍充分利用 Visual Studio IDE 的最新功能。     

1. 可以任何时候，点击安装程序，支持或者修改xp的安装属性。  
2. 解决方案资源管理器”中，打开项目的快捷菜单，然后选择“属性”。在中属性页对话框中的项目，在配置属性 > 常规，将平台工具集属性设置为所需的 Windows XP 工具集。   
工具集所对应的差异：  
* a、C + + 语言功能 
* b、远程调试  
* c、静态分析  

MSVC版本号等同于平台工具集v140  
* C++历史版本：C++98, C++03, C++11(0x), C++14, C++17,C++2a  
* C++ 03基本上是一个非常小的修补程序发行版   
 
|版本|编译器|说明|
|----------|----------|----------|
|MSVC++ 15.0| _MSC_VER == 1910 (VS 2017 15.9)  ||
	|MSVC++ 14.0| _MSC_VER == 1900 (VS 2015 )|（改进了对C++11/14/17） | 
	|MSVC++ 12.0| _MSC_VER == 1800 (VS 2013)|（支持C++11与C99特性）  |
	|MSVC++ 11.0 |_MSC_VER == 1700 (VS 2012)|  |
	|MSVC++ 10.0| _MSC_VER == 1600 (VS 2010)|  |
	|MSVC++ 9.0 | _MSC_VER == 1500 (VS 2008) |（.NET 3.5 C++标准支持98%） | 
	|MSVC++ 8.0 | _MSC_VER == 1400 (VS 2005)|  |
	|MSVC++ 6.0 | _MSC_VER == 1200 (VC 6.0) | |
VS2005 & VS2008均遵循C++03，支持C++0x的某些功能，不完全符合C++0x。
[【各编译器对C++标准的支持情况】](https://zh.cppreference.com/w/cpp/compiler_support)
  

|Visual Studio Version |	MSVC Toolset Version |	MSVC compiler version (_MSC_VER)|  
|----------|----------|----------|
|VS2015 and updates 1, 2, & 3| 	v140 in VS  |version 14.00 	1900|  
|VS2017, version 15.1 & 15.2 |	v141 in VS  |version 14.10 	1910|    
|VS2017, version 15.3 & 15.4 |	v141 in VS  |version 14.11 	1911|  
|VS2017, version 15.5 	|v141 in VS  |version 14.12 	1912|  

[高版本Visual studio编写的程序支持xp](https://blog.csdn.net/yangbingzhou/article/details/49736065)

[维基百科 C++11](https://zh.wikipedia.org/wiki/C%2B%2B11)

[知乎C++11新特性详解](https://zhuanlan.zhihu.com/p/21930436)

[酷壳 C++11十大特性](https://coolshell.cn/articles/5265.html)

[C++11 快速上手](https://github.com/changkun/modern-cpp-tutorial)

### 去除的特性：
- 不再允许字符串字面值常量赋值给一个 char * 。  
 		如果需要用字符串字面值常量赋值和初始化一个
char * ，应该使用 const char * 或者 auto 。  
          
   ``` 
    //char * str = "hello, world";//vs2005 编译通过  
	const char * str1 = "hello world!";  
	auto str2 = "hello world!";
  ```  
         

- C++98 异常说明、 unexpected_handler 、 set_unexpected() 等相关特性被弃用，应该使用 noexcept 。  
-  auto_ptr 被弃用，应使用 unique_ptr 。  
- register 关键字被弃用，可以使用但不再具备任何实际含义。  
- bool 类型的 ++ 操作被弃用。
- 如果一个类有析构函数，为其生成拷贝构造函数和拷贝赋值运算符的特性被弃用了。  
- C 语言风格的类型转换被弃用（即在变量前使用 (convert_type) ） ，应该使用 static_cast 、 
reinterpret_cast 、 const_cast 来进行类型转换。  
- 特别地，在最新的 C++17 标准中弃用了一些可以使用的 C 标准库，例如 <ccomplex> 、 <cstdalign> 、 <cstdbool> 与 <ctgmath> 等
-  ......等等
C++11 提供了 std::bind 和 std::function ）、 export 等特性也均被弃⽤。

语言可用性的强化

###  常量包括：nullptr和constexpr
####  1、nullptr

```
#ifndef NULL
    #ifdef __cplusplus
        #define NULL 0
    #else
        #define NULL ((void *)0)
    #endif
#endif
```

常量0扮演着整型常量和空指针常量的双重身份。
为了解决0的二义性,C++11通过引入一个新的关键字来纠正这个问题，用来标志空指针常量: nullptr。
它的类型是nullptr_t(用来隐式转换到任何指针或者成员指针(pointer-to-member)类型，不能转换成整型类型，除了bool)。

我们使⽤了 decltype 和 std::is_same 这两个属于现代 C++ 的语法，简单
来说， decltype ⽤于类型推导，⽽ std::is_same ⽤于⽐较两个类型是否相等，我们会在后⾯decltype ⼀
节中详细讨论。

#### 2、constexpr 泛化的常量表达式
常量表达式(const expression) 是指值不会改变并且在编译过程就能得到计算结果的表达式。
const用来修饰常量，只有当其初始值是个常量表达式时，const修饰的对象才是个常量表达式。
C++11 提出了
 constexpr 用于定义常量表达式。一般而言，如果你认定变量是一个常量表达式，那就把它声明成 constexpr 类型。constexpr 变量在定义时必须初始化.
 constexpr 让⽤户显式的声明函数或对象构造函数在编译器会成为常量表达式，constexpr函数是指能用于常量表达式的函数。返回值类型及所有形参的类型都是字面值类型（算术类型，引用，指针等属于字面值类型），而且函数体中有且只有一条 return 语句（C++14已经扩充）。为了能在编译过程中随之展开，constexpr函数被隐式地指定为内联函数。constexpr 函数的返回值在编译时就能被确定（引申：如何在编译器就被确定，需要在有执行器和编译器的不同日志或者常量来表示）。

 constexpr 有很多规则，下面是不完整的，基本情形：

constexpr变量：使用常量表达式进行初始化；
constexpr函数：可以调用其他 constexpr 函数，并且所有执行的表达式（包括创建对象）必须能够出现在常量表达式中；  
 一个常量表达式：

 - 只能包含带有字面量类型的变量；
 - 只能访问那些生存期在该表达式求值过程中开始，或是在之前被声明为常量表达式友好的对象；
  - 不能包含任何带有未定义行为的表达式；
  - 不能调用未定义的 constexpr 函数或构造函数。




在下面这段代码中，实现函数 f() 使得 static_assert 不被触发。  
```
// <在这里填入你的答案>
int main() {
    constexpr int a = f();
    constexpr int b = f();
    static_assert(a != b, "fail");
}
```
[答案](https://zhuanlan.zhihu.com/p/24835482)
引申：变量、常量、常量表达式在PE文件中的所属区段。
引申:[(static_assert编译器断言)](https://blog.csdn.net/drdairen/article/details/76689014)
c++0x引入了static_assert关键字，用来实现编译期间的断言，叫静态断言。

## 变量和初始化
### 3、if/switch 变量声明强化 （C++17)

在之前 C++ 中，变量的声明在虽然能够位于任何位置，甚⾄于 for 语句内能够声明⼀个临时变量
int ，但始终没有办法在 if 和 switch 语句中声明⼀个临时的变量。

```
// 将 临 时 变 量 放 到 if 语 句 内
 if (const std::vector <int >::iterator itr = std::find(vec.begin(), vec.end(), 3);itr != vec.end()) 
 {
	 *itr = 4;
 }
```

F:\C++11\Code\VS2017\C++新特性\C++新特性>cl 3.if.switch.cpp

C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\VC\Tools\MSVC\14.15.26726\include\xlocale(319): warning C4530: 使用了 C++ 异常处理程序，但未启用展开语义。请指定 /EHsc
3.if.switch.cpp(23): error C2429: 语言功能 "if/switch 中的 init-statement" 需要编译器标志 "/std:c++17"

### 4、初始化列表

之前 C++ 中，不同的对象有着不同的初始化⽅法，例如普通数组、没有构造、析构和虚函数的类或结构体的都可以使⽤ {} 进⾏初始化，也就是我们所说的初始化列表。⽽对于类对象的初始化，要么需要通过拷贝构造、要么就需要使⽤ () 进⾏。这些不同⽅法都针对各⾃对象，不能通⽤。
```
	int arr[3] = {1, 2, 3};
	Foo foo(1, 2);
	std::vector <int> vec = {1, 2, 3, 4, 5};
```

为了解决这个问题，C++11 引入 std::initializer_list把初始化列表的概念绑定到了类型上，允许构造函数或其他函数像参数⼀样使⽤初始化列表，这就为类对象的初始化与普通数组和 POD 的初始化⽅法提供了统⼀的桥梁。
```
	class SequenceClass
	{
	public:
		//初始化列表构造函数
		SequenceClass(std::initializer_list<int> list) {}
	};
```
初始化列表除了⽤在对象构造上，还能将其作为普通函数的形参。
void foo2(std::initializer_list<std::string> list) { }
C++11 还提供了统⼀的语法来初始化任意的对象, Foo foo2{3,4};

### 5、结构化绑定  

结构化绑定提供了类似其他语⾔中提供的多返回值的功能。
Python的结构化绑定
```
	def GetValue():
	    x,y,z={4,5,6}
	    return x,y,z
	
	//结构化绑定
	a,b,c = GetValue()
	print (a,b,c)
```
C++11 新增了 std::tuple 容器⽤于构造⼀个元组，进⽽囊括多个返回值，但C++11/14 并没有提供⼀种简单的⽅法直接从元组中拿到并定义元组中的元素，尽管我们可以使⽤std::tie 对元组进⾏拆包，但我们依然必须⾮常清楚这个元组包含多少个对象，各个对象是什么类型，⾮常⿇烦。
```
	std::tuple<int, double, std::string> f() {
	    return std::make_tuple(1, 2.3, "456");
	}
	
	void main() {
	    auto [x, y, z] = f();
	}
```
引申:std::tie的释义。

## 类型推导 6-7
### 6、auto
之前C/C++中，参数类型要明确定义，对快速编码没帮助，面对一大堆复杂模板类型，必须明确指定类型才能后续编码，拖效率而且代码又长又硬。
auto和register 早就存在C++中，register（c++17保留关键字） 请求编译器尽可能的将变量存在CPU内部寄存器，而不是通过内存寻址访问，以提高效率。注意是尽可能，不是绝对，寄存器是有限的  register char *d;。
auto最常见和显著的是迭代器，但是不能用作形参 比如 int add(auto x,auto y),不能用来推导数组 auto arr2[10]=arr;

### 7、decltype  
decltype 关键字用于查询表达式的类型,且是在编译期就能推导出表达式类型,是为了解决 auto 关键字只能对变量进⾏类型推导的缺陷⽽出现。

对于decltype( e )而言，其判别结果受以下条件的影响：
1. 1、如果e是一个标识符或者类成员的访问表达式，则decltype(e)就是e所代表的实体的类型。
如果没有这种类型或者e是一个重载函数集，那么程序是错误的（下例中的 (2) 和 (3)）；
2. 2、如果e是一个函数调用或者一个重载操作符调用（忽略e外面的括号），那么decltype(e)就是该函数的返回类型（下例中的 (1)）；
3. 3、如果e不属于以上所述的情况，则假设e的类型是 T：当e是一个左值时，decltype(e)就是T&；
否则（e是一个右值），decltype(e)是T（下例中的 (4) 即属于这种情况。
在这个例子中，e实际是(a->x)，由于有这个括号，因此它不属于前面两种情况，所以应当以本条作为判别依据。而(a->x)是一个左值，因此会返回double &）。
4. 4、decltype((e))的结果永远是引用，而decltype(e)结果只有当 e 本身就是一个引用时才是引用

``` 
	auto x = 1;auto y = 2; 
	decltype(x+y) z;    //推断类型
	if (std::is_same <decltype(x), decltype(z)>::value)
    	std::cout << "type z == type x" << std::endl;
```

引申：std::is_same<T, U> ⽤于判断 T 和 U 这两个类型是否相等。

### 8、尾返回类型推导  
auto不能用于函数形参，但能否用于推导函数类型的推导？  
decltype(x+y) add(T x, U y) 这样是无法通过编译。编译时，x,y尚未被定义。为解决此问题，通过auto将返回类型后置。

~~~ //C++11
  template <typename T, typename U>   
	auto add2(T x, U y) -> decltype(x+y)
     { return x + y;}
~~~

//C++14 普通函数具返回值推导
``` template <typename T, typename U>
  	auto add3(T x, U y)
    {
		return x + y;
	}

	auto p = add2<double , int >(2.0, 2);
	auto q = add3<double , int >(1.0, 2);
 ```
### 9、decltype(auto)  
decltype(auto) 是 C++14 开始提供的⼀个略微复杂的⽤法。目的是用于对转发函数或封的返回类型进行推导，无需显式指定 decltype 的参数表达式。

 ```
	std::string lookup1();
	std::string look_up_a_string_1() { return lookup1(); }
	decltype(auto) look_up_a_string_1() { return lookup1();}	
 ```

## 控制流
### 10、if constexpr  
 constexpr，它将表达式或函数编译为常量结果。把一特性引入条件判断中，编译时就可以完成分支判断。

 ### 11、区间for迭代 C++11引入  
 写出类似python的简洁的for循环语句。 auto &可以更改值，auto只读  
```
   std::vector<int> vec = {1,2,3,4,5};  
  	for (auto &element : vec)  
        	element += 1;                      // 可写
```

## 模板 C++的黑魔法
C++ 的模板⼀直是这门语⾔的⼀种特殊的艺术，模板甚⾄可以独⽴作为⼀门新的语⾔来进⾏使⽤。
模板的哲学在于将⼀切能够在编译期处理的问题丢到编译期进⾏处理，仅在运⾏时处理那些最核⼼的动态服务，进⽽⼤幅优化运⾏期的性能。因此模板也被很多⼈视作 C++ 的⿊魔法之⼀。  
  
### 12、外部模板    
之前的C++中，模板只有在使用时才被编译器实例化，只要是被完整定义都会进行实例化，会导致重复的实例化导致编译时间增加，而且不能通知编译器并不进行实例化模板。  
template class std::vector <bool >; // 强行 实例化  
extern template class std::vector <double >; // 不在该当前编译文件中 实例化模板  

### 13、尖括号‘>’  
之前的C++编译，>>一律被当做右移运算符进行处理，但实际可能会用到
std::vector<std::vector<int>> matrix;
则在之前编译器中编译不过，C++11开始合法。 

### 14、类型别名模板  
了解类型别名模板之前，需理解“模板”和“类型”之间的不同。模板是用来产生类型的！typedef可以为类型定义新的名称，却没办法为模板定义新名称。因为模板不是类型。  

之前C++中，
``` 
	template <typename T, typename U>  
	class MagicType {};
```
 //  不合法  
```
	template <typename T>  
	typedef MagicType <std::vector <T>, std::string > FakeDarkMagic;  
```
//C++以后合法  

```
	template <typename T>  
	using TrueDarkMagic = MagicType <std::vector <T>, std::string >;  
```
typedef 语法： `typedef 原名称 新名称`; 但对函数指针等别名的定义语法却不相同，阅读困难。  
```
	typedef int (*process)(void *);  
	using NewProcess = int(*)(void *);  
```

### 15、默认模板参数 和可变模板参数  
```
	//参数可以默认化  
	template <typename T = int, typename U = int>
	auto add(T x, U y) -> decltype(x+y) 
	{  
		return x+y;
	}  
```
c++11之前的模板类和模板函数，只接受固定的一组参数，开发中用的委托方式依据编译时期个数的可变性采用不同的宏来改变的，c++之后允许任意个数、类别的参数。  
```
	//接受不限个数  
	template<typename... Values> class tuple;   
	class tuple<int, std::vector<int>> twoTuple; //2参数  
```
  
//0个类型  
`class tuple<> zTuple;`     //0参数  
//至少一个参数  
`template<typename T, typename... Rest> class tuple2;  `
```
	template<typename... Params> 
	void printf(const std::string &strFormat, Params... parameters);
```
其中，Params 与 parameters 分别代表模板与函数的变长参数集合， 称之为参数包 (parameter pack)。参数包必须要和运算符"...搭配使用，避免语法上的歧义。无法如同一般参数在类和函数汇总使用，以递归方式取出.


实现可变参数的printf，递归是⾮常容易想到的⼀种⼿段，也是最经典的处理⽅法。
 递归模板函数是⼀种标准的做法，但缺点显⽽易见的在于必须定义⼀个终⽌递归的函数，C++17 中增加了变参模板展开的⽀持,可以一个函数中展开打印printf。

 ### 16、折叠表达式  
 思考下，如何计算 1 - 10 这样的序列和  
 C++ 17 中将变长参数这种特性进⼀步带给了表达式。  
```
 template <typename ... T>  
auto sum(T ... t)   
{  
    return (t + ...);  
}  
int main()   
{  
    std::cout << sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10) << std::endl;  
}  
```

### 面向对象
### 17、构造函数  
a、委托构造 C++11引入
一个类中一个构造函数可以调用另外一个构造函数，以简化代码。
 [explicit关键字](https://blog.csdn.net/gatieme/article/details/50867575)  
```
Class Base{  
    Base(){}  
    Base(int i):Base(){}  //委托无参构造函数  

    //声明为explicit的构造函数不能在隐式转换中使用，只能显示调用，去构造一个类对象。  
    explicit Base(int n,std::string str):Base(n){}
}
```

b、继承构造
构造函数如需继承则需要将参数一一传递，效率低下。C++11用using引入继承构造函数概念。
```
Class Deri :public Base{
    public:
        using Base::Base;//继承构造

        // Deri(3); 采用继承构造
}
```

c、显式虚函数重载
之前C++中，经常发生意外重载虚函数。
```cpp
class Base {
    virtual void foo();
    };
class Deri: Base {
    void foo();
};
```
Deri可能恰巧同名，或者继承后需要调用__super::foo但忘记调用，或者继承后变为普通的类方法等等。
引入`override` 和 `final` 这两个关键字来防止。

#### override

重载时，显示告知编译器进行重载，查找失败则编译失败。
```cpp
class Base {
virtual void foo(int);
};
class Deri : public Base {
virtual void foo(int) override; // 合法
virtual void foo(float) override; // 非法, 父类没有此虚函数
};
```
#### final  
为了防止类被继续继承以及终止此虚函数继续重载引起。可用在函数和类上。 
```   
	class Base{  
	    virtual void foo(int) final;
	}  
	class Deri final: Base{}  //类采用final 不再被继承  
	class Err : Deri{}        //非法 已final  
	class Deri2 : Base  
	{  
	    void foo(int);      //非法  
	}  
```

### 显示禁用默认函数
在 C++ 中，如果程序员没有提供，编译器会默认为对象⽣成默认构造函数、复制构造、赋值运算符以及析构函数。  
若用户定义了任何构造函数，编译器不再生成默认构造函数，但有时我们希望同时拥有这两种构造函数。  
C++11可以允许显式声明采⽤或拒绝编译器⾃带的函数。  

```
cpp
class Magic {
    public:
    Magic() = default;                       // 显式声明  使用 编译器生成的构造
    Magic& operator=(const Magic&) = delete; // 显式声明  拒绝 编译器生成的构造
    Magic(int magic_number);
}
```

### 强类型枚举  
### 18、强类型枚举  
C++中的枚举类型都被视作整数，非类型安全，会让两种完全不同的枚举类型可以进⾏直接的⽐较（虽然编译器给出了检查，但并⾮所有）,**甚至同一个命名空间中的不同枚举类型的枚举值名字不能相同**.
C++11引入枚举类 （enumaration class），并使用 `enum class` 的语法进行声明：
```cpp
enum class new_enum : unsigned int {
    value1,
    value2,
    value3 = 100,
    value4 = 100
};
```
枚举类，实现了类型安全，不能够被隐式的转换为整数，同时也不能够将其与整数数字进行比较，更不可能对不同的枚举类型的枚举值进行比较。但相同枚举值之间如果指定的值相同，那么可以进行比较：

```cpp
if (new_enum::value3 == new_enum::value4) {
    std::cout << "==" << std::endl;         // 会输出
}
```

`new_enum::value3`,使用冒号和类型关键字来指定枚举中枚举值的类型，使得可以为枚举赋值（默认使用int)。

```cpp
#include <iostream>
template<typename T>
std::ostream& operator<<(typename std::enable_if<std::is_enum<T>::value, std::ostream>::type& stream, const T& e)
{
    return stream << static_cast<typename std::underlying_type<T>::type>(e);
}
```

这时，下面的代码将能够被编译：

```cpp
std::cout << new_enum::value3 << std::endl
```

end 语言可用性的强化

## 语言运行期的优化

### 19、Lambda  
c++11重要的新特性之一。实际是提供一个类似匿名函数，是想要一个函数但又不想去命名。几乎是现代语言的标配。  

基本语法： 

```
[捕获列表](参数列表) mutable(可选) 异常属性 -> 返回类型 {
// 函数体
}
```
采用的是尾返回类型。
lambda表达式内部默认是不能使用函数体外部的变量，使用捕获列表可以起到传递外部数据作用。分下列几种传递方式：

### 捕获列表

**1. 值捕获**

与参数传值类似，值捕获的前提是变量可以拷贝，不同之处则在于，被捕获的变量在 lambda 表达式被创建时拷贝，而非调用时才拷贝：
```
	int value_1 = 1;  
	auto copy_value_1 = [value_1] { return value_1;};   //
```

**2. 引用捕获**  

与引用传参类似，引用捕获保存的是引用，值会发生变化。 
``` 
	auto copy_value_2 = [&value_1] { return value_1;}; 
``` 

**3. 隐式捕获**  

手动书写捕获列表有时候是非常复杂的，这种机械性的工作可以交给编译器来处理，这时候可以在捕获列表中写一个 `&` 或 `=` 向编译器声明采用 引用捕获或者值捕获.  
  
捕获提供了lambda 表达式对外部值进行使用的功能，捕获列表的最常用的四种形式可以是：

* \[\] 空捕获列表
* \[name1, name2, ...\] 捕获一系列变量
* \[&\] 引用捕获, 让编译器自行推导捕获列表
* \[=\] 值捕获, 让编译器执行推导应用列表

**4. 表达式捕获\(C++14\)**

> 这部分内容需要了解后面马上要提到的右值引用以及智能指针

上面提到的值捕获、引用捕获都是已经在外层作用域声明的变量，因此这些捕获方式捕获的均为左值，而不能捕获右值。

C++14 给与了我们方便，允许捕获的成员用任意的表达式进行初始化，这就允许了右值的捕获，被声明的捕获变量类型会根据表达式进行判断，判断方式与使用 `auto` 本质上是相同的：
```cpp
{
    auto important = std::make_unique<int>(1);
    auto add = [v1 = 1, v2 = std::move(important)](int x, int y) -> int {
        return x+y+v1+(*v2);}
}
```
`important` 是一个独占指针，是不能够被捕获到的，这时候我们需要将其转移为右值，在表达式中初始化。
[make_unique](https://zh.cppreference.com/w/cpp/memory/unique_ptr/make_unique)

### 泛型Lambda
C++14,Lamdba函数形参可以使用 `auto`来达到泛型意义。

```cpp
auto add = [](auto x, auto y) {
    return x+y;
};
```

### 20、函数对象包装器

#### std::function
这属于标准库的优化，增加了C++语言进行时的能力，放在此处介绍。

Lambda 表达式的本质是一个和函数对象类型相似的类类型（称为闭包类型）的对象（称为闭包对象），当 Lambda 表达式的捕获列表为空时，闭包对象还能够转换为函数指针值进行传递，例如：

```cpp
using foo = void(int);  // 函数类型
void functional(foo f)  // 定义在参数列表中的函数类型 foo 被视为退化后的函数指针类型 foo*
  { f(1) ;}             // 通过函数指针调用函数
  
  auto f = [](int value){ std::cout << value << std::endl;}
  functional(f);        //传递闭包对象，隐式转换为foo* 函数指针
  f(1);                 //lambda调用
```

上述两种调用方式，一种将Lambda 作为函数类型传递进行调用，而另一种则是直接调用 Lambda 表达式，在 C++11 中，统一了这些概念，将能够被调用的对象的类型，统一称之为可调用类型。而这种类型，便是通过 `std::function` 引入的。

 `std::function` 是一种通用、多态的函数封装，它的实例可以对任何可以调用的目标实体进行存储、复制和调用操作，它也是对 C++中现有的可调用实体的一种类型安全的包裹（相对来说，函数指针的调用不是类型安全的），换句话说，就是函数的容器。当我们有了函数的容器之后便能够更加方便的将函数、函数指针作为对象进行处理。

 ```cpp
    // std::function 包装了一个返回值为 int, 参数为 int 的函数
    int foo2(int){}
    std::function<int(int)> func = foo2;
    
    std::function<int(int)> func2 = [&](int value) -> int { return 1 + value + 10; };
 ```

#### std::bind/std::placeholder

std::bind 用来绑定函数调用的参数。
std::placeholder 用来占位。

```cpp
int foo(int a, int b, int c) { }
int main() {
    auto bindFoo = std::bind(foo, std::placeholders::_1, 1,2); //对参数a进行占位
    bindFoo(1); // 调用 bindFoo 只需要提供第一个参数
}
```

### 21、 右值引用
右值引用解决了 C++ 中大量的历史遗留问题，消除了诸如 `std::vector`、`std::string` 之类的额外开销，也才使得函数对象容器 `std::function` 成为了可能，是和lambda同等重要的c++11的特征之一。

要弄明白右值引用到底是怎么一回事，必须要对左值和右值做一个明确的理解。

**左值\(lvalue, left value\)**，简单理解是赋值符号左边的值。准确来说，左值是表达式（不一定是赋值表达式）后依然存在的持久对象。

**右值\(rvalue, right value\)**，右边的值，是指表达式结束后就不再存在的临时对象。

而 C++11 中为了引入强大的右值引用，将右值的概念进行了进一步的划分，分为：纯右值、将亡值。

**纯右值\(prvalue, pure rvalue\)**，纯粹的右值，要么是纯粹的字面量，例如 `10`, `true`；要么是求值结果相当于字面量或匿名临时对象，例如 `1+2`。非引用返回的临时变量、运算表达式产生的临时变量、原始字面量、Lambda 表达式都属于纯右值。

**将亡值\(xvalue, expiring value\)**，是 C++11 为了引入右值引用而提出的概念（因此在传统 C++中，纯右值和右值是统一个概念），也就是即将被销毁、却能够被移动的值。

```cpp
std::vector<int> foo() {
    std::vector<int> temp = {1, 2, 3, 4};
    return temp;
}

std::vector<int> v = foo();
```

函数 `foo` 的返回值 `temp` 在内部创建然后被赋值给 `v`，然而 `v` 获得这个对象时，会将整个 temp 拷贝一份，然后把 `temp` 销毁，如果这个 `temp` 非常大，这将造成大量额外的开销（这也就是传统 C++ 一直被诟病的问题）。在最后一行中，`v` 是左值、`foo()` 返回的值就是右值（也是纯右值）。

但是，`v` 可以被别的变量捕获到，而 `foo()` 产生的那个返回值作为一个临时值，一旦被 `v` 复制后，将立即被销毁，无法获取、也不能修改。

#### 右值引用和左值引用

需要拿到一个将亡值，就需要用到右值引用的申明：`T &&`，其中 `T` 是类型。右值引用的声明让这个临时值的生命周期得以延长、只要变量还活着，那么将亡值将继续存活。

C++11 提供了 `std::move` 这个方法将左值参数无条件的转换为右值，有了它我们就能够方便的获得一个右值临时对象，例如：
```cpp
    std::string lv1 = "string,"; // lv1 是一个左值
    std::string&& rv1 = std::move(lv1); // 合法, std::move可以将左值转移为右值
```

#### 移动语义


传统 C++ 通过拷贝构造函数和赋值操作符为类对象设计了拷贝/复制的概念，但为了实现对资源的移动操作，调用者必须使用先复制、再析构的方式，否则就需要自己实现移动对象的接口。试想，搬家的时候是把家里的东西直接搬到新家去，而不是将所有东西复制一份（重买）再放到新家、再把原来的东西全部扔掉（销毁），这是非常反人类的一件事情。

传统的 C++ 没有区分『移动』和『拷贝』的概念，造成了大量的数据拷贝，浪费时间和空间。右值引用的出现恰好就解决了这两个概念的混淆问题，例如：
```cpp
#include <iostream>
class A {
public:
    int *m_ptr;
    A():m_ptr(new int(1)) { std::cout << "构造" << m_ptr << std::endl; } //见代码
    A(A& a):m_ptr(new int(*a.m_ptr)) { std::cout << "拷贝" << m_ptr << std::endl; } // 无意义的对象拷贝
    A(A&& a):m_ptr(a.m_ptr) { a.m_ptr = nullptr; std::cout << "移动" << m_ptr << std::endl; }
    ~A(){ std::cout << "析构" << m_ptr << std::endl; delete m_ptr; }
};
// 防止编译器优化
A return_rvalue(bool test) {
    A a,b;
    return (test) ? a : b;
}

A obj = return_rvalue(false);
```

return_rvalue，产生将亡值b，被 `A` 的移动构造（`A(A&&)`）引用，从而延长生命周期，并将这个右值中的指针拿到，保存到了 `obj` 中，而将亡值的指针被设置为 `nullptr`，防止了这块内存区域被销毁，避免了无意义的拷贝构造，加强了性能。。
```cpp
        std::string str = "Hello world.";
        std::vector<std::string> v;
        v.push_back(std::move(str));
```
使用v.push_back(const T&&), 将str移动到vector中，不会出现拷贝行为，减少拷贝开销，str的值将变为空。

### 22、引用坍缩规则和完美转发

问题来源：一个声明的右值引用其实是一个左值，给参数转发（传递）带来问题。

```cpp
    void reference(int&  v) {};
    void reference(int&& v) {}

    template <typename T>
    void pass(T&& v) 
    {
        reference(v); // 始终调用 reference(int& )
    }
```
     `pass(1)` ：虽然传递的是右值，但由于 `v` 是一个引用，所以同时也是左值。因此 `reference(v)` 会调用 `reference(int&)`，输出『左值』。

     `pass(v)`而言，`v`是一个左值，为什么会成功传递给 `pass(T&&)` 呢？ “引用坍缩规则”！

**引用坍缩规则**：在 C++ 中，我们不能够对一个引用类型继续进行引用，但由于右值引用的出现而放宽了这一做法，从而产生了引用坍缩规则，允许我们对引用进行引用，既能左引用，又能右引用。但是却遵循如下规则：

| 函数形参类型 | 实参参数类型 | 推导后函数形参类型 |
| :---: | :---: | :---: |
| T& | 左引用 | T& |
| T& | 右引用 | T& |
| T&& | 左引用 | T& |
| T&& | 右引用 | T&& |

因此，模板函数中使用 `T&&` 不一定能进行右值引用，当传入左值时，此函数的引用将被推导为左值。更准确的讲，**无论模板参数是什么类型的引用，当且仅当实参类型为右引用时，模板参数才能被推导为右引用类型**。这才使得 `v` 作为左值的成功传递。

### 23、完美转发  
完美转发就是基于上述规律产生的。所谓完美转发，就是为了让我们在传递参数的时候，保持原来的参数类型（左引用保持左引用，右引用保持右引用）。为了解决这个问题，我们应该使用 `std::forward` 来进行参数的转发（传递）：

```cpp
    template <typename T>
void pass(T&& v) {
    reference(v);                   //普通传参
    reference(std::move(v));        //std::move 传参
    reference(std::forward<T>(v));  //std::forward 传参
}
```
    普通传参：为左值转发
    std::move：总能接受一个左值，转发调用 `reference(int&&)`,总是右值引用，
    std::forward :没有造成任何多余的拷贝，同时**完美转发**\(传递\)了函数的实参给了内部调用的其他函数。

    std::move和std::forward转到定义，move只是单纯的进行了类型转换，`forward<T>(v)`实现了`static_cast<T&&>(v)`;  

## 容器
### 24、std::array和 std::forward_list
有了vector为什么还要引进array？vector太强大。 vector是堆内存，有时需要自己负责释放。array从栈内分配内存，能灵活访问元素，获得更高的性能。
array相比数组，使代码更加现代，封装了下操作函数，友好的使用标准库的容器算法，比如std:sort。
array会在编译时创建一个固定大小的数组，不能被隐式转换为指针。使用时只需要指定其类型和大小。
```cpp
    void foo(int * p,int len){}

    std::array<int, 4> arr= {1,2,3,4};

    int len = 4;
    std::array<int, len> arr = {1,2,3,4}; // 非法, 数组大小参数必须是常量表达式
    foo(arr,arr.size());                  // 非法 无法隐式转换
    foo(&arr[0], arr.size());
    foo(arr.data(), arr.size());

    std::sort(arr.begin(), arr.end());      //排序
```
std::forward_list
`std::forward_list` 是一个列表容器，使用方法和 `std::list` 基本类似。

[std::forward_list样例代码](https://blog.csdn.net/fengbingchun/article/details/72858282)

与 `std::list` 的双向链表的实现不同，`std::forward_list` 使用单向链表进行实现，提供了 `O(1)` 复杂度的元素插入，不支持快速随机访问（这也是链表的特点），也是标准库容器中唯一一个不提供 `size()` 方法的容器。当不需要双向迭代时，具有比 `std::list` 更高的空间利用率。

### 25、 无序容器
C++11 引入了两组无序容器：`std::unordered_map`/`std::unordered_multimap` 和 `std::unordered_set`/`std::unordered_multiset`。
`std::map`/`std::set`,内部红黑树实现，插入和搜索的平均复杂度`O(log(size))`。插入按`<` 操作符比较找位置，遍历时`<` 操作符顺序遍历。

使用参看代码。
  
### 26、元组 std::tuple
c++中除了std::pair外，都无法存放不同类型的数据，需要自己定义结构，std::pair只能保持两个元素，然后引入了std::tuple。
### 元组基本操作

关于元组的使用有三个核心的函数：

1. `std::make_tuple`: 构造元组
2. `std::get`: 获得元组某个位置的值
3. `std::tie`: 元组拆包
```
    auto tp = std::make_tuple(1, "Test", 3.14);   // tp = tuple<int, string, double>  
    assert(std::get<0>(tp) == 1 && std::get<1>(tp) == "Test");  
    int a; string s; double d;  
	std::tie(a, s, d) = tp;                //tie的用法  
    std::tie(a, std::ignore, d) = tp;   //用std::ignore占位符来表示不解某个位置的值  
```

`std::get<>` 依赖一个编译期的常量，在使用std::get<index = 1>(t);获取值是不合法的，想用如何使用？`boost::variant` 配合变长模板参数，请自行研究。
 除了使用常量获取元组对象外，C++14 增加了使用类型来获取元组中的对象：
    std::cout << std::get<std::string>(tp) << std::endl;

tuple元素合并
 `std::tuple_cat`,合并两个元组。
 ```cpp
auto new_tuple = std::tuple_cat(get_student(1), std::move(t));
```


## 标准库：指针

### RAII与引用计数。
`Objective-C`/`Swift` 中，为防止内存泄露，引入自动计数概念，增加一次引用计数加一，删除一次引用计数减一，计数为0则自动删除堆内存。
C++引入智能指针`std::shared_ptr`/`std::unique_ptr`/`std::weak_ptr`，辅助管理内存。

### 27、std::shared_ptr

`std::shared_ptr` 记录多少个 `shared_ptr` 共同指向一个对象，从而消除显示的调用 `delete`，当引用计数变为零的时候就会将对象自动删除。但因为使用 `std::shared_ptr` 仍然需要使用 `new` 来调用，这使得代码出现了某种程度上的不对称。为消除显示的使用 `new`，所以`std::make_shared` 会分配创建传入参数中的对象，并返回这个对象类型的`std::shared_ptr`指针。例如：  
auto pointer = std::make_shared<int>(10);  
通过 `get()` 方法来获取原始指针，通过 `reset()` 来减少一个引用计数，并通过`get_count()`来查看一个对象的引用计数。  
```
	auto pointer = std::make_shared<int>(10);   
	int *p = pointer.get(); // 这样不会增加引用计数  
	std::cout << "pointer.use_count() = " << pointer.use_count() << std::endl;   
	pointer.reset();  
```
### 28、std::unique_ptr

`std::unique_ptr` 是一种独占的智能指针，它禁止其他智能指针与其共享同一个对象，从而保证代码的安全，可以利用
`std::move` 将其转移给其他的 `unique_ptr`。
```cpp
	std::unique_ptr<int> p = std::make_unique<int>(10); // make_unique 从 C++14 引入
	std::unique_ptr<int> p1 = p; // 非法
	
	 std::unique_ptr<int> p2(std::move(p));
```

### 29、std::weak_ptr

 `std::shared_ptr` 在循环引用时，依然存在着资源无法释放的问题。
 ```cpp
struct A;
struct B;

struct A {
    std::shared_ptr<B> pointer;
};
struct B {
    std::shared_ptr<A> pointer;
};
 ```
 a,b 内部的 pointer 同时又引用了 `a,b`，这使得 `a,b` 的引用计数均变为了 2，而离开作用域时，`a,b` 智能指针被析构，却只能造成这块区域的引用计数减一，这样就导致了 `a,b` 对象指向的内存区域引用计数不为零，而外部已经没有办法找到这块区域了，也就造成了内存泄露，如图所示：

![](./img/pointers1.png)

引入弱指针引用`std::weak_ptr`，不会引起引用计数增加，当换用弱引用时候，最终的释放流程如下图所示：
weak_ptr是一种不控制对象生命周期的智能指针, 指向shared_ptr指针指向的对象的内存，却并不拥有该内存。
但是，使用weak_ptr成员lock，则可返回其指向内存的一个shared_ptr对象，且在所指对象内存已经无效时，返回指针空值（nullptr）。
![](./img/pointers2.png)

`std::weak_ptr` 没有 `*` 运算符和 `->` 运算符，所以不能够对资源进行操作，它的唯一作用就是用于检查 `std::shared_ptr` 是否存在，其 `expired()` 方法能在资源未被释放时，会返回 `true`，否则返回 `false`。

### 30、正则表达式

正则表达式简介略。  
C++11 提供的正则表达式库操作 `std::string` 对象，模式 `std::regex` (本质是 `std::basic_regex`)进行初始化，通过 `std::regex_match` 进行匹配，从而产生 `std::smatch` （本质是 `std::match_results` 对象）。  
用法：  
	 `std::regex re("[a-z]+\\.txt");  `  
	 `std::regex_match(str, re); `   //返回True和False     
	 `std::regex_match(std::string, std::smatch, std::regex);` //结果进 smatch  
	 [正则表达式库文档](http://en.cppreference.com/w/cpp/regex)  

### 31、标准库：线程与并发

#### a、std::thread
`std::thread` 用于创建一个执行的线程实例，包含`<thread>` 头，提供了很多基本的线程操作，例如，get_id(),join()。

#### b、std::mutex, std::unique\_lock
mutex 就是并发的核心之一，C++11引入了 mutex 相关的类，其所有相关的函数都放在 `<mutex>` 头文件中。  
实例 `std::mutex` 可以创建互斥量，其成员函数 `lock()` `unlock()` 操作。但是在在实际编写代码的过程中，最好不去直接调用成员函数，因为调用成员函数就需要在每个临界区的出口处和异常时调用 `unlock()`。  
C++11为互斥量提供了一个 RAII 语法的模板类`std::lock_gurad`, 在不失代码简洁性的同时，很好的保证了代码的异常安全性。
```cpp
void func(const std::string & m) {
    static std::mutex mutex;
    std::lock_guard<std::mutex> lock(mutex);

    // ...操作

    // 临界区结束 离开时，互斥锁会被析构，同时unlock互斥锁
}
```
C++保证了所有栈对象在声明周期结束时会被销毁，所以这样的代码也是异常安全的。无论 `func()` 正常返回、还是在中途抛出异常，都会引发堆栈回退，也就自动调用了 `unlock()`。

#### c、std::unique_lock 
`unique_lock`   则相对于 `std::lock_guard` 出现并更加灵活，并独占所有权（其他的 `unique_lock`不会同时拥有某个`mutex` 对象的所有权）的方式管理 `mutex` 对象上的上锁和解锁的操作。处理并发时推荐使用 。

`std::lock_guard` 不能显式的调用 `lock` 和 `unlock`， 而 `std::unique_lock` 可以在声明后的任意位置调用 ，可以缩小锁的作用范围，提供更高的并发度。

如果你用到了条件变量 `std::condition_variable::wait` 则必须使用 `std::unique_lock` 作为参数。

```cpp

	std::mutex mtx;
	
	void block_area() {
	std::unique_lock<std::mutex> lock(mtx);
	    //...临界区
	lock.unlock();
	    //...some other code 
	lock.lock(); //  can lock again
	}
```

#### d、std::future, std::packaged\_task  
主线程 A ，开辟B线程后执行其他操作，某个特定时间来获得线程B的结果。引入之前的做法，保存在全局变量、读写配置等方式，需要结果时调用线程等待结果。   
`std::future` 则是提供了一个访问异步操作结果的途径，用来获取结果，自然的作为一种简单的线程同步方式。  
`std::packaged_task` 可以用来封装任何可以调用的目标，实现异步的调用。    

封装好要调用的目标后，可以使用 `get_future()` 来获得一个 `std::future` 对象，以便之后实施线程同步。  
```cpp
	std::packaged_task<int()> task([](){return 7;});
	std::future<int> result = task.get_future(); // 获得 task 的 future
	std::thread(std::move(task)).detach();       // 在一个线程中执行 task
```

e、`std::condition_variable`    
 互斥操作不够用，为了解决死锁引入。比如，线程可能需要等待某个条件为真才能继续执行，而一个忙等待循环中可能会导致所有其他线程都无法进入临界区使得条件为真时，就会发生死锁。        
  所以，`condition_variable` 实例被创建出现主要就是用于唤醒等待线程从而避免死锁。  
`std::condition_variable`的 `notify_one()` 用于唤醒一个线程；  
`notify_all()` 则是通知所有线程。

