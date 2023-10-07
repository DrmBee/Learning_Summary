# C++学习

书籍：C++ Primer Plus

## 第1章 预备知识

### 1.1 C++简介

C++融合了3种不同的编程方式：

- C语言代表的过程性语言
- C++在C语言基础上添加的类代表的面向对象语言
- C++模板支持的泛型编程。

### 1.2 C++简史

计算机语言一般要处理两个概念：数据和算法。

过程性语言强调的是编程的算法方面，而面向对象强调的则是数据。

###### 1.2.3 面向对象编程

通常，类规定了可使用哪些数据来表示对象以及可以对这些数据执行哪些操作。

OOP程序设计方法首先设计类，它们准确地表示了程序要处理的东西，然后便可以设计一个使用这些类的对象的程序。从低级组织（如类）到高级组织（如程序）的处理过程叫做自下向上的编程。

###### 1.2.4 C++和泛型编程

泛型编程是C++支持的另一种编程模式。它与OOP的目标相同，即使重用代码和抽象通用概念的技术更简单。不过OOP强调的是编程的数据方面，而泛型编程强调的是独立于特定数据类型。

术语泛型指的是创建独立于类型的代码。泛型编程需要对语言进行扩展，以便可以只编写一个泛型（即不是特定类型的）函数，并将其用于各种实际类型。C++模板提供了完成这种任务的机制。

#### 1.4 程序创建的技巧

```
cin.get()
cin.get()
return 0;
/*
cin.get()语句读取下一次键击，因此上述语句让程序等待，直到按下了Enter键（在按下Enter键之前，键击将不被发送给程序，因此按其它键都不管用）。如果程序在其常规输入后留下一个没有被处理的键击，则第二条语句是必须的。
*/
```

## 第2章 开始学习C++

#### 2.3 其它C++语句

###### 2.3.3 类简介

类是用户定义的一种数据类型。要定义类，需要描述它能够表示什么信息和可对数据执行哪些操作。



## 第3章 处理数据

#### 3.1 简单变量

sizeof运算符：返回一个对象或者类型所占的内存字节数。

头文件climits定义了符号常量来表示类型的限制。

```
cin.get()
cout.put()
```

###### 3.4.4 类型转换

1. 以{}方式初始化时进行的转换（C++11）

C++11将使用大括号的初始化称为列表初始化（list-initialization），因为这种初始化常用于复杂的数据类型提供值列表。列表初始化不允许缩窄（narrowing），即变量的类型可能无法表示赋给它的值。例如，不允许将浮点型转换为整型。在不同的整型之间转换或将整型转换为浮点型可能被允许，条件是编译器知道目标变量能够正确地存储赋给它的值。

2. 强制类型转换

强制类型转换不会改变变量本身，而是创建一个新的、指定类型的值，可以在表达式中使用这个值。

3. C++11中的auto声明

C++11新增了一个工具，让编译器能够根据初始值的类型推断变量的类型。在初始化声明中，如果使用关键字auto，而不指定变量的类型，编译器将把变量的类型设置成与初始值相同。

## 第4章 复合类型

###### 4.2.3 字符串输入

cin使用空白（空格、制表符和换行符）来确定字符串的结束位置。

###### 4.2.4 每次读取一行字符串输入

采用面向行而不是面向单词的方法。

istream中的类提供了一些面向行的类成员函数：getline()和get()。这两个函数都读取一行输入，直到到达换行符。然而，随后getline()将丢弃换行符，而get()将换行符保留在输入序列中。

- cin.getline(array_name，读取的字符数)
- cin.get(array_name，读取的字符数)
  - 使用不带任何参数的cin.get()调用可读取下一个字符（即使是换行符）

#### 4.3 string类简介

###### 4.3.1 C++11字符串初始化

```
//将一行输入读取到string对象中
getline(cin,str)
```



###### 4.3.2 赋值、拼接和附加



###### 4.3.3 string类的其他操作

```
#include<cstring>
strcpy()
strcat()
strncpy()
strncat()
# 确定字符串中字符数的方法：
int len1 = str.size()
int len2 = strlen(charr)
```

###### 4.3.4 string类 I/O

#### 4.4 结构

#### 4.5 共用体

#### 4.6 枚举

#### 4.7 指针

###### 4.7.4 使用new来分配内存

`typeName *pointer_name = new typeName;`

```
//下面这种方式是危险的，因为并不知道p的地址到底在哪里
//尽量在使用引用之前对指针进行初始化！
int *p;
*p = 23333333;
//方式1
int *pn = new int;
//方式2
int higgens;
int *pt = &higgens;
```

###### 4.7.5 使用delete释放内存

`delete pointer_name;`

内存泄漏（memory leak）

参考：https://blog.csdn.net/stanjiang2010/article/details/5386647

**内存泄漏不是系统无法收回那片内存，而是自己的应用程序无法使用那片内存。**

注意：使用delete的关键在于，将它用于new分配的内存

###### 4.7.6 使用new来创建动态数组

1. 为数组分配内存的通用格式：

`type_name *poingter_name = new type_name[num_elements];`

使用new运算符可以确保内存块足以存储num_elements个类型为type_name的元素。

2. 使用动态数组

- 可以把指针当作数组名使用；

- 删除动态数组

`delete [] pointer_name`

```
int *p = new int;
delete p;
//动态数组
int *p = new int [10];
delete [] p;
//数组指针
int (*p)[n];
//指针数组
int *p[n];
```



#### 4.8 指针、数组和指针算术

###### 4.8.3 指针和字符串

在cout和多数C++表达式中，char数组名、char指针以及用引号括起来的字符串常量都被解释为字符串第一个字符的地址。

```
char s[20]="ni hao";
char *p = s;
cout<<p<<endl;
cout<<&s<<endl;
cout<<*p<<endl;
cout<<(int *)p<<endl;
/*
ni hao
0x151bbff770
n
0x151bbff770
*/
```

注意：

- 在字符串中，指针指向一个字符串时，输出指针的时候，输出的是整个字符串的值，这和普通变量的指针输出有所不同，普通指针输出的是地址；而获取字符串的地址是`(int *)char_pointer_name`。
- 也就是说：一般来说，如果给cout提供一个指针，它将打印地址。但如果指针的类型为char*，则cout将显示指向的字符串。如果要显示的是字符串的地址，则必须将这种指针强制转换为另一种指针类型。

- 不要使用字符串常量或未初始化的指针来接受输入。
- 在将字符串读入程序时，应使用已分配的内存地址。该地址可以是数组名，也可以是使用new初始化过的指针。

###### 4.8.4 使用new创建动态结构

###### 4.8.5 自动存储、静态存储和动态存储

C++有3种管理数据内存的方式：自动存储、静态存储和动态存储。

####  $\textcolor{red}{4.9 类型组合}$

```
int a=1,b=2,c=3;
int *a[3] = {&a,&b,&c};
const int ** pa = a;
```

#### 4.10 数组的替代品

模板类vector和array是数组的替代品

###### 4.10.1 模板类vector

模板类vector类似于string类，也是一种动态数组。

可以在运行阶段设置vector对象的长度，可在末尾附加新数据，还可以在中间插入新数据。

一般而言，下面的声明创建一个名为vt的vector对象，它可存储n_elem个类型为typeName的元素：

`#include<vector>`

`using namespace std;`

`vector<typeName> vt(n_elem); `

其中参数n_elem可以是整型常量，也可以是整型变量。

###### 4.10.2 模板类array

array对象的长度是固定的，使用栈，而不是自由存储区，因此其效率与数组相同。

```
#include<array>
using namespace std;
/*
下面的声明创建一个名为arr的array对象，它包含n_elem个类型为typename的元素。与创建vector对象不同的是，n_elem不能是变量。
*/
array<typeName, n_elem> arr;
```



###### 4.10.3 比较数组、vector对象和array对象

- array对象和数组存储在相同的内存区域（即栈）种，而vector对象存储在另一个区域（自由存储区或栈）中；

- 可以将一个array对象赋给另一个array对象；而对于数组，必须逐元素复制数据。
- 使用vector和array对象的成员函数at()。

## 第5章 循环和关系表达式

###### 5.1.15 比较string类字符串

```
/*
可以直接和常量字符串进行比较
*/
string word="nihao";
if(word == "nihao"){
    ...
}
```

```
clock()：返回程序开始执行后所用的系统时间。
         存在两个问题：- clock()返回时间的单位不一定是秒；
                     - 该函数的返回类型在某些系统上可能是long，在另一些系统上可能是unsigned long或其它类型。
          解决方法：使用头文件ctime
                  - 其中定义了一个符号常量——CLOCKS_PER_SEC，该常量等于每秒钟包含的系统时间单位数。因此，将系统时间除以这个值，可以得到秒数。
                  - ctime将clock_t作为clock()返回类型的别名，这意味着可以将变量声明为clock_t类型，编译器将把它转换为long、unsigned int或适合系统的其它类型。
```

#### 5.4 基于范围的for循环（C++11）

```
//例子1：不能修改数组的元素
double prices[5] = {4.99, 10.99, 6.87, 7.99, 8.49}
for(double x : prices)
	cout<<x<<std::endl;
//例子2：可以修改数组的元素
for(double &x : prices)
	x = x * 0.8;
//例子3：可结合使用基于范围的for循环和初始化列表
for(int x : {3,5,2,8,6})
	cout << x << " "
cout<<"\n";
```

#### 5.5 循环和文本输入

###### 5.5.1 使用原始的cin进行输入



###### 5.5.2 使用cin.get(char)进行补救

cin.get(ch)读取输入中的下一个字符（即使它是空格），并将其赋给变量ch。

###### 5.5.3 使用哪一个cin.get()

```
//C++允许函数重载
cin.get()
cin.get(ch)
cin.get(name,Arsize)
```

###### 5.5.4 文件尾条件

检测到EOF后，cin将两位（eofbit和failbit）都设置为1.可以通过成员函数eof()来查看eofbit是否被设置；如果检测到EOF，则cin.eof()将返回bool值true，否则返回false。fail()函数同理。

###### 5.5.5 另一个cin.get()版本



#### 5.6 嵌套循环和二维数组

C++常用的做法，将一个指针数组初始化为一组字符串常量。

```
const char *cities[4] = 
{
	"shanghai",
	"beijing",
	"siyang",
	"nanjing"
}
for(int i = 0; i < 4; i++){
	cout<<cities[i]<<endl;//注意输出的是字符串的值。
}

```

## 第6章 分支语句和逻辑运算符

#### 6.3 字符函数库cctype

C++从C语言继承了一个与字符相关的、非常方便的函数软件包，它可以简化诸如确定字符是否为大写字母、数字、标点符号等工作，这些函数的原型是在头文件cctype中定义的。

- isalpha(ch)
- ispunct(ch)

![image-20231001105427763](.\C++学习.assets\image-20231001105427763.png)

#### 6.7 读取数字的循环

```
int main()
{
	int num[10];
	for(int i = 0;i<10;i++){
		while(!(cin>>num[i])){
			cin.clear();
			while(cin.get()!='\n'){
				continue;
			}
			cout<<"please input a number:";
		}
	}
}
/*
1.cin.clear()的功能
2.cin.get()的功能，是从缓冲池里获得？还是从输入中获得？
*/
```

#### 6.8 简单文件输入/输出

###### 6.8.1 文本I/O和文本文件

使用cin进行输入时，程序将输入视为一系列的字节，其中每个字节都被解释为字符编码。不管目标数据类型是什么，输入一开始都是字符数据——文本数据 。然后，cin对象负责将文本转换为其它类型。

###### 6.8.2 写入到文本文件中

```
#include<fstream>
ofstream outFile;
ofstream fout;
outFile.open("info.txt");
...
/***********************/
double wt = 125.8;
outFile << wt;	//write a number to info.txt
char line[10] = "nihao";
fout << line << endl;	//write a line of text
/***********************/
outFile.close();
```

- outFile可以使用cout可使用的任何方法。

- 在程序运行之前，文件info.txt并不存在。在这种情况下，方法open()将创建一个名为info.txt的文件。如果在此运行该程序，文件info.txt将存在，此时默认情况下，open()将首先截断该文件，即将其长度截短到零——丢弃其原有的内容，然后将新的输出加入到该文件中。

###### 6.8.3 读取文本文件

```
#include<fstream>
ifstream inFile;
ifstream fin;
inFile.open("bowling.txt");

/***************************/
double wt;
inFile >> wt;	//read a number from bowling.txt
char line[81];
fin.getline(line,81);//read a line of text
/***************************/

```

- 所有可用于cin的操作和方法都可用于ifstream对象。
- 检测文件是否被成功打开的首先方法是使用方法is_open()

```
inFile.open("bowling.txt");
if(!inFile.is_open()){
	exit(EXIT_FAILURE);
}
/*
如果文件被成功打开，方法is_open()将返回true
*/
```

$\textcolor{red}{good() and bad()}$



## 第7章 函数——C++的编程模块

```
void fun(const int a[]);
//表示仅在被调函数中是只读的，主调函数仍然可以修改
```

###### 7.3.5 指针和const

两种方式：

- 让指针指向一个常量对象，防止使用该指针来修改所指向的值；
- 将指针本身声明为常量，防止改变指针所指向的位置。

```
int age = 39;
const int *pt = &age;
//该声明指出pt指向一个const int，因此不能使用指针来修改这个值
//该声明中的const只能防止修改pt指向的值，而不能防止修改pt的值，也就是说可以将一个新地址赋给pt
//pt的声明并不意味着它指向的值实际上就是一个常量，而只是意味着对pt而言，这个值是常量
```

$\textcolor{red}{注意：}$

- C++禁止将const的地址赋给非const指针。
- page 222

```
int sloth = 3;
int *const finger = &sloth;
//该声明中使得finger只能指向sloth，但是允许使用finger来修改sloth的值，注意必须在创建的时候就对其进行初始化操作。
```

#### 7.4 函数和二维数组



#### 7.7 函数和string对象



#### 7.8 函数与array对象





#### 7.10 函数指针

与数据项相似，函数也有地址。函数的地址是存储其机器语言代码的内存的开始地址。

###### 7.10.1 函数指针的基础知识

1. 获取函数的地址

   函数名就是函数的地址

2. 声明函数指针

   ```
   double pam(int);
   double (*pf)(int);//括号不可以丢弃
   pf = pam;
   ```

3. 使用指针来调用函数

   ```
   double pam(int);
   double (*pf)(int);//括号不可以丢弃
   pf = pam;//注意pam本身就表示一个地址，所以不需要使用&运算符
   double y = pam(4);
   double x = (*pf)(5);
   double x = pf(5);//两种方式都是允许的
   ```

   

###### 7.10.3 深入探讨函数指针

$\textcolor{red}{page 244}$



## 第8章 函数探幽

#### 8.1 C++内联函数



#### 8.2 引用变量

引用是已定义的变量的别名，引用变量的主要用途是用作函数的形参。通过将引用变量用作参数，函数将使用原始数据，而不是其副本。

###### 8.2.1 创建引用变量

```
int rats;
int & rodents = rats;
int * prats = &rats;
```

引用与指针的区别：

- 引用在声明时必须将其初始化，而不能像指针那样，先声明再赋值。这和const指针类似，即必须在创建的时候进行初始化。

###### 8.2.2 将引用用作函数参数

引用经常被用作函数参数，使得函数中的变量名成为调用程序中的变量的别名，这种传递参数的方法称为按引用传递。按引用传递允许被调用的函数能够访问调用函数中的变量。

```
void swap(int &a,int &b)
{
	int temp;
	temp = a;
	a = b;
	b = temp;
}
int main()
{
	int a=1,b=2;
	swap(a,b);
}
```

###### 8.2.3 引用的属性和特别之处

```
double refcube(double &a);
double z = refcube(x+3.0);//invalid
```

###### 8.2.4 将引用用于结构

```
struct free_throws
{
	std::string name;
	int made;
	int attempts;
	float percent;
}
void set_pc(free_throws &ft);
void display(const free_throws &ft);//不希望函数修改传入的结构
free_throws & accumulate(free_throws &target, const free_throws &sourse);
//返回引用类型【是不是得注意：不能够返回函数中的临时变量？】

```

###### 8.2.5 将引用用于类对象

###### 8.2.6 对象、继承和引用

例如，ostream是基类（因为ofstream是建立在他的基础之上的），而ofstream是派生类（因为它是从ostream派生而来的）。派生类继承了基类的方法，这意味着ofstream对象可以使用基类的特性。

继承的另一个特征是，**基类引用可以指向派生类对象，而无需进行强制类型转换。**

可以定义一个接受基类引用作为参数的函数，调用该函数时，可以将基类对象作为参数，也可以将派生类对象作为参数。例如，参数类型为ostream &的函数可以接受ostream对象或声明的ofstream对象作为参数。

#### 8.3 默认参数

必须通过函数原型。

```
char * left(const char *str, int n=1);
```

对于带参数列表的函数，必须从右向左添加默认值。也就是说，要为某个参数设置默认值，则必须为它右边的所有参数提供默认值。

#### 8.4 函数重载

函数多态（函数重载）能够使用多个同名的函数。

是函数参数的类型不同实现重载，而不是函数类型使得可以对函数进行重载。

$\textcolor{red}{引用折叠}$

https://zhuanlan.zhihu.com/p/50816420

#### 8.5 函数模板



















## 第9章 内存模型和名称空间







## 第10章 对象和类





## 第11章 使用类





## 第12章 类和动态内存分配





## 第13章 类继承





## 第14章 C++中的代码重用





## 第15章 友元、异常和其他





## 第16章 string类和标准模板库





## 第17章 输入、输出和文件





## 第18章 探讨C++新标准





iostream

ctime

cmath

cstring

climits

vector

array

cctype

fstream

cstdlib





**auto**

引用、继承、多态、模板

https://zhuanlan.zhihu.com/p/409035433
