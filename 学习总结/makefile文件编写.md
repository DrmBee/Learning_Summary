# makefile 文件编写

#### 一、简介

makefile文件**用于管理和组织代码工程的编译和链接**，其不是可执行文件，其被make工具解析并完成相关动作。

makefile带来的好处就是-------“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件开发的效率。make是一个命令工具，是一个解释makefile中指令的命令工具，一般来说，大多数的IDE都有这个命令，比如：Delphi的make，Visual C++的nmake，Linux下GNU的make。可见，makefile都成为了一种在工程方面的编译方法。

#### 二、什么是makefile？

makefile文件保存了编译器和连接器的参数选项，还表述了所有源文件之间的关系（源代码文件需要的特定的包含文件，可执行文件要求包含的目标文件模块及库等）。创建程序（make程序）首先读取makefile文件，然后再激活编译器，汇编器，资源编译器和连接器以便产生最后的输出，最后输出并生成的通常是可执行文件。创建程序利用内置的推理规则来激活编译器，以便通过对特定CPP文件的编译来产生特定的OBJ文件。

```
理解：makefile文件相当于定义了一系列规则，make程序相当于对makefile文件进行解析解释，根据makefile文件中的内容命令编译器、汇编器和连接器进行相应的操作，最后生成可执行文件。
```

#### 三、make是如何工作的？

在默认的方式下，也就是我们只输入make命令。那么，

1. make会在当前目录下找名字叫“Makefile”或“makefile”的文件。
2. 如果找到，它会找文件中的第一个目标文件（target）,并把这个文件作为最终的目标文件。

#### 四、makefile里有什么？

makefile里主要包含了5个东西：显示规则、隐晦规则、变量定义、文件指示和注释。

1. 显示规则。显示规则说明了如何生成一个或多个的目标文件。这是由makefile的书写者明显写出，要生成的文件，文件的依赖文件，生成的命令。
2. 隐晦规则。由于我们的make有自动推动的功能，所以隐晦的规则可以让我们比较粗略地简略地书写书写makefile，这是由make所支持的。
3. 变量的定义。在makefile中我们要定义一系列的变量，变量一般都是字符串，这个有点像C语言的宏，当makefile被执行时，其中的变量都会被扩展到相应的引用位置上。
4. 文件指示。其中包括了三个部分，一个是在一个makefile中引用另一个makefile，就像C语言中的include一样；另一个是指根据某些情况指定makefile中的有效部分，就像C语言中的预编译#if一样；还有就是定义一个多行的命令。
5. 注释。makefile文件中只有行注释，其注释用“#”字符。

最后，在makefile中的命令，必须要以tab键开始。



#### 五、makefile的规则：

1. 一个规则是由**目标、条件（又称为先决条件）和命令**所组成的。命令与条件之间的表达的是依赖关系（因为这种关系，我们有时候会将条件与依赖不加以区分），这种依赖关系指明在构建目标之前，必须保证条件先满足（或已经构建好了条件）。
2. 所谓**目标**就是我们**想要生成的文件**，然后要生成文件**所需要的资源**称为**条件**，将**条件变成目标的“动作”**称为命令。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717095727461.jpg)

3. 当条件是目标时，必须先把目标构建出来。（依赖关系）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717100016886.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

4. 一个makefile中式存在多个规则的，在编写规则时应先把各文件的依赖关系想清楚，需要表达什么样的依赖关系。例如下图，我们要生成最终的目标，需要依赖的是目标1，而目标1还没有被构建出来，这时候会先去构建目标1，执行第二个规则，这时候目标2还没有构建，又去执行第三个规则，这种依赖关系与函数的**递归调用**很类似。

   需要注意的是：**如果第一个规则中的条件不存在，或者该条件不与其他规则有联系，那么规则中的第一个目标将被视为默认目标，其他目标（规则）就不会被执行。**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717100320933.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)``

#### 六、伪目标（又为假目标）

1. 一般来说，make的最终目标是makefile中的第一个目标，而其他目标一般是因为要生成这个最终目标而附带出来的。这是make的默认行为。当然，你的makefile是由多个目标组成，你可以通过 `make+目标`，让其完成你所指定的目标。make可以指定所有makefile中的目标，那么也包括 `伪目标`，于是我们可以根据这种性质来让我们的makefile根据指定的不同的目标来完成不同的事。

   所谓伪目标就是**没有条件，只有目标和命令**，然后**在目标前面加了 `.PHONY` 关键字声明**。它不代表一个真正的文件名，是一个 ”虚假“ 的目标。有时我们也可以将一个 `伪目标称为标签`。

   格式：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190719092029866.PNG)

2. 伪目标的用途是什么，下面先举个例子：

   比如在当前目录下有个hello.c文件，在makefile编写一个规则编译这个.c文件成为可执行文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190716212217800.PNG)

执行make命令，在当前目录找到makefile，然后执行makefile里面的命令。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190716212414801.PNG)

通过makefile生成了一个可执行文件hello, 如果我们这时候想把这个可执行文件删除，只需执行linux命令 `rm hello`删除该文件。这样是可行的，但当一个工程变大，生成的中间文件很多，还要一个一个删除就很浪费事件了。这时候，就需要再加一个规则，在这个规则 中添加删除命令。然后在命令行执行 `make+标签`命令，即可删除可执行文件hello, 如

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190716213250811.PNG)

这里的 `clean`是一个目标，在执行make时可以指定 `clean`这个目标来执行其所在规则定义的命令，也就是执行 `rm hello`这个操作。也就是说，我们使用伪目标这种规则所定义的命令不是去创建目标文件，而是使用 `make`指定具体的目标来执行一些特定的命令。这里的 clean 还不是真正的”伪目标“，当在同一个目标下存在一个与该目标名一样的文件，这时候再去执行make clean时，会提示该clean已经是最新了，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718090602359.png)

为什么会出现这种情况呢？其实，每次在执行make命令时，make都会去检查依赖关系中相关文件的时间戳，如果文件中的相关文件的时间戳大于目标文件的时间戳，也就是说依赖比目标新，则说明依赖文件有改变，那么需要运行规则当中的命令去重新构建目标。因为make工具将clean目标当成一个文件，且在当前目录下找到了这个文件，加上这个规则里没有依赖，所以，当要求make构建clean目标时，它就会认为clean是最新的了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727112434789.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

3. 在实际工作中会出现当前目录有的文件名会出现与目标名一样的情况，那么如何处理呢？

   只需要在目标前面加上 `.PHONY`关键字，用来说明clean这个是伪目标，如果当前目标下有跟目标一样的文件，还可以去执行 `make clean`命令了。**对于伪目标，就是只将它作为一个”命令“使用，而不是一个”文件“**，所以每一次make这个伪目标，其所在的规则中的命令都会被执行。如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190716230855559.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

4. GNU常用的伪目标名称：

   all : 这个伪目标是所有目标的目标，其功能一般是编译所有的目标。

   clean : 这个伪目标功能是删除所有被make创建的文件。

   install : 这个伪目标功能是安装编译好的程序，其实就是把目标执行文件拷贝到指定的目标中去。

   print : 这个伪目标的功能是列出改变过的源文件。

   tar : 这个伪目标功能是把源程序打包备份。也就是一个tar文件。

   dist : 这个伪目标功能是创建一个压缩文件，一般是把tar文件压缩成Z文件，或是gz文件。

   TAGS : 这个伪目标功能是更新所有的目标，以备完整地重编译使用。

   check 和 test : 这两个伪目标一般用来测试makefile的流程。

   （打包就是把若干文件或文件夹放到一个tar文件中，但是不会压缩文件大小。
   压缩就是在打包的基础上压缩文件的大小。）

#### 七、扩展：

**echo命令** ：该命令是Bash shell的一个命令，其功能用于打印字符串到终端上。在终端输入”echo hello world“,它会原样输出hello world这个字符串。这个命令用于在编译或者调试过程中输出一些信息。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717153333639.PNG)

在makefile中，先不加echo命令的情况，如下图，make执行命令行并把命令行的内容进行输出。我们称之为”回显“，就好像我们输入命令执行一样。这样make后生成了一个可执行文件hello。需要注意的是，由于在makefile中”`$`“具有特殊含义，因此如果想采用echo输出”`$`“，则必须用两个连着的”`$`“.还有，如果想显示”`$@`“符号，需要在前面加一个脱字符”`\`“。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717162550992.PNG)

**如果前面加一个”@“符号，这时候在执行make命令时，这些命令执行时不会在终端上显示**，生成了一个可执行文件hello。如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717155111938.PNG)

如果只加了echo，然后再执行，如下图，这时候会把makefile文件中命令显示出来： `echo gcc hello.c -o hello ` 和 `gcc hello.c -o hello` , 但不会去生成可执行文件hello。 echo命令就只是用来显示后面的内容。

（注意：echo前面必须只有tab键的长度，且至少有一个tab,不能用空格代替，否则编译错误为： `*** missing separator. Stop`）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717163015574.PNG)

当通过上面的图片显示可以看到，显示了`echo gcc hello.c -o hello`的内容和`gcc hello.c -o hello`的内容，这样出现的内容就会很多余。这时候，在echo前面加个“@”符号，就不会显示echo行的内容，只显示echo后面的内容，而且如果echo后的内容是命令，make也不会去执行这个命令。如下图，这样我们可以使用`@echo+要显示的内容`，来显示一些调试信息。
		通过一个例子来说明之前说写的内容：在一个目录里写了三个c文件，`main.c`、`1.c`和`2.c`，然后使用makefile文件去构建可执行文件`main`。（`#`号表示注释，类似c语言的`//`）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717193845312.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

执行make命令，从下图可以看到，写的编译信息通过终端显示了出来。通过编译信息显示的顺序也可以知道，在make执行第一条规则时，由于条件`main.o`、`1.o`、`2.o`三个先决条件没有被构建，所以按从左到右的顺序去构建这三个条件，最后构建这三个条件后，执行第一个规则，将这三个条件去链接成可执行文件main。



![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717193737393.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

 使用**ls -a**命令可以看到，make结束后产生了一些中间文件：main.o、1.c、2.c。通过执行伪目标	**make clean**去清除这些中间文件。

#### 八、makefile变量

当工程文件变多，makefile文件会写的很庞大，这时就需要使用变量来使makefile内容变得简洁、具有可维护性。比如可以将上个图中的makefile改成下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717203914338.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

makefile中的变量只代表文本数据（字符串）

makefile中的变量名规则：

- 变量名可以包含字符、数字、下划线
- 不能包含”：“、”#“、”=“或” “
- 变量名大小写敏感

对变量的引用采用 `$(变量名)`或者 `${变量名}`这两种方式。

**变量可分为三种：1.用户自定义变量；2.预定义变量； 3.自动变量**

1. 用户自定义变量：上面的OBJ、OBJS就是用户自定义的变量
2. 预定义变量：上面的CC、RM就是预定义变量，该变量的值可重新被改变，如果不改变的话，保持默认值。上图又可改成：

（注意：CC、RM必须为大写）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718093351281.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

make输出，从下图可以看到， `$(CC)`替换成 `cc` , `$(RM)` 替换成 `rm -f` :

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190717205216850.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

makefile 常见的预定义变量：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727163133616.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190728102016393.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

**ar**：建立、 修改、 提取归档文件。 归档文件是包含多个文件内容的一个大文件， 其结构保证了可以恢复原始文件内容

**GCC的执行过程：**

- 调用CPP进行预处理，对源代码文件中的文件包含（include）、预编译语句（如宏定义define等）进行分析；
- 调用ccl进行编译，生成.s为后缀的目标文件；
- 调用as进行汇编，汇编语言文件经过预编译和汇编之后都生成以.o为后缀的目标文件；
- 调用ld进行链接，所有的目标文件被安排在可执行程序中的恰当位置。同时，该程序所调用到的库文件也从各自所在的档案库中链接到合适的地方。

3. 自动变量：所谓自动化变量，就是这种变量会把模式中所定义的一系列的文件自动地挨个取出，直到所有的符合模式的文件都取完了。这种自动化变量直接出现在规则的命令中。上图又可改成：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718093222727.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

常用的自动变量：

-  `$@` : 代表规则中的目标文件名。如果目标是一个文档（Linux中，一般称.a文件为文档），那么它代表这个文档的文件名。在多目标的模式规则中，它代表的是哪个触发规则被执行的目标文件名（当目标有多个时）。如下图，第一个规则中 `$@` 代表libtest.a文件，第二个规则中 `$@` 代表 1.o、2.o文件。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727164831981.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

-  `$%` : 规则的目标文件是一个静态库文件时，代表静态库的一个成员名。例如，规则的目标是”`foo.a(bar.o)`“,那么，” `$%`“的值就为”`bar.o`“ , "`$@`"的值为”`foo.a`“。如果目标不是函数库文件，其值为空，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727165446240.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

-  `$<` : 规则的第一个依赖文件名。如果是隐含规则，则它代表通过目标指定的第一个依赖文件。
- `$？` : 所有比目标文件更新的依赖文件列表，空格分割。如果目标是静态库文件名，代表的是库成员（.o文件）的更新情况。
- `$^` : 规则的所有依赖文件列表，使用空格分隔（当依赖有多个时）。如果目标是静态库文件名，它所代表的只能是所有库成员（.o文件）名。一个文件可重复的出现在目标的依赖中，变量 `$^`只记录它的一次引用情况。就是说变量”`$^`“会去掉重复的依赖文件。如下图，在规则1中目标是静态库，所以$^代表该静态库的成员1.o、2.o

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727170129597.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

- `$+`：类似“`$^`”，但是它保留了依赖文件中重复出现的文件。主要用在程序链接时，库的交叉引用场合.

**四种变量定义（赋值）：**

GUN make中，一个变量的定义有四种方式。我们把这四种方式定义的变量可以看作变量的四种不同风格。变量的这四种不同的风格的区别在于：定义方式；展开时机

1.递归展开式变量（递归赋值）---- 影响的变量可能会是多个

这一类型的变量的定义是通过 `=`或者使用指示符 `define`定义的变量。

比如下图，整个变量的替换过程是这样的：首先 `$(name1)` 被替换为“`$(name2)”`，接下来“`$(name2)`”被替换为“`$(name3)`”，最后“`$(name3)`”被替换为“`小明`”。整个替换的过程是在执行`“echo $(name1)”`命令行时进行的。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718111932949.PNG)

其优点是：这种类型变量的定义时，**可以引用其它的之前没有定义的变量（可能在后续部分定义，或者是通过 make 的命令行选项传递的变量）。**
但这种递归展开式又有缺点，如下图：使用此风格的变量定义，可能会由于出现变量的递归定义而导致 make 陷入到无限的变量展开过程中，最终使 make 执行失败。 它将会导致 make 进入对变量name1、name2、name3的无限展开过程中去（这种定义就是变量的递归定义）。展开过程将是套嵌的、不能终止的（在发生这种情况时， make 会提示错误信息并结束）。一般在书写 Makefile 时，使用这种追加变量值的方法也很少使用（也不是我们推荐的方式）。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718112020630.PNG)

2. 直接展开式变量(简单赋值):对“递归展开式”的优化

这一类型的变量的定义是通过 `:=` 定义的变量。
如下图，变量值中对另外变量的引用或者函数的引用在定义时被展开（对变量进行替换）。所以在变量被定义以后就是一个实际所需要定义的文本串，其中不再包含任何对其它变量的引用。这种定义变量的方式更适合在大的编程项目中使用，
因为它更像我们一般的编程语言。需要注意的是：此风格变量在定义时就完成了对所引用的变量的展开，因此它不能实现对其后定义变量的引用。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718112503334.PNG)

如果在定义name2时前面没有name1的定义，那么name2的值是空的，如下图，由于在变量“name1”的定义出现在“name2”定义之后。因此在“name2”的定义中，“name2”的值为空。这一点也是直接展开式与递归展开式的不同点。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718112938210.PNG)

如果将name2的定义改成递归展开式，那么name2的值是不为空的，如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718113500307.PNG)

3. 条件赋值(?=)：意思就是如果变量在前面没有定义，使用赋值符号中的值定义该变量。如果变量在前面已经定义，对该变量的赋值无效。比如下图，由于name1之前已经定义了，所以第二次定义name1无效。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718114259123.PNG)

如果前面name1未定义，这将name1赋值为"小明"，如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718114611709.PNG)

4. 追加赋值(+=)：意思就是在原来已经被赋值的变量上在附加内容。如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718115230595.PNG)

**隐式规则：**

所谓隐式，就是有些中间的规则不用写出来，make 会在自己的“隐含规则”库中寻找可以用的规则，如果找到，那么就会使用。如果找不到，那么就会报错。在之前所写的规则都是显式的。在下面例子中，make 调用的隐含规则是，把`[.o]`的目标的依赖文件置成`[.c]`，并使用 默认规则“`$(CC) $(CFLAGS) $(CPPFLAGS) -c -o`”来生成`[.o]`的目标，这样就不用再写将.c文件编译成.o的规则了。三个.o文件构建后，因为该规则没有命令，然后就不会去生成可执行文件a.out。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718171645516.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

这里需要注意的是，使用隐式规则是会使用到一些变量（比如预定义变量），这里的CFLAGS、CPPFALGS是无默认值的，如果不给这些变量赋值，这些值是”空“的。如果给这些赋予一些参数，如下图，这样在执行默认规则时就会显示出来了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718172553810.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

在规则中添加生成可执行文件`a.out`的命令,如下图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718173558378.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

**模式规则：**

模式规则中，至少在规则的目标定义中要包含"`%`"，否则，就是一般的规则。目标中的"`%`“定义表示对文件名的匹配，”`%`“表示长度任意的非空字符串。例如：”`%.c`“表示以”.c“结尾的文件名 ， 而"`s%.c`"则表示以"s"开头、”`.c`"结尾的文件名 。从下图可以看到，main.o : main.c 、1.o : 1.c、2.o : 2.c 三个规则都相同，可以使用模式规则把这三个规则写成%.o : %.c。
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019071816254495.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

这里需要注意的是，前面`a.out : $(OBJS)`原来是隐式规则，然后这里使用了模式规则，从`.c`文件生成`.o`文件的规则不在按照隐式规则的规定去生成，而是按照模式规则的规定。如下图，执行make后按照模式规则里面的命令去执行。

（在使用gcc编程时，没有指定输入可执行文件名，默认生成可执行文件a.out文件。执行时必须键入命令 ./a.out，即要带上扩展名，如果键入./a 则不正确，因为它寻找a这个文件，而不是a.out这个文件。）

**make函数（以下只列举两个函数，更多信息请参考GNU make手册）**

1. 文件名处理函数：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718194354438.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

测试：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718194917150.PNG)

2. 文本处理函数：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718194644118.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

测试：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190718195340190.PNG)

两个函数结合：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019071819550367.PNG)

3. shell函数：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727190907523.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTM1ODEyMDc=,size_16,color_FFFFFF,t_70)

测试： 使用shell命令打印当前目录的文件属性

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727191116210.PNG)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190727191218376.PNG)



https://blog.csdn.net/haoel/article/details/2886

