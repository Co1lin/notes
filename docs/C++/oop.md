# OOP Notes for exam

## 基本写法

### Makefile

!!! danger "Makefile needs tab instead of spaces!"
    
    下面的 Makefile 复制使用时可能会报错 `Makefile:23: *** missing separator.  Stop.` 。这是因为报错所在行不是用 Tab 缩进的，而用的是空格。此时需要改成 Tab 。

常规版：

```makefile
all: test	# 只输入make它就找第一个

test: product.o sum.o main.o functions.h
	g++ product.o sum.o main.o -o test	# specify the name of the output

product.o: product.cpp functions.h
	g++ -c product.cpp -o product.o	# -c表示只编译不链接

sum.o: sum.cpp functions.h
	g++ -c sum.cpp -o sum.o

main.o: main.cpp functions.h
	g++ -c main.cpp -o main.o

clean:	# clean不是第一个的依赖因此不会自动运行，所以需要make clean
	rm *.o test

```

比较通用的版本：

```makefile
####################################
# Learnt from Internet
# Edited by Colin
# 2020.02
####################################
cc = g++
FLAG = 
DEF =
CXXFLAGS = -std=c++17 -O3
prom = main
deps = $(shell find . -maxdepth 10 -name "*.h" -or -name "*.hpp")
src = $(shell find . -maxdepth 10 -name "*.cpp")
obj = $(src:%.cpp=%.o) 
$(prom): $(obj)
	$(cc) -o $(prom) $(obj)
%.o: %.cpp
	$(cc) $(CXXFLAGS) $(FLAG) $(DEF) -c $< -o $@
.PHONY: clean 
clean:
	rm -rf $(prom) $(obj)
```

### 程序命令行参数 argv

例子

```c++
int main(int argc, char **argv)	// or char*[] argv
{
	int a, b;
	a = atoi(argv[1]);
	b = atoi(argv[2]);
	std::cout << a + b << std::endl;
	return 0;
} 
```

argc是参数的数量，算上程序名argv[0]。遍历argc时有用，因为argc不好知道有几个。

argv是参数。第n个参数为argv[n]。

### gdb调试

g++ -g a.cpp –o a.out编译程序

gdb a.out 调试a.out程序

run  运行程序  
break + 行号 设置断点  
break 10 if (k==2) 可根据具体运行条件断点
delete break 1 删除1号断点
watch x 当x的值发生变化时暂停
continue 跳至下一个断点
step 单步执行(进入)  
next 单步执行(不进入)
print x 输出变量/表达式x  
GDB中输入 p x=1，程序中x的值会被手动修改为1
display x 持续监测变量/表达式x
list 列出程序源代码  
quit 退出
回车 重复上一条指令

所有命令都可以缩写为前几个字母，只要保持唯一，如`next`可缩写为`n`。

info break看断点信息

Disp 列代表断点被命中后，该断点保留(keep)、删除(del)还是关闭(dis)

https://zhuanlan.zhihu.com/p/29468840

### 函数重载 overload

同名、**参数必须不同**。作用域相同。返回值可相同也可不同。**根据函数调用语句的实际参数决定哪一个函数被调用**。

属于静态多态。编译时就知道。

内置类型转换：当函数重载时，会优先调用类型匹配的函数实现，否则才会进行类型转换

### auto

函数参数不能是auto类型。

**追踪返回类型的函数**

可以将函数返回类型的声明信息放到函数参数列表的后面进行声明

`auto func(char* ptr, int val) -> int;`

### decltype

类型推导。`decltype(a)`

#### 更高级的返回值推导用法

https://github.com/thu-coai/THUOOP/issues/24

```c++
// 得到一个类型的退化版本
int a = 3;
int& b = a;
double c = 3.14;
decay_t< decltype(b) >; // int
// 得到“更通用版本”
common_type_t< decltype(b) >; // int; 一个就直接退化
common_type_t< decltype(b), decltype(c) >;	// double
```

例子：

```c++
// max 函数
template<typename T1, typename T2>
std::common_type_t<T1, T2> max(T1 a, T2 b) {
    return b < a ? a : b;
}
// 返回容器的第一个元素值
template <class A>
auto work2(const A& _array) -> common_type_t< decltype(_array.front()) >
{
	return _array.front();
}
// 仿照此方法可以把容器中的元素包装进模板类再返回
template <class A>
auto work2(const A& _array) -> MyArray < common_type_t< decltype(_array.front()) > >
{
	/* ... */
}
```



### new & delete

`delete[] array`

详细过程见PPT L5 P42

### 避免重复包含头文件

```c++
#ifndef MATRIX_H
#define MATRIX_H

#endif 
```

Or

```c++
#pragma once
```

对比：

1. 后者效率更高。 原因： 前者要每次读入整个文件来处理，后者第二次读入相同文件的时候会直接跳过， 不会产生file IO。
   但是大部分编译器也针对前者的效率进行了优化，实际效率可能像差不多。
   
2. 后者以来文件系统的文件名，因此如果有符号链接/两个文件内容相同，则会被引入两次。

3. 前者在标准内，后者不在，但是被[绝大部分编译器](https://en.wikipedia.org/wiki/Pragma_once#Portability)支持。

## operator<

```c++
bool operator< (const Computer& _y) const // const declares that this function would not modify this.
{
	
}
```

### operator++

前缀：直接++

```c++
Test& operator++ ()
{
    ++data;
    return *this; // 返回自身
}
```

后缀：哑元参数int

先构造一个原来的，返回++之前的。

```c++
Test operator++(int)
{
    Test test(data);
    ++data;
    return test;
}
```

### operator[]

可用于“字典”/map。

如果返回类型是引用，则数组运算符调用可以出现在等号左边，接受赋值，即
	Obj[index] = value;
如果返回类型不是引用，则只能出现在等号右边
	Var = Obj[index];

```c++
char week_name[7][4] = { 	"mon", "tu", "wed", 
								"thu", "fri", "sat", "sun"};

class WeekTemp
{
  	int temp[7];
public:
  	int& operator[] (const char* name) // 字符串作下标
  	{
    	for (int i = 0; i < 7; i++)
      {
      		if (strcmp(week_name[i], name) == 0) 
					return temp[i];
    	}
  	}
};
```

### operator<<

为什么重载流运算符要返回引用？避免复制。

```c++
friend istream& operator>> (istream& in, Test& dst)
{
		in >> dst.id;
		return in;
}

friend ostream& operator<< (ostream& out, const Test& src)
{
	out << src.id << endl;
	return out;
}
cin >> obj;
cout << obj << endl;
```

**需要使用友元函数**。因为成员函数有一个隐藏的参数this。而流操作符左边必须是流类型。友元函数就没有那个this，满足了要求。

### 友元与友元类

友元不继承！

```c++
class Y {}; // Y能访问A的所有成员
class A {
    int data;  // 私有数据成员
    enum { a = 100 };  // 私有枚举项
    friend class X;  // 友元类前置声明（详细类型指定符）
    friend Y;  // 友元类声明（简单类型指定符） (C++11起)
}; 
class X {}; // X能访问A的所有成员
```

### 静态 static

静态变量：使用static修饰的变量
定义示例：static int i = 1;
初始化：初次定义时需要初始化，且只能初始化一次。如果定义时不初始化，则编译器会自动赋值为0
**静态局部变量**存储在静态存储区，生命周期将持续到**整个程序结束**
**静态全局变量**的作用域**仅限其声明的文件**，不能被其他文件所用，<u>可以避免和其他文件中的同名变量冲突</u>
静态函数：使用static修饰的函数
定义示例：static int func() {…}
**静态函数**的作用域**仅限其声明的文件**，不能被其他文件所用，可以避免和其他文件中的同名函数冲突

#### 静态数据成员（静态成员变量）（“类变量”）

属于整个类；被该类的所有对象共享

在类实例化对象前已分配内存空间

**<u>静态数据成员应该在.h文件中声明，在.cpp文件中定义。</u>**

如果静态数据成员在.h文件中同时完成声明和定义，链接将无法进行。因为可能头文件被包含了多次，从而出现重复定义。

test.h：

```c++
class Test
{
public:
  static int count;	//声明静态数据成员；对类实例计数用
  Test();
  ~Test();
};
```

test.cpp：

```c++
#include “Test.h”
int Test::count = 0;	//定义静态数据成员（要加上类型）
Test::Test() { count ++; }
Test::~Test() { count --; }
```

main.cpp：

```c++
#include <iostream>
#include “Test.h”
using namespace std;

int main() {
  Test t1[10];
  cout << “Test#: ” << Test::count << “ or ” << t1[0].count << endl;
  //通过类名或对象（实例）访问静态数据成员
}
```

#### 静态成员函数

在类实例化对象前分配内存空间

不能访问非静态成员，否则相当于使用没有初始化的变量

```c++
static int how_many() { return count; } 
```

```c++
cout << Test::how_many() << endl;
```

### 引用

#### 引用传参不构造也不析构

类成员里有指针时最好这样，避免函数结束时delete了形参里的指针。

#### 引用不能改指向



### 拷贝构造函数

参数是语言规定的，是同类对象的常量引用

```
MyClass(const MyClass& src) {}
```

如果没有显式定义，则自动合成，采用~~位拷贝(Bitcopy)~~，即直接使用赋值运算符拷贝类的所有数据成员。

被调用的三种常见情况：

1、用一个类对象定义另一个新的类对象

```	c++
Test a;
Test c = a;	// 并不是调用重载的等号！！而是用的拷贝构造函数！
Test b(a);
```

2、函数调用时以类的对象为形参

函数调用，形参传参

`Func(Test a)`

3、函数返回类对象

`Test Func(void)`

返回值时构造临时对象

`MyClass res = f(a)`

没有RVO时，返回值先给临时变量，临时变量再给`res`。

禁止RVO的编译选项：`-fno-elide-constructors `

### 右值引用

```c++
int &&e = a + b;

int &&j = lvalue;	// NOT allowed
```

左值引用能绑定左值，右值引用能绑定右值。

常量左值引用能也绑定右值。`const int &e = 3;`

```c++
void ref(int &&x)
{
	cout << "right " << x << endl;
}
ref(404);
```

混淆：右值引用本身为左值。

### 移动构造函数

```c++
Test(Test&& t) : buf(t.buf)
{ //直接复制地址，避免拷贝
		cout << "Test(Test&&) called. this->buf @ "
			<< hex << buf << endl;
		t.buf = nullptr; // 将t.buf改为nullptr，使其不再指向原来内存区域
		// 这之后t就用不了了；它的地址现在被拷贝目标对象所有
}

```

禁止RVO时，返回值先移动给临时变量，再移动给目标对象。

`std::move`：将左值转化为右值

move函数本身不对对象做任何操作，仅做类型转换，即转换为右值。**移动的具体操作在移动构造函数内实现。**

```c++
Test y = std::move(x);
f(std::move(z));	// 调用f的移动构造函数传参版本
// 性能更好的swap函数
template <class T>
swap(T& a, T& b)
{ 
     T tmp(std::move(a));
     a = std::move(b);
     b = std::move(tmp);
}
```

### 拷贝赋值运算符

赋值重载函数必须要是类的非静态成员函数(non-static member function)，**不能是友元函数**。

```c++
MyClass& operator= (const MyClass& right)
{
   if (this != &right)
   {	// 避免自己赋值给自己
			// 将right对象中的内容拷贝到当前对象中...
	 }
   return *this;	// 返回当前对象
}
```

注意区分下面两种代码：

```c++
// 已经定义的对象之间相互赋值，调用拷贝赋值运算符
ClassName a;
ClassName b;
a = b;
// a未定义；用b初始化a
ClassName a = b;
```

### 移动赋值运算符

```c++
Test& operator= (Test&& right)
{
	if (this == &right)	// 避免自己给自己（无用操作）
		cout << "same obj!\n";
	else
	{	
		this->buf = right.buf;  // 直接赋值地址
		right.buf = nullptr;	// 原来的置空
		cout << "operator=(Test&&) called.\n“;
  }
	return *this;
}
```

### 自动类型转换

一下两种方法必须恰好用一种。

#### 目标类型转换运算符

```c++
// Src -> Dst 的转换
class Src
{
public:
	operator Dst() const
	{ // 返回值类型是Dst；在convert function中必须省略不写
    Dst ret;
    // ...
    return ret;
	}
}
```

### 构造函数转换法

```c++
class Src;	// 前置类型声明，因为在Dst中要用到Src类
class Dst
{
public:
  Dst(const Src& s)
  { 
		// cout << "Dst::Dst(const Src&)" << endl; 
  }
};
```

#### 禁止自动类型转换

如果用explicit修饰类型转换运算符或类型转换构造函数，则相应的类型转换必须显式地进行。

```c++
explicit operator Dst() const;
explicit Dst(const Src& s);

Dst d1(s);	//可以执行，被认为是显式初始化
```

### 强制类型转换

const_cast，去除类型的const或volatile属性。
static_cast，类似于C风格的强制转换。无条件转换，静态类型转换。
dynamic_cast，动态类型转换，如**派生类和基类之间**的多态类型转换。
reinterpret_cast，仅仅重新解释类型，但没有进行二进制的转换。

如：

```c++
Dst d2 = static_cast<Dst>(s);
```

### 类的组合

构造顺序：先完成子对象构造，再完成当前对象构造

析构顺序：与构造顺序相反

### 类的继承

```c++
class Child : Father {};	// 默认为private继承
class Child : public Father {};
class Child : protected Father {};	// 很少被使用
```

构造函数、析构函数、友元函数、赋值运算符不被继承！

子类若想要显式调用父类构造函数，则只能在子类构造函数的**初始化成员列表**中进行。

```c++
Derive(int i) : Base(i) {};
```

继承父类构造函数

```c++
Base(int i) : data(i) {};
using Base::Base; 		///相当于 Derive(int i):Base(i) {};

// 当父类有多个构造函数时，一句using可以自动构造相应的多个
```

### 重写隐藏 redefining

目的：在派生类中重新定义基类函数，实现派生类的特殊功能。
屏蔽了基类的所有其它同名函数。
**函数名必须相同，函数参数可以不同**

相当于重新定义父类同名函数。一般不涉及虚函数。

可以在派生类中通过using 类名::成员函数名; 在派生类中“恢复”指定的基类成员函数（即去掉屏蔽），使之重新可用。

### 向上类型转换

只对public继承有效，在继承图上是上升的；对private、protected继承无效。

### 虚函数

通过基类**指针或引用**调用该成员函数时……

```c++
virtual int func(int a);
```

协变：派生类（子类）虚函数的返回类型和基类（父类）相同

实现原理：虚函数表。在构造函数的开头插入了初始化VPTR的代码。

构造函数不能是虚函数。在构造函数中调用一个虚函数，被调用的只是这个函数的本地版本，即虚机制在构造函数中不工作。

**析构函数往往是虚函数。**虚机制在析构函数中也不工作。

<u>**重要原则：**</u>
<u>**总是将基类的析构函数设置为虚析构函数**</u>

### 重写覆盖 override

子类重新定义父类中的虚函数，**函数名必须相同，函数参数必须相同，返回值一般情况应相同**。

屏蔽了父类中的同名函数。

程序运行时才知道。晚捆绑（只对虚函数起作用）。

```c++
// in Father
virtual void f(int a) {}
// in Child
virtual void f(int a) override {}

// final 关键字可以让虚函数不能被后续子类override
virtual void f(int a) final {}
```

使用const修饰成员函数，可能导致重写覆盖失效

### 抽象类与纯虚函数

纯虚函数的声明：

```c++
virtual int f(int a) = 0;
// 在类外定义函数，提供实现
int MyClass::f(int a) { return 1; }
```

包含纯虚函数的类就是**抽象类**。作用时提供接口。

抽象类不能定义对象！！（即不能实例化。）

纯虚函数被override之前还是纯虚函数。

如果一个抽象类的纯虚函数没有被全部实现（除了纯虚析构函数），则其子类还是抽象类，还是不能实例化。

**纯虚析构函数**仍然需要函数体。
目的：使基类成为抽象类，不能创建基类的对象。如果有其他函数是纯虚函数，则析构函数不必是纯虚的。

### 向下类型转换 dynamic_cast

```c++
SrcClass* p1;
TargetClass* p2 = dynamic_cast<TargetClass*>(p1);

Father* p1;
Child* p2 = dynamic_cast<Child*>(p1);
```

### 模板 template

模板参数必须在编译期确定。因此不能为变量，只能是常量。静多态。

函数模板

```c++
template <typename T> // template <class T>
T sum(T a, T b)
{
	return a + b;
}

// 指定调用类型
sum<int>(9, 2.1);
```

类模板

```c++
template <typename T>
class A
{
  T data;
public:
  void print();
  
  template <typename T1>
  T1 get();
};
//类外定义类模板中的成员函数
template <typename T>
void A<T>::print() {}

// “双重模板”
template <typename T> template <typename T1>
T1 A<T>::get() {}

A<int> a;
```

类模板的模板参数

```c++
template <typename T, unsigned size>
class Array
{
  T data[size];
public:
  void print();
};

A<int, 10> a;
```

#### 传递数组作为函数参数

普通数组；把大小作为模板参数n；注意符号“&”。

```c++
template <class A, int n>
void work(const A (&_array)[n])
{
  
}
// call the function
double arr = { 0, 1, 2 };
work(arr);
```



### 多线程 thread

```c++
void test(int seconds)
{
  this_thread::sleep_for(chrono::seconds(seconds));
}

int main()
{
  thread t_nothing;	// 创建一个空thread
  thread t1(test, 3);	// 创建一个thread
  thread t2(test, 2);
  // thread 创建之后到销毁之前，必须决定join还是detach
  t1.join();
  t2.detach();
  
  // 功能性接口
  this_thread::get_id();
  this_thread::sleep_for();
  this_thread::sleep_until();
  this_thread::yield();
  
  return 0;
}


```

### 主从模式

```c++
#include <iostream>
#include <cmath>
#include <vector>
#include <thread>
using namespace std;

thread* threads[4];	// 线程指针
int thread_total[4];	//每个线程的计数器
int total = 0, mi, mx;	//总计数器
bool check_num(int num) { /* ... */ }	//枚举是否为素数

//统计[l,r)之间的素数个数
//存入thread_total[num]中
void check(int l, int r, int num) 
{
  thread_total[num] = 0;
  for (int i = l; i < r; i++)
    if (check_num(i))
      thread_total[num]++; 
}

int main()
{
  mi = 1;
  for (int i = 0; i < 4; i++)
  {	// 用循环创建线程
    mx = mi + 5000000 / 4;
    if (mx > 5000000) mx = 5000000;
    //为第i个线程分配[mi,mx)区间的任务
    threads[i] = new thread(check, mi, mx, i);
    mi = mx; 
  }
  //阻塞主线程，等待所有子线程完成统计
  for (int i = 0; i < 4; i++)
    threads[i]->join();
  //汇总子线程的统计结果，释放thread实例
  for (int i = 0; i < 4; i++) {
    total += thread_total[i];
    delete threads[i];
  }
  //输出
  cout << total << endl;
  return 0; 
}
```

### 互斥锁模式 mutex

```c++
static mutex exclusive;	// 互斥量
void check_range(int l, int r)
{
  int tmp_total = 0;
  for (int i = l; i < r; i++)
    if (check_num(i))
	  tmp_total++;
  exclusive.lock(); //加锁
  total+=tmp_total;
  exclusive.unlock(); //解锁
}
```

### 异步 async future

```c++
#include <future>
#include <chrono>

int worker(int arg) { /* ... */ }	// 返回int值的一个工作函数

int main()
{
  future<int> fut = async(worker, 403);
  // auto fut = async(worker, 403);
  // future的接口
  fut.wait();	// 阻塞当前线程，等待异步线程结束
  int res = fut.get();	// get运行结果；一个future只能被get一次
  fut.wait_for(chrono::milliseconds(100));	// 超时后返回一个future_status，并取消对当前线程的阻塞
  /*
    future_status::deferred	仍未启动
    future_status::ready		结果就绪
    future_status::timeout	已超过时限，异步线程仍在执行
  */
  
  return 0;
}
```

#### 轮询

```c++
int worker(int arg) { /* ... */ }	// 返回int值的一个工作函数
int input() { /* ... */ }
vector<future<int>> future_lists;	// 异步线程对象表
vector<int> num_lists;	// 输入数据表

int main()
{
  while (ture)
  {
    int num = input();
    //创建异步线程
		future_lists.push_back(async(worker, num));
    num_lists.push_back(num);
    res_lists.push_back(0);
    
    //通过future检测每一个异步线程是否完成
		for (int i = future_lists.size() - 1; i >= 0; i--)
    {
	  	//每个future等待0.1秒来检测状态
	  	future_status status = future_lists[i].wait_for(
         chrono::milliseconds(100));
	  	if (status == future_status::ready)	// 已经得到了结果
      {
        // 输出结果
        cout << num_lists[i] << " : " << future_lists[i].get() << endl;
        //删除已经完成任务的future
        future_lists.erase(future_lists.begin() + i);
        num_lists.erase(num_lists.begin() + i);
      }
  }
  
  return 0;
}
```

### promise



### 函数指针

```c++
double work(int& x);
// [返回值] (*[声明的变量名])([参数类型列表])
double (*fp)(int&) = work;
// 自动推导
auto fp = work;
```

### 函数对象

类的对象，用起来像函数，看做函数对象。末尾带括号。

```c++
// 仿照greater<int>()实现函数对象
template<class T>
class Greater
{
public:
	bool operator()(const T &a, const T &b) const
  {	// 重载()；用于排序的cmp函数的特点：三个const
		return a > b;
	}
};
Greater<double>()(4.3, 1.0);
```

### function 类

function为函数指针与对象提供了统一的接口

```c++
// function<[返回值](参数列表)> func = f;
void process(function<int()> f1, function<double(int)> f2) {}
int work1();
class Work2
{
 public:
  double operator()(int a) { /* ... */ }
};

process(work1, Work2());
```

### 智能指针

```c++
#include <memory>
```

#### unique_ptr

独占。同一时间内只有一个智能指针可以指向该对象。

```c++
unique_ptr<string> p3 (new string ("auto"));
unique_ptr<string> p4；  
// 不能再 p4 = p3;
// 这样new完之后就不需要记得去delete了，避免了内存泄漏
```

#### shared_ptr

共享。计数。多个智能指针可以指向相同对象，该对象和其相关<u>资源会在“最后一个引用被销毁”时候释放</u>。

```c++
// 不能使用同一裸指针初始化多个智能指针
int* p = new int(); 
shared_ptr<int> p1(p);
shared_ptr<int> p2(p);	// 会产生多个辅助指针！

shared_ptr<int> sp(new int(1));

string *s1 = new string("s1");
shared_ptr<string> sp1(s1);
shared_ptr<string> sp2;
sp2 = sp1;

cout << sp1.use_count() <<endl;	//查看引用计数
cout << sp2.use_count() << endl;
cout << sp1.unique() << endl;	// 是否独占

cout << sp1 << endl;	// sp1代表的指针
cout << sp1.get() << endl;	// 同上
cout << sp1 << endl;	// "s1"；相当于*(sp1代表的指针)，是指针指向的对象

sp1.reset();	// 清除指针并减少引用计数

// 智能指针的向下转换
dynamic_pointer_cast<Child>(p);
```

#### weak_ptr 弱引用

指向对象，但不增加引用计数。

```c++
shared_ptr<int> sp(new int(3));
weak_ptr<int> wp1 = sp;

wp.use_count()	//获取引用计数
wp.reset()			//清除指针
wp.expired()		//检查对象是否无效
sp = wp.lock()	//从弱引用获得一个智能指针
```



### Lambda 函数

```c++
[capture] (parameters) mutable -> return-type {statement}

[](int x) { return x % 2 == 0;}	// 判断x是否是偶数
```

### 行为型模式

能以最少的代码变动完成功能的增减

#### 迭代器模式

```c++
//迭代器基类
class Iterator {
public:
    virtual ~Iterator() { }
    virtual Iterator& operator++() = 0;
    virtual float& operator++(int) = 0;
    virtual float& operator*() = 0;
    virtual float* operator->() = 0;
    virtual bool operator!=(const Iterator &other) const = 0;
    bool operator==(const Iterator &other) const
    {
        return !(*this != other);
    }
};

class Collection {
public:
    virtual ~Collection() { }
    virtual Iterator* begin() const = 0;
    virtual Iterator* end() const = 0;
    virtual int size() = 0;
};

//继承自迭代器基类并配套ArrayCollection使用的迭代器
class ArrayIterator : public Iterator {
    float *_data;	//ArrayCollection的数据
    int _index;		//数据访问到的下标
public:
    ArrayIterator(float* data, int index) :
            _data(data), _index(index) { }
    ArrayIterator(const ArrayIterator& other) :
            _data(other._data), _index(other._index) { }
    ~ArrayIterator() { }
    Iterator& operator++()
    {
        _index++;
        return *this;
    }
    /*
    Iterator operator++(int)
    {
        ArrayIterator ret(*this);
        _index++;
        return ret;
    }
    */
    float& ArrayIterator::operator++(int) {
        _index++;
        return _data[_index - 1];
    }

    float& operator*()  //对data的内存位置取值
    {
        return *(_data + _index);
    }
    float* operator->()
    {
        return (_data + _index);
    }
    bool operator!=(const Iterator &other) const    //判断是不是指向内存的同一位置
    {
        return (_data != ((ArrayIterator*)(&other))->_data ||
                _index != ((ArrayIterator*)(&other))->_index);
    }
};

class ArrayCollection : public Collection { //底层为数组的存储结构类
    friend class ArrayIterator; //friend可以使得配套的迭代器类可以访问数据
    float* _data;
    int _size;
public:
    ArrayCollection() : _size(10)
    {
        _data = new float[_size];
    }
    ArrayCollection(int size, float* data) : _size(size)
    {
        _data = new float[_size]; 		//开辟数组空间用以存储数据
        for (int i = 0; i < size; i++)
            *(_data+i) = *(data+i);
    }
    ~ArrayCollection()
    {
        delete[] _data;
    }
    int size()
    {
        return _size;
    }
    Iterator* begin() const //头迭代器，并放入相应数据
    {
        return new ArrayIterator(_data, 0);
    }
    Iterator* end() const   //尾迭代器，并放入相应数据
    {
        return new ArrayIterator(_data, _size);
    }
};
// in main:
float scores[]={ 90, 20, 40, 40, 30, 60, 70, 30, 90, 100 };
Collection *collection = new ArrayCollection(10, scores);

Iterator* begin = collection -> begin();
Iterator* end = collection -> end();
int passed = 0;

for (Iterator* p = begin; *p != *end; (*p)++) {
    if (**p >= 60)
        passed ++;
}
cout << passed << endl;	// 5
```

定义实现分开版本：

```c++
//迭代器基类
class Iterator {
public:
    virtual ~Iterator() { }
    virtual Iterator& operator++() = 0;
    virtual Iterator& operator++(int) = 0;
    virtual float& operator*() = 0;
    virtual float* operator->() = 0;
    virtual bool operator!=(const Iterator &other) const = 0;
    bool operator==(const Iterator &other) const {
        return !(*this != other);
    }
};

class Collection {
public:
    virtual ~Collection() { }
    virtual Iterator* begin() const = 0;
    virtual Iterator* end() const = 0;
    virtual int size() = 0;
};

//继承自迭代器基类并配套ArrayCollection使用的迭代器
class ArrayIterator : public Iterator {
    float *_data;	//ArrayCollection的数据
    int _index;		//数据访问到的下标
public:
    ArrayIterator(float* data, int index) :
            _data(data), _index(index) { }
    ArrayIterator(const ArrayIterator& other) :
            _data(other._data), _index(other._index) { }
    ~ArrayIterator() { }
    Iterator& operator++();
    Iterator& operator++(int);
    float& operator*();
    float* operator->();
    bool operator!=(const Iterator &other) const;
};

class ArrayCollection : public Collection { //底层为数组的存储结构类
    friend class ArrayIterator; //friend可以使得配套的迭代器类可以访问数据
    float* _data;
    int _size;
public:
    ArrayCollection() : _size(10){_data = new float[_size]; }
    ArrayCollection(int size, float* data) : _size(size) {
        _data = new float[_size]; 		//开辟数组空间用以存储数据
        for (int i = 0; i < size; i++)
            *(_data+i) = *(data+i);
    }
    ~ArrayCollection() { delete[] _data; }
    int size() { return _size; }
    Iterator* begin() const;
    Iterator* end() const;
};
Iterator* ArrayCollection::begin() const {	//头迭代器，并放入相应数据
    return new ArrayIterator(_data, 0);
}
Iterator* ArrayCollection::end() const { 		//尾迭代器，并放入相应数据
    return new ArrayIterator(_data, _size);
}

//迭代器各种内容的实现
Iterator& ArrayIterator::operator++() {
    _index++; return *this;
}
//因为是数组，所以直接将空间指针位置+1即可，可以思考下这里为什么返回float&，而不是Iterator
/*
float& ArrayIterator::operator++(int) {
    _index++;
    return _data[_index - 1];
}
*/
Iterator& ArrayIterator::operator++(int) {
    ArrayIterator ret(*this);
    _index++;
    return ret;
}

//对data的内存位置取值
float& ArrayIterator::operator*() {
    return *(_data + _index);
}
float* ArrayIterator::operator->() {
    return (_data + _index);
}
//判断是不是指向内存的同一位置
bool ArrayIterator::operator!=(const Iterator &other) const {
    return (_data != ((ArrayIterator*)(&other))->_data ||
            _index != ((ArrayIterator*)(&other))->_index);
}
```

hasNext实现模式：

```c++
class Item
{
public:
    Item(const string& strName, const float& price):
            m_name(strName),m_price(price) {}
    Item(const Item& item):
            m_name(item.m_name),m_price(item.m_price) {}
    string tostring()
    {
        std::stringstream buffer;
        buffer << m_price;
        string strPrice = buffer.str();
        string strName = m_name + " :";
        return strName + strPrice;
    }
private:
    string m_name;
    float m_price;
};

class Container;
class Menu;
class MenuIterator;


class Iterator
{
public:
    virtual ~Iterator() {}
    //virtual void first() = 0;
    virtual void next() = 0;
    virtual bool hasnext() = 0;
    virtual Item* current() = 0;
protected:
    Container * m_pContainer;
};


class Container
{
public:
    virtual ~Container() {};
protected:
    //Observer(){};
};


class Menu : public Container
{
public:
    virtual ~Menu()
    {
        for(int i=0 ; i< m_items.size(); i++)
        {
            delete m_items[i];
        }
    }
    int size()
    {
        return m_items.size();
    }
    Item* value(int nIndex)
    {
        if(nIndex >= 0 && nIndex < m_items.size())
        {
            return m_items[nIndex];
        }
        else
        {
            return NULL;
        }
    }
    void additem(Item& item)
    {
        Item *pItem = new Item(item);
        m_items.push_back(pItem);
    }

private:
    friend class MenuIterator;
    vector<Item*> m_items;
};


class MenuIterator : public Iterator
{
    Menu* m_menu;
    int curpos;
public:
    MenuIterator(Menu& a): m_menu(&a), curpos(0) {}
    /*virtual void first()
    {
        curpos=0;
    }*/
    virtual void next()
    {
        curpos++;
    }
    virtual bool hasnext()
    {
        if(curpos >= 0 && curpos < m_menu->m_items.size())
            return true;
        else
            return false;
    }
    virtual Item* current()
    {
        return m_menu->value(curpos);
    }
};


int main()
{
    Item it1("chicken", 10.0);
    Item it2("Apple", 5.0);
    Item it3("Beaf", 20.0);
    Item it4("soup",15.0);

    Menu menu;
    menu.additem(it1);
    menu.additem(it2);
    menu.additem(it3);
    menu.additem(it4);

    Iterator* iter = new MenuIterator(menu);

    while(iter->hasnext())
    {
        Item* pItem = iter->current();
        if(pItem)
            cout << pItem->tostring() << endl;
        iter->next();
    }
}
```

#### 模板方法

抽象父类定义接口、流程，（每种组合的）子类具体实现。针对接口编程。

抽象类：一次完成两个操作。操作1有m种实现，操作2有n种实现。

则一共需要m*n个子类实现全部完成操作的方式。

```c++
class AbstractClass
{
public:
	virtual void operation1() = 0;
	virtual void operation2() = 0;

	void run()
  { // 定义一个算法流程
		operation1();
		operation2();
	}
};

class ConcreteA : public AbstractClass
{	// 实现方案组合A
public:
	void operation1()
  { // 定义不同的操作
		cout << "ConcreteA::operation1" << endl; 
	}
	void operation2()
  {
		cout << "ConcreteA::operation2" << endl;
	}
};

class ConcreteB : public AbstractClass
{	// 实现方案组合B
public:
	void operation1()
  { // 定义不同的操作
		cout << "ConcreteB::operation1" << endl; 
	}
	void operation2()
  {
		cout << "ConcreteB::operation2" << endl;
	}
};

AbstractClass* absClass[] = { new ConcreteA(), new ConcreteB() };
for (auto x: absClass)
{
		x -> run();
		delete x;
}
```

#### 策略模式

抽象类定义操作流程，初始化时接收各步操作的具体的策略子类。

父类：一次完成两个操作。操作1有m种实现策略，操作2有n种实现策略。

则一共需要m+n个策略子类实现全部完成操作的方式。

单一责任原则。

```c++
class AbstractClass
{
  // 获取不同的策略类的每一步操作的策略指针
	Op1Strategy *op1_strategy;
	Op2Strategy *op2_strategy;
public:
	// 各个策略类的组合
	AbstractClass(Op1Strategy* op1, Op2Strategy* op2) : 
		op1_strategy(op1), op2_strategy(op2) {}
  
	// 定义操作流程
	void run()
  {
		op1_strategy->operate();	// 执行策略
		op2_strategy->operate();
	}
};

// 操作1策略基类
class Op1Strategy
{
public:
	virtual void operate() = 0;
}

// 操作1具体实现1
class Op1StrategyImpl1: public Op1Strategy
{
public:
	void operate()
  {
		cout << "Operation1 Implementation 1" << endl;
	}
}

// 操作1具体实现2
class Op1StrategyImpl2: public Op1Strategy
{
public:
	void operate()
  {
		cout << "Operation1 Implementation 2" << endl;
	}
}

// in main:
Op1StrategyImpl1* op1imp1 = new Op1StrategyImpl1();
Op2StrategyImpl1* op2imp1 = new Op2StrategyImpl1();
AbstractClass* solve = new AbstractClass(op1imp1, op2imp2);
solve->run();
```

### 结构型模式

能在结构层面上尽可能的解耦合

#### 适配器模式

将一个类的接口转换成客户希望的另一个接口，从而使得原本由于接口不兼容而不能一起工作的类可以在统一的接口环境下工作。

##### 组合适配

```c++
//堆栈基类
class Stack
{
public:
virtual ~Stack() { }
	virtual 	bool full() = 0;
	virtual 	bool empty() = 0;
	virtual 	void push(int i) = 0; 
	virtual 	void pop() = 0;
	virtual 	int size() = 0;
	virtual 	int top() = 0;
}

class Vector2Stack : public Stack
{
private:
	std::vector<int> m_data; //将vector的接口组合进来实现具体功能
	const int m_size;
public:
	Vector2Stack(int size) : m_size(size) { }
	bool full() { return (int)m_data.size() >= m_size; } 	//满栈检测
	bool empty() { return (int)m_data.size() == 0; } 	//空栈检测
	void push(int i) { m_data.push_back(i); } 		//入栈
	void pop() { if (!empty()) m_data.pop_back(); }	//出栈
	int size() { return m_data.size(); }				//获取堆栈已用空间
	int top() {										//获取栈头内容
		if (!empty()) 
			return m_data[m_data.size()-1];
		else 
			return INT_MIN;
	}
};

Vector2Stack stack(10);
```

##### 继承适配

```c++
//直接继承vector并改造接口，采用私有继承可以使得外界只能接触到Vector2Stack中的接口

class Vector2Stack : private std::vector<int>, public Stack {
public:
	Vector2Stack(int size) : vector<int>(size) { }
	bool full() { return false; }
	bool empty() { return vector<int>::empty(); }
	void push(int i) { push_back(i); }
	void pop() { pop_back(); }
	int size() { return vector<int>::size(); }
	int top() { return back(); }
};

Vector2Stack stack(10);
```

#### 代理/委托模式

在被访问对象上加上一个访问层，将复杂操作包裹在内部不对外部类开放，仅对外开放功能接口，即可完成上述要求，这就是代理/委托模式。

适配器的核心要素是变换接口，代理的核心要素是分割访问对象与被访问对象以减少耦合，并能**在中间增加各种控制功能**。

```c++
template <typename T> 
//提前声明智能指针模板类
class SmartPtr; 

//辅助指针，用于存储指针计数以及封装实际指针地址
template <typename T>
class U_Ptr {
private:
	friend class SmartPtr<T>;
  
	U_Ptr(T *ptr) :p(ptr), count(1) { }
~U_Ptr() { delete p; }
  
	int count;   
	T *p; //数据存放地址
};

template <typename T>
//智能指针
class SmartPtr {
private:
	U_Ptr<T> *rp;	//进行实际指针操作的辅助指针
public:
	SmartPtr(T *ptr) :rp(new U_Ptr<T>(ptr)) { }
	//调动拷贝构造即增加引用计数
	SmartPtr(const SmartPtr<T> &sp) :rp(sp.rp) { ++rp->count; }
	SmartPtr& operator=(const SmartPtr<T>& rhs) {
		++rhs.rp->count; //赋值号后的指针引用加1
		if (--rp->count == 0) delete rp;	//原内部指针引用减1
		rp = rhs.rp;		//代理新的指针
		return *this;
	}
	~SmartPtr() { //只有引用次数为0才会释放
		if (--rp->count == 0) delete rp;
	}
	//对智能指针操作等同于对内部辅助指针操作
	T & operator *() { return *(rp->p); }
	T* operator ->() { return rp->p; }
};

int main(int argc, char *argv[])
{
	//声明指针
	int *i = new int(2);
	//使用代理来包裹指针
	SmartPtr<int> ptr1(i);
	SmartPtr<int> ptr2(ptr1);
	SmartPtr<int> ptr3 = ptr2;
	//之后的操作均通过代理进行
	cout << *ptr1 << endl;
	*ptr1 = 20;
	cout << *ptr2 << endl;
	return 0;
}

```

#### 装饰器模式

统一继承自Component。

最后是链式调用。

```c++
#include <iostream>
using namespace std;

//所有View的基类
class Component {
public:
    virtual ~Component() { }
    virtual void draw() = 0;
};

//一个基本的TextView类
class TextView : public Component {
public:
    void draw() {
        cout << "TextView." << endl;
    }
};

//装饰器的核心内涵在于用装饰器类整体包裹改动之前的类，以保留原来的全部接口
//在原来接口保留的基础上进行新功能扩充

class Decorator : public Component {
    //这里一个基类指针可以让Decorator能够以递归的形式不断增加新功能
    Component* _component;
public:
    Decorator(Component* component) : _component(component) {
    }
    virtual void addon() = 0;
    void draw() {
        addon();
        _component -> draw();
    }
};

//包裹原Component并扩充边框
class Border : public Decorator {
public:
    Border(Component* component) : Decorator(component) { }
    void addon() { cout << "Bordered "; }
};

//包裹原Component并扩充水平滚动条
class HScroll : public Decorator {
public:
    HScroll(Component* component): Decorator(component) { }
    void addon() { cout << "HScrolled "; }
};

//包裹原Component并扩充垂直滚动条
class VScroll : public Decorator {
public:
    VScroll(Component* component): Decorator(component) { }
    void addon() { cout << "VScrolled "; }
};


int main()
{
    //基础的textView
    TextView textView;
    //在基础textView上增加滚动条
    VScroll vs_TextView(&textView);
    //在增加垂直滚动条的基础上增加滚动横条
    HScroll hs_vs_TextView(&vs_TextView);
    //在增加水平与垂直滚动条之后增加边框
    Border b_hs_vs_TextView(&hs_vs_TextView);
    b_hs_vs_TextView.draw();

    return 0;
}
```

### 创建型模式

#### 单例模式



#### 工厂模式

```c++
class TeaFactory {
public:
    void setMilk(int amount) { ... }
    void setSugar(int amount) { ... }
    Tea *createTea(const string &type) {
        Tea *tea = nullptr;
        if (type == "GreenTea")
            tea = new GreenTea;
        else if (type == "BlackTea")
            tea = new Blacktea;
        else ...  // 其他可能的茶叶类型
        if (milkAmount > 0) tea->addMilk(...);
        if (sugarAmount > 0) tea->addSugar(...);
        ...  // 其他的属性配置
    }
};

```

#### 抽象工厂模式

```c++
class AbstractLanguageFactory {
public:
    virtual Lexer *createLexer();
    virtual Parser *createParser();
    virtual Generator *createGenerator();
};

class CppFactory : public AbstractLanguageFactory {
public:
    Lexer *createLexer() { return new CppLexer; }
    Parser *createParser() { return new CppParser; }
    Generator *createGenerator() { return new CppGenerator; }
};

class JavaFactory : public AbstractLanguageFactory { ... };

class Compiler {
    AbstractFactory *factory;
public:
    Compiler(AbstractFactory *factory) {
        this->factory = factory;
    }
    LexResult *lex(Code *input) {
        Lexer *lexer = factory->createLexer();
        return lexer->lex(input);
    }
    ParseResult *parse(LexResult *input) {
        Parser *parser = factory->createParser();
        return parser->parse(input);
    }
    // ...
};

int main() {
	CppFactory *cppFactory = new CppFactory();
	Compiler *cppCompiler = new Compiler(cppFactory);
	
	Code *code = ...
	LexResult *lex = cppCompiler->lex(code);
	// ...
}
```



## 实用工具类知识

### 排序

```c++
bool mycmp(int a, int b)
{
  /*
  	a在b前返回true
  	a在b后返回false
  */
}
sort(arr + 0, arr + n, mycmp);
```

```c++
vector<Computer> data;
// Computer 类重载了“>”、“<”
sort(data.begin(), data.end(), greater<Computer>());
```

```c++
struct Cmp
{
  bool operator() (const Computer& _a, const Computer& _b)
  {
    return _a.stock > _b.stock;
  }
};
sort(data.begin(), data.end(), Cmp);
```



### 字符串类

### 文件读写 fstream

```c++
#include <fstream>
fstream fs1("./input.txt", ios::in);
fs1 >> a;
fstream fs2("./output.txt", ios::out);
fs2 << a;

ifstream fin("./input.txt");
ofstream fout("./output.txt");
// 有f！！不是istream / ostream ！

// 循环从文件读入未知长度的内容，直到读完为止。
while (fin) {}
```

### 输入

```c++
string str;
// 读到空格
cin >> str;
// 读一整行
getline(cin, str);
// 读到特定分隔符；可以读入换行符
getline(cin, str, '#');

// 流操纵算子
cin >> ws;	// ws算子；除去前导空格
// 检查下一个字符，但并不读取。如果到结尾了就终止无限循环读入。
int c = cin.peek();	// 返回的是char类型的字符！！比如0返回的是ASCII码48。
if (c == EOF)
  break;

cin.get();	//读取一个字符
cin.ignore(int n=1, int delim=EOF);	//丢弃n个字符，或者直至遇到delim分隔符
cin.peek();	//查看下一个字符
cin.putback(char c);	//返还一个字符
cin.unget();	//返还一个字符

```

### 输出

格式化输出：

```c++
#include <iomanip>

cout << defaultfloat;  //还原默认输出格式
cout << setprecision(2) << 3.1415926 << endl;
				//输出精度设置为2 -> 3.2
cout << oct << 12 << " " << hex << 12 << endl; 
				//八进制输出 -> 14  十六进制输出 -> c
cout << dec;	//还原十进制
cout << setw(3) << setfill('*') << 5 << endl;
				//设置对齐长度为3，对齐字符为* -> **5
cout << fixed << 2018.0 << " " << 0.0001 << endl;
				//浮点数补全 -> 2018.000000 0.000100
cout << scientific << 2018.0 << " " << 0.0001 << endl;
				//科学计数法 -> 2.018000e+03 1.000000e-04
```





### string

```c++
// 转换为常量char字符串
str.c_str();
str.push_back('a');
str.append(s2);
str += s1;
```

#### 截取

```c++
string s0("0123456789");
	string s1(s0, 3, 4);	// 从s0[3]开始，长度为4。结果：3456
	string s2(s0, 4);	// 从s0[4]开始直到结尾。结果：456789
	string s3("0123456789", 4);	// 从头开始，长度为4。（取前4。）结果：0123
	string s4(s0.begin() + 2, s0.begin() + 6);	// 从s0[2]开始，到s0[5]（s0[6]之前）结束。结果：2345
	cout << s1 << endl;
	cout << s2 << endl;
	cout << s3 << endl;
	cout << s4 << endl;
```





#### char[]转整型int (stoi)

```c++
int atoi(const char *str)
// Usage:
#include <stdlib.h>
char s;
int val = atoi(s);

// 进制转换 （第二个参数是起始位置）
int a = stoi("2001")		  //a=2001
std::string::size_type sz;   // 一个大小数值 size_t alias
int b = stoi("50 cats", &sz)  //b=50 sz=2 读入长度
int c = stoi("40c3", nullptr, 16) //c=0x40c3 十六进制
int d = stoi("0x7f", nullptr, 0)  //d=0x7f 自动检查进制
```

反过来是itoa。然而这个在一些环境下是没有的。

https://blog.csdn.net/p312011150/article/details/81273888

#### number to string

```c++
int a;
string s = to_string(a);
```

#### sstream 转换

ss << sth. 放进去

ss >> sth. 拿出来

```c++
int string_to_int(const string& s)
{
	stringstream ss;
	ss << s;
	int value;
	ss >> value;
	return value;
}

string s;
cin >> s;
cout << string_to_int(s) << endl;
```



```c++
#include <sstream>
using namespace std;

int main()
{
  stringstream ss;
	string s("403");
	int value = 404;

	// int to string
	// put int to ss
	ss << value;
	// output ss to string
	ss >> s;
	cout << s << endl;

	// string to int
	// put string to ss
	ss << s;
	// output ss to int
	ss >> value;
	cout << value << endl;
  
  return 0;
}
```

##### sstream复用的坑

`ss.clear()` 是状态位。比如上一次用到末尾时下一次再用就需要clear。

`ss.str("")`是清空内容以供复用。

##### sstream拼接多个字符串

https://blog.csdn.net/liitdar/article/details/82598039

###### 新的ss一次性拼接

```c++
ss << s1 << s2 << s3;
cout << ss.str() << endl;
```

###### 用过的ss在尾部添加

**需要用到clear来清楚“错误状态”。**

https://www.cnblogs.com/elenno/p/stringstream_clear.html

```c++
ss >> value;	// used
ss.clear();
ss << ", 403";	// add something after being used
cout << ss.str() << endl;
```

##### 基于stringstream的类型转换模板函数

```c++
template <class InType, class OutType>
OutType convert(InType val)
{
	static stringstream ss;
	ss.str("");	// empty the buffer
	ss.clear();	// clear the state
	ss << val;
	OutType res;
	ss >> res;
	return res;
}
```

### vector

```c++
vector<double> vec = { 0, 1, 2, 3, 4, 5 };
    for (int i = 6; i <= 10; i++)
        vec.push_back(i);

    vec.insert(vec.begin() + 4, 3.5);   // 在index=4前插入3.5
    vec.erase(vec.begin() + 10);    // 删除index=10（元素9被删掉了）
    vec.emplace(vec.begin() + 10, 9);   // 在index=10前插入9
    vec.emplace_back(11);

    vector<double> vec2(vec.begin() + 1, vec.begin() + 3); // [左, 右)


    for (auto& i : vec)
    {
        cout << i << ' ';
    }
    cout << endl;
    for (auto& i : vec2)
    {
        cout << i << ' ';
    }
```





### 正则表达式

```c++
#include <regex>
```

```c++
// 字符串能否完全匹配正则表达式
string s("subject");
regex e("sub.*");
cout << regex_match(s, e) << endl;
```

```c++
// 完全匹配；分组捕获
// 每组用()标识；0号永远是被匹配的整个字符串本身
// 不想捕获的分组这样标记： (?:pattern)
string s("ver10");
regex e(R"(ver(\d+))");
smatch sm;	// smatch 对象存储分组结果
if (regex_match(s, sm, e))	// 判断是否成功并存储结果
{
  cout << sm.size() << endl;
  // smatch对象可以直接输出；可以直接赋值给string对象
  for (auto& i : sm)	// for (int i = 0; i < sm.size(); i++)
	{
		cout << i << endl;
	}
}
```

```c++
// 搜索
// 搜索字符串中第一个能匹配的子串，存储到smatch对象中
smatch result;
regex_research(s, result, re);

smatch sm;
name = reg_match(state, "(My name is |I am )(\\w+)")[2];
sm = reg_match(state, "([1-9]\\d{0,3})[-.](1[0-2]|0?[1-9])[-.](3[01]|[12]\\d|0?[1-9])");
birth = Date(sm[1], sm[2], sm[3]);
phone = reg_match(state, "\\d{11}")[0];
email = reg_match(state, "\\w+.?@[a-zA-Z0-9_-]+(\\.[a-zA-Z0-9_-]+)+")[0];
```

```c++
// 替换
string s("this subject has a submarine");
regex e(R"(sub[\S]*)");
//regex_replace返回值即为替换后的字符串 
cout << regex_replace(s,e,"SUB") << endl;	// this SUB has a SUB
// regex_replace(s, re, s1)
// 第三个参数，替换上去的字符串里，可以使用一些特殊符号
// $& 代表re匹配的子串
// $1, $2 代表re匹配的第1/2个分组
string s("this subject has a submarine");
regex e(R"((sub)([\S]*))");
cout << regex_replace(s,e,"$1 and [$2]") << endl;	// this sub and [ject] has a sub and [marine]
```

http://tool.chinaz.com/regex/

贪婪与懒惰

### 判断一个字符是否是数字 isdigit

```c++
isdigit('3');	// 参数接收一个int；接收到EOF返回false可用于判断读入结束
```

