# cherno c++教程笔记


<!--more-->

##  c++文件如何工作

+ **源文件(.cpp)->每一个源文件通过(编译器)生成对应(.obj)->.obj文件通过（链接器）链接生成(.exe)**

 

+ **编译器怎么工作**：编译器将源文件.cpp（文本）变为中继格式.obj

1. 预处理：#include（就是简单地复制粘贴）

   ​              	#define A B(搜索A替换成B)

	#if … #endif(根据特定条件包含或剔除代码)
	
	( 预处理结束后，c++代码开始被编译成机器码)

2. 每一个.cpp文件在这里称作编译单元（translation unity）（包括include的内容）

3. 编译器优化，包括常数折叠

4. 调用函数时，编译器会生成call以便辅助链接器链接

+ **链接器怎样工作**：找到每个符号和函数的位置并把它们连接到一起
  + 一种常见的链接错误：在文件1中定义了函数A，在定义的主体部分调用了B函数，而B在定义在文件2中，文件1中并没有B的声明。这时链接器会由于找不到B而报错，即使在文件1中A没有被调用也会报错（因为链接器不确定以后A是否会被其他文件调用）。此时只需在A前加上static代表这个函数A只针对所在文件定义。
  + 链接的对象必S须明确，函数的参数个数，返回类型都必须明确
  + 链接的对象不能重复，在函数定义时，用static修饰，可以避免#include造成的链接对象重复造成的链接错误，或者用inline修饰，即将函数的主体拿过来取代调用也可以避免（内联函数），或者将所用函数只放在一个翻译单元，其他只inSclude它的声明

+ **函数的声明与定义**：声明就像是对编译器的承诺，而定义则是在链接时所必备的函数内容，包含函数的主体部分

  

------

------



## c++基础

> 数据类型，关键字等以及循环，while及for循环的基本用法及结构含义; 条件，if语句基本用法; 控制流，continue，break，return的基本用法等省略。友链：https://blog.csdn.net/Augenstern_QXL/article/details/117249021

### 头文件

+ 包含各种函数的声明，以便随时调用函数

+ #pragma once头文件保护符，以防止头文件被多次调用在一个链接文件中

+ 头文件保护符还有另一种，即使用#ifndef…#define…#endif

+ 方括号和圆括号只不过代表引入的头文件是否是绝对路径

### 指针

+ 计算机和内存打交道，内存即是一切，而指针对于管理和操纵内存极其重要，指针是一个数字，它存储一个内存地址。

+ 指针定义的格式是变量类型后加*，而指针本身也是一个变量，所以我们可以对指针取地址然后赋值给另一个指针（双指针）这样就无限套娃；

+ 用*ptr=10这样的格式可以对指针指向的变量进行操作，但需要注意指针变量声明时设定的指针类型是啥（虽然说实质上就是一个数，但是指定此规则），否则编译器不知道如何将值写入相应内存。

  ```c++
  int main() 
  {
  	int a = 10;
  	int* p = &a;
  	int** p2 = &p;
  	std::cout << *p << std::endl;
  	std::cout << p << std::endl;
  	std::cout << *p2 << std::endl;
  	return 0;
  	//结果
  	//10
  	//00B6F934
  	//00B6F934
  };
  ```

+ 用new开辟内存到堆上并赋值给指针，要记得结束后delete释放空间，内存后面会详细讲。

------



### 引用

+ 引用本质上和指针是一样的(可以看做指针常量)，是基于指针的一种语法糖，来使代码易读易写。

+ 引用是对已存在的变量进行引用，引用本身并不是一个新的变量，不占内存。

+ 引用传参和地址传参都可以改变函数中参变量的值（本质都是地址传参）

  ```c++
  int main() 
  {
  	int a = 10;
  	int& ref = a;
      
  	std::cout << &ref << std::endl;
  	std::cout << &a << std::endl;
  	return 0;
  
  	//结果
  	/*004FF72C
  	004FF72C*/
  }
  ```

------



### class：面向对象编程（始）

> 面向对象是一种代码风格，c++，java，c#都是面向对象的语言；而事实上c++支持面向对象，面向过程，基于对象，泛型编程四种。

+ 使用类的结构便利程序员开发

+ 类中变量默认为私有变量

+ 类中的函数叫方法

  ```c++
  class Player
  {
  public:
  	int x, y;
  	int speed;
  	void Move(int xa, int ya)
  	{
  		x += xa * speed;
  		y += y * speed;
  	}
  };
  int main() 
  {
  	Player player1;
  	player1.x = 5;
  	player1.y = 3;
  	player1.speed = 2;
  	player1.Move(2, 3);
  	std::cout << player1.x << std::endl;
  	std::cout << player1.y << std::endl;
  	
  
  	return 0;
  
  	//结果
  	/*9
  	  9
  	  10*/
  }
  ```

  

-------



### 结构体

+ 结构体和类的区别仅仅是结构体默认变量为公共而类默认变量为私有

+ 区分结构体和类仅仅是靠个人习惯，Cherno的习惯是如果仅仅是表示一些数据就用结构体，如果要创建含有许多内容，许多功能的类，则用类，仅仅是习惯区分而已。

  ```c++
  class C1
  {
  	int  m_A; //默认是私有权限
  };
  
  struct C2
  {
  	int m_A;  //默认是公共权限
  };
  
  int main() {
  
  	C1 c1;
  	c1.m_A = 10; //错误，访问权限是私有
  
  	C2 c2;
  	c2.m_A = 10; //正确，访问权限是公共
  
  	system("pause");
  
  	return 0;
  }
  ```

  



------



### static关键字

+ **在类或者结构体外使用static**修饰的符号在link阶段是局部的，也就是它只对它的编译单元可见（即上文所描述的作用）；在编程中，尽量使用static，它可以避免全局变量引起的链接器报错

* **在类或结构体内的static**则表示这部分内存是这个类的所有实例共享的。用static修饰的变量为静态变量，会被所有实例共享，任何一个实例改变它都会改变，所以用实例来调用它并没有意义，可以直接classname::variablename来调用(静态方法也不需要访问实例就可以调用，如classname::functionname())，静态变量在类外定义，类中声明。

  ```c++
  
  class Player
  {
  public:
  	int x, y;
  	int speed;
  	static int name;
  };
  static int Player::name=333;
  
  
  int main() 
  {
  	Player player1;
  	Player player2;
  	std::cout << Player::name << std::endl;
  	player1.name = 444;
  	std::cout << Player::name << std::endl;
      player2.name = 555;
      std::cout << Player::name << std::endl;
  	return 0;
  
  	//结果
  	//333
      //444
      //555
  }
  ```



+ 静态方法内部无法访问到非静态变量，因为静态方法没有类实例，这涉及到类的运行机制，类中的每个非静态方法都会获得当前的类实例作为参数（this指针）这是一个隐藏参数，然而静态方法却不具备。静态方法几乎是独立于类中其他成员之外的，可以看做类本身自带的方法，和类外的函数运行机制极其相似(函数本质也是传参，f（x）的定义无法运行而f（int x）却可以，因为指明了参数的类型)。

+ **局部作用域中的静态变量**：理解变量生命周期和作用域的概念，特别是生命周期。局部静态变量是指在某一作用域中声明一个变量，这个变量的生命周期是到整个程序结束，变量的作用域只是这个作用域。

-------



### 枚举

+ 枚举就是一类数的集合，使用枚举就是为了让代码更简洁易懂
  枚举的基本结构应掌握（如上，有很多方式，这里不拓展了）
  枚举的数据依次递增，第一个默认是0
  枚举的类型可以自己设定
  普通的枚举并不是一个命名空间，里面的数据只是单纯存在于某个作用域内
  
  ```c++
  enum color:char
  {
  	BLACK,YELLOW=6,RED
  };
  ```
  
  

------



### 构造函数（面向对象编程）

+ 与java和c不同的是，c++必须手动初始化所有的基本类型，否则他们就会被设置成之前存留在内存中的值（如ｊａｖａ中ｉｎｔ和ｆｌｏａｔ有默认初始值为０），而为了不在每次实例化就调用函数去初始化，我们使用构造函数来解决。

+ 构造函数是一个特殊的函数，作用是来初始化类，在每一次实例化类时调用，确保你初始化了所有内存和做了所有你需要的设置，构造函数会在每一次实例化时就自动调用一次（包括new开辟），注意静态函数在被调用时并未初始化，所以不会调用构造函数。

  ```c++
  class Entity
  {
  public:
  	float X, Y;
  	Entity() {
  		X = 0.0;
  		Y = 0.0;
  	
  	}
  	Entity(float x,float y) {
  		X = x;
  		Y = y;
  
  	}
  
  	void Print()
  	{
  		std::cout << X << "," << Y << std::endl;
  	}
  
  };
  int main()
  {
  		Entity e;
  		Entity e2(10.0f, 5.0f);
  		e.Print();
  		e2.Print();
  	}
  //0,0
  //10,5
  ```

  

+ 特殊情况下，可以按照需求更改或删除构造函数，例如如果你定义了一个只含有静态方法的类，并不希望别人将他实例化。就可以将构造函数写在private权限中，或者将构造函数delete 如：Log()=delete，防止被调用，也就无法实例化。

  ```c++
  class Log
  {
  private:
  	Log() {};//或者log（）=delete
  public:
  	static void write() {
  		/*content*/
  	}
  };
  ```

  

+ 构造函数的特点：
  名字与类名相同，可以有参数，但是不能有返回值（void也不行）
  作用是对对象进行初始化工作，如给成员变量赋值等。
  如果定义类时没有写构造函数，系统会生成一个默认的无参构造函数，默认构造函数没有参数，不做任何工作。
+ 如果定义了构造函数，系统不再生成默认的无参构造函数
  对象生成时构造函数自动调用，对象一旦生成，不能在其上再次执行构造函数
  一个类可以有多个构造函数，为重载关系

------



### 析构函数(面向对象编程)

+ 在释放任何内容或者清理内存空间时执行，析构函数是用来释放或销毁构造函数中进行的一些的初始化工作以避免内存泄露。

+ 析构函数使用于栈和堆，因此如果new一个对象（在堆上），则在调用delete时调用析构函数；如果在栈上创建，则在跳出作用域时，对象被删除，调用析构函数。

+ 定义方法和构造函数相比只是1在前面加了~

  ```c++
  ~Entity() {
  		std::cout << "distory" << std::endl;
  	
  	}
  ```

+ 可以手动调用析构函数，不过没必要，还有可能造成系统崩溃

------



### 继承（面向对象编程）

+ 继承是面向对象编程的三大特性之一，提供了一种方式，将一系列类的所有通用代码放到**父类（基类）**中，避免相似代码的重复编写。

+ **子类（派生类）**继承父类后，相当于父类的一个超集，继承父类的成员，也可以拓展自己的成员。

+ 派生类拥有基类的全部成员函数和成员变量，不论是private、protected、public。需要注意的是：在派生类的各个成员函数中，不能访问基类的private成员。类的大小是可以变化的，因此会发现子类在继承父类后大小会增加。

  ```c++
  class Entity
  {
  public:
  	float X, Y;
  	int speed;
  	void Move(int xa, int ya)
  	{
  		X += xa * speed;
  		Y += ya * speed;
  	}
  	
  
  };
  class Player:public Entity
  {
  public:
  	const char* name;
  	void Print()
  	{
  		std::cout <<name<< std::endl;
  	}
  };
  ```

  

------



### 虚函数（面向对象编程）

+ 在类中声明方法或为函数传参时，调用这个方法的时候，c++总会调用属于这个类型的方法而非它的子类。

  ```c++
  class Entity
  {
  public:
  	std::string GetName() { return "entity"; }
  
  
  };
  class Player:public Entity
  {
  private:
  	std::string m_Name;
  public:
  	Player(const std::string& name)
  		:m_Name(name) {}
  	std::string GetName() { return m_Name; }
  	
  };
  
  void PrintName(Entity* entity)
  {
  	std::cout << entity->GetName() << std::endl;
  }
  
  int main()
  {
  	Entity* e = new Entity();
  	std::cout << e->GetName() << std::endl;
  	Player* p = new Player("mortyda");
  	std::cout << p->GetName() << std::endl;
  	PrintName(p);//传入player类型却打印出了“entity”
  
  
  		return 0;
  	}
  //entity
  //mortyda
  //entity
  ```

+ 如果想要告诉c++我们的传参是它的子类，可以在父类中编写一个方法标记为虚函数，然后在子类中重写这个方法让它去做其他的事情。

  ```c++
  
  class Entity
  {
  public:
  	virtual std::string GetName() { return "entity"; }//关键字virtual
  
  
  };
  class Player:public Entity
  {
  private:
  	std::string m_Name;
  public:
  	Player(const std::string& name)
  		:m_Name(name) {}
  	override std::string GetName() { return m_Name; }//关键字override省略（最好加上）
  	
  };
  
  void PrintName(Entity* entity)
  {
  	std::cout << entity->GetName() << std::endl;
  }
  
  
  
  int main()
  {
  	Entity* e = new Entity();
  	std::cout << e->GetName() << std::endl;
  	Player* p = new Player("mortyda");
  	std::cout << p->GetName() << std::endl;
  	PrintName(p);//不管是方法还是函数均正确调用出了子类的方法
  
  
  		return 0;
  	}
  //entity
  //mortyda
  //mortyda
  ```

  

+ 虚函数引入了一种要动态分配的东西，一般通过虚表(vtable)来实现,虚表是存放着所有虚函数的映射的列表

+ 只需在父类的函数中加virtual即可，最好在子类重写时在{}前加override使得代码可读性更好（当然不加也能运行）

+ 虚函数是有成本的，一是需要额外的内存来存储虚表，父类中有一个指针指向虚表，二是在调用虚函数时要遍历虚表来寻找我们想要的函数。

------



**一种特殊的虚函数：纯虚函数**

+ 在日常需求中经常会创建一个只包含未实现方法然后交由子类去实际实现功能的类，这样如果想实例化这个类，这个函数必须在在子类中实现（只有实现了所有纯虚函数的类才可以被实例化）。这个父类通常被称为**接口**，只需将虚函数的body部分改为=0即可

  ```c++
  class Entity
  {
  public:
  	virtual std::string GetName() = 0;
  };
  ```

  

-------



### c++中的可见性（面向对象编程）

+ 可见性是面向对象中的概念，指的是一个类中的成员或方法是否可见，谁能够访问，调用，使用它。c++中有三个访问修饰符public，private，protected

- public所有人可见；protect仅自己和子类可见；private尽自己可见，但有个东西叫**友元（friend）**可以将其他类或函数标记为当前类的友元以允许其访问这个类的私有成员。

+ 可见性是非常有意义的，可见性为他人提供了建议和指导。

------



### 数组

+ 数组的数据是连续的

+ 一个数组就是一个指针(new)，指向一段连续的内存空间，访问数组不同索引时，指针偏移相应数据类型对应字节x索引数

  ```c++
  #include <iostream>
  int main() 
  {
  	int example[5];
  	for (int i = 0; i < 5; i++) {
  		example[i] = i;
  	}
  	int* ptr = example;
  	std::cout << example[0] << std::endl;
  	//0
  	std::cout << ptr << std::endl;
  	std::cout << example << std::endl;
  	//00CFFA08
  	//00CFFA08
      //数组是一个指针
  	std::cout << *(ptr+1) << std::endl;
  	std::cout << *((int*)((char*)ptr+4)) << std::endl;
  	//1
  	//1
      //指针偏移相应数据类型对应字节x索引数
  	std::cout << (ptr+1) << std::endl;
  	//00CFFA0C
      
  	return 0;
  }
  ```

  

+ 一般用new来动态分配内存来创建数组，这是由于用new分配的内存生命周期长，会一直存在直到你手动删除它,因此如果你有一个函数要返回函数内新创建的数组，就应该用new来分配;另一种是在栈上直接创建数组，最好不要这样用

  ```c++
  #include <iostream>
  int main() 
  {
  	int example[5];//栈上创建，跳出作用域时被销毁
  	int* another = new int[5];//堆上创建，一直存在
  	delete[] another;
  
  	return 0;
  
  }
  ```

  **内存间接寻址**：栈上创建数组，会有指针直接指向它的内存；堆上创建数组，则会指向存放它地址的指针，导致访问时在内存中跳来跳去，影响性能；

  ```cpp
  class Entity
  {
  public:
      int example[5]; //栈数组
      Entity()  //创建一个构造函数，用来初始化所有值为2
      {
          for (int i = 0; i< 5;i++) 
               example[i] = 2;   
      }
  };
  
  int main()
  {
      Entity e; //实例化一个对象。如果我们查看Entity e的内存地址，可以看到Entity的内存上实际就是一行，包含了数组中所有的2，所有的数据都在这儿
  
      std::cin.get();
  }
  ```

+ c++11的标准库中定义的有std：：array，有很多优于原生数组的点，如边界检查，记录数组大小等。

+ 原生数组的大小是无法访问的（当然也有一些方法，不可靠，而且在栈上创建的话，如果数组作为函数参数，就无法使用sizeof来计算）。

  ```c++
  int a[5]; //栈数组
  sizeof(a); //20bytes
  int count = sizeof(a) / sizeof(int); //5
  
  int* example = new int[5];  //堆数组
  int count = sizeof(example) / sizeof(int); //1
  ```

  

+ 数组的大小必须好好维护，在栈中为数组申请内存时，大小必须是在编译时要知道的常量（此处挖坑）

  

------



### 字符串

+ c++中有数据类型char是字符型，将指针转化为字符型指针，就可以根据字节进行运算，也可以帮助我们分配内存缓冲区（1k内存就分配1024字节）。因为有不同语言存在，一个字符一般不止有一个字节（8位，有2**8=256种组合，不满足所有语言的需要）所以使用utf-16.

+ 字符串其实就是一组字符数组，在内存中就是一串连续的内存空间，空间末尾会有一个字符串的休止符标记为0，这样指针在跑的时候就知道字符串在哪里结束。

+ 一般用const char* zfc=“sasd”声明一个字符数组（c风格）这里的const默认加上，表明字符串大小不可变（因为实际上就是一块已声明的内存），而且在c++中用双引号定义字符串时，默认它就是一个const char数组而非char数组，当然需要操作的时候可以手动改成char。

 ```c++
 const char* name = "cherno"; //c风格字符串
 char* name = "cherno" //报错，因为C++中默认的双引号就是一个字符数组const char*
 char name2[8] = { 'm','o','r','t','y','d','a'};;//报错，缺少空终止符
 char name2[8] = { 'm','o','r','t','y','d','a', 0};//正确
 char name2[8] = { 'm','o','r','t','y','d','a', '\0'};//正确,因为ascii码'\0'就是null
 ```



+ 然而在c++标准库中存在string__basic模板类，以及用char作为参数带入模板类产生的string类可以使用，在使用时要引入string头文件，string中含有接受参数char指针及const char指针的构造函数（虽然在iostream中含string的定义但无法输出流，而string对输出流做了重载）
+ string中含有很多重载函数，因此可以支持许多操作，包括字符串相加，字符串大小啥的

 ``` c++
 #include <iostream>
 #include <string>
 int main() 
 {
 	const char* name = "mortyda";//c 风格
 	std::string name2 = "mortyda";//c++风格
 
 	std::cout << name2 << std::endl;
 	
 	return 0;
 }
 ```

------



#### 字符串字面量(string literal)

字符串字面量就是双引号之间的一串字符，默认情况下是一个const char数组。

Char *p =”hello“//p是一个指针，直接指向常量区，修改p[0]即是修改常量区的内容，是不允许的。

Char p[] =”hello“//编译器在栈上创建一个字符串p，把“hello”从常量区复制到p，修改p[0]就相当于修改数组元素一样，是允许的。

```c++
#include <iostream>
#include <string>
int main() 
{
	char name[] = "mortyda";
	std::cout << name << std::endl;
	
	return 0;
}
```

------



### const关键词

+ const更像一个假的关键字，因为它在生成代码时并没有做什么，const是一种针对开发人员写代码的强制规则，是一种让代码保持整洁的机制，const让某个东西保持不变。

+ 变量前加const可以变成常量,通过强制转换可以绕过const的约束（不推荐）;

  ```c++
    const int Age =90;
    int* a = new int;
    *a = 2;
    a = &Age //error!
    a =(int*)&Age //ok
  ```

  

+ 指针前加const变为**常量指针**，指针指向可改，指向的值不可改

  指针后加const变为**指针常量**，指针指向不可改，指向的值可改

  指针前后都加const那就是都不可更改

  ```c++
  const int* a = new int;
  *a = 2; //error! 不能再去修改指针指向的内容了。
  a =(int*)&Age  //可以改变指针指向的地址
      
  int* const a = new int;
  *a = 2; //ok
  a =(int*)&Age  //error
  ```

  

+ 在类中，在方法名（） 后加const表明这个方法不可修改类中的任何内容，而在这里又有**mutable关键词**，允许const方法修改用mutable修饰的变量。

  ```c++
  class Entity
  {
  private:
      int m_x,m_y;
  public:
      int Getx() const  //const的第三种用法，他和变量没有关系，而是用在方法名的后面
      {
          return m_x; //不能修改类的成员变量
          m_x = 2; //ERROR!
      }
      void Setx(int a)
      {
          m_x = a; //ok
      }
  };
  
  void PrintEntity(const Entity& e)  //const Entity调用const函数
  {
      std::cout << e.Getx() << std::endl;
  }
  
  int main()
  {
      Entity e;
  }
  ```

  

+ mutable还有一种情况是在lambda表达式中使用，它可以创建局部变量进行操作（视频里只演示了这一个功能，绝大多数情况下mutable只是在类中使用）

  ```c++
  #include <iostream>
  int main()
  {
      int x = 8;
      auto f = [=]() mutable
      {
          x++;
          std::cout << x << std::endl;
      };
      f();
  }
  
  ```

-------



### 构造函数初始化列表

+ 在c++的构造函数中应该进行成员初始化，就是在构造函数的函数名后加：achengyuan（0）这样来进行初始化，注意如果有多个成员变量，初始化的时候必须按照声明变量的顺序进行

  ```c++
  class Entity
  {
  private:
  	std::string m_Name;
  	int score;
  public:
  	Entity()
  		: m_Name("unknown"),score(0)
  	{
  	}
  };
  ```

  

+ 构造函数初始化列表很重要，可以把它看做一种代码风格，但跟应该记住它有重要的功能，在创建实例时，如果没有初始化列表，就可能会同时创建两个变量而造成性能浪费 ;其中一个直接浪费掉了

  ```c++
  #include <iostream>
  #include <string>
  
  class Example {
  
  public:
  	Example()
  	{
  		std::cout << "create" << std::endl;
  
  	}
  	Example(int x)
  	{
  		std::cout << "create with" << x << std::endl;
  	}
  };
  class Entity1
  {
  private:
  	std::string m_Name;
  	Example example;//此时也会创建实例
  public:
  	Entity1()
  	{
  		m_Name = std::string("unknown");
  		example = Example(12);//又创建了一遍
  	}
  };
  
  int main() 
  {
  	Entity1 e;
  	
  	
  	return 0;
  }
  //create
  //create with12
  ```

  

--------



### 三元运算符

+ 读懂：

  例：s_speed=s_level>5?10:15

  嵌套时：s_speed=s_level>5?s_level>10? 15:10:5

+ 三元运算符时运算速度快一些，代码简洁一些（不用创建多余的变量）
+ 尽量不对三元运算符进行嵌套

------



### 如何创建对象

+ 创建对象需要分配内存，在c++中我们可以选择在堆或栈上创建对象

+ 首选用栈，最快最简洁，如果使用堆就要用new关键词，int* sth=new int(100) 栈上创建的生命周期会受到作用域的限制，而且栈空间一般十分有限，chenro讲的跟网上的大差不差

[C++创建对象的两种方式 - 邱明成 - 博客园 (cnblogs.com)](https://www.cnblogs.com/qiumingcheng/p/7819639.html)

------



### new关键词 

+ 基本用法不说了（int*b=new int[50]）,在堆上分配内存并返回相应指针,如果用new和[]开辟内存，delete时也要加[],否则不用。

  ```c++
  int* ptr = new int;//找到连续四字节的内存并返回它的地址
  int* ptr2 = new int[3];//创建数组对象，占用12字节
  Entity1* ptr3=new Entity1()//创建自定义类对象，括号可以省去，因为有默认构造函数
  delete ptr;
  delete[] ptr2;
  delete ptr1;
  ```

+ 和类一起使用时（Classa *a=new Classa()，不加括号则调用默认构造函数）不仅分配了内存，而且调用了构造函数

  > new 关键字创建对象时 对于内置类型：加括号会初始化，不加括号不初始化；对于自定义类型，都会调用默认构造函数，加不加括号没区别。

+ 一般情况下new关键词会调用底层c函数：malloc（是用来分配内存的，传入一个需要的size返回void指针指向内存地址）

  ```c++
  Entity1* ptr3 = new Entity1();//创建自定义类对象，括号可以省去，因为有默认构造函数
  Entity1* ptr4 =(Entity1*) malloc(sizeof(Entity1));//malloc函数返回void型指针，需强转，且此时未调用构造函数
  ```

+ 其实new支持一个placement new语法，可以决定内存来自哪里（此时并未开辟内存，而是在指定内存上调用构造函数进行了初始化）

  ```c++
  Entity1* ptr3 = new Entity1();
  Entity1* ptr5 = new(ptr3) Entity1();
  delete ptr5;
  ```

+ new是一个操作符，也是可以重载的。

------



### 隐式转换及explicit关键字

+ 隐式转换就是自动的将一种数据类型作为另一种类型使用。比如类中的构造函数如果初始化了某个属性int sth，那么就可以在声明变量时直接传参classa a=8；（举例）

+ c++只支持一次的隐式转换。在构造函数前加explicit关键词，可以禁止该类对象隐式转换

  ```c++
  class Entity2
  {
  private:
  	std::string m_Name;
  	int m_Age;
  public:
  	Entity2(const std::string& name)
  		:m_Name(name),m_Age(-1){}
  	Entity2(int age)//explicit Entity2(int age) 则b=20会报错
  		:m_Name("UnKnown"), m_Age(age) {};
  };
  int main() 
  {	
  	//Entity2 a = Entity2("Mortyda");
  	//Entity2 b = Entity2(20);
  	Entity2 a = (std::string)"mortyda";//手动进行了一次转换，因为c++只支持一次隐式转换，但是charno直接将字面量赋给了a却没报错，不知道为啥
  	Entity2 b = 20;//隐式转换	
  	return 0;
  }
  ```

  

------



### c++中的运算符以及运算符重载

+ 在你的程序中定义运算符，重载是指给他一个新的含义或者是增加函数或者是重新创建

+ 除了在值得的时候对运算符符进行重载否则尽量少用

+ 重载的方式和定义函数差不多，返回类型 operator 运算符 (参数)（粗略形容一下）*补：后面的应用中发现了可以重载为友元函数。

  ```c++
  struct Vector2
  {
  	float x, y;
  	Vector2(float x, float y) 
  		: x(x),y(y){}
  	Vector2 Add(const Vector2& other)const 
  	{
  		return Vector2(x + other.x, y + other.y);
  	}
  	Vector2 operator+(const Vector2 & other) const//对+进行重载，当然根据不同代码风格也可以在add函数中反过来调用operator+
  	{
  		return Add(other);
  	}
  };
  int main() 
  {
  	Vector2 a(1.1f, 1.2f);
  	Vector2 b(1.3f, 1.4f);
  	std::cout << a.Add(b).x<< a.Add(b).y << std::endl;
  	std::cout << (a+b).x << (a+b).y << std::endl;
  	return 0;
  }
  //2.42.6
  //2.42.6
  ```

  

-------



### c++中的this指针

+ this指针指向当前对象实例,在类中调用类外函数时可以使用this传参

  ```c++
  class Entity;  //前置声明。
  void PrintEntity(Entity* e); //在这里声明
  class Entity  
  {
    public:
      int x,y;
      Entity(int x, inty)
      {
          this->x = x;   
          this->y = y; 
          PrintEntity(this); //我们希望能在这个类里调用PrintEntity,就可以传入this
      }
  }; 
  void PrintEntity(Entity* e) //在这里定义
  {
      //print something
  }
  ```

+ this可以避免类中方法定义时形参和类中其他成员重名

  ```c++
  class Entity
  {
  public:
   int x,y;
   Entity(int x, int y)
   {
          this->x = x;//等同(*this).x  
          this->y = y; 
   }
  };
  ```

+ 在有const修饰的类的方法中，this一个常量指针，指向的值不可以改

  ```c++
   int GetX() const  //在函数后面加上const是很常见的，因为他不会修改这个类
   {
   `  Entity* e = this;//ERROR!
      const Entity* e= this;//ok,表明此时this类型为const Entity*
   
   }
  ```

---------



### 对象的生存周期（基于栈的生存周期）

+ 在c++中每当进入一个作用域中时，我们就是在push一次栈，push的内容就是声明的变量，而当作用域结束时，变量的生命就结束了。

  ```c++
  #include <iostream>
  #include <string>
  
  class Example {
  public:
  	Example()
  	{
  		std::cout << "create" << std::endl;
  	}
  	Example(int x)
  	{
  		std::cout << "create with" << x << std::endl;
  	}
  	~Example()
  	{
  		std::cout << "die"<< std::endl;
  	}
  };
  int main()
  {
  	{
  		Example* exa=new Example();
          //create
  		Example* exa2;	
          //create 
          //die
  	}
  }
  ```

  

+ 由于不知道栈上创建变量的生存周期很短，编译可能会产生错误，因为离开了局部作用域后，变量就销毁了。

  ```c++
  //经典错误，栈上创建数组
  int CreateArray()
  {
      int array[50];  //在栈上创建的
      return array;
  }
  int main()
  {
      int* a = CreateArray(); //不能正常工作
  }
  ```

  

+ 在栈上创建变量是有意义的，可以帮助我们自动化代码。一个简单的例子是作用域指针，本质上是一个类，是一个指针的包装器，构造时在堆上分配指针，在析构时删除指针，我们可以自动化new和delete。作用域指针的例子大致就是在一个类的构造函数和析构函数创建和删除指针（在堆上）然后通过栈上创建类对象达到堆上创建指针又能自动化删除的功能。

  ```c++
  #include <iostream>
  #include <string>
  
  class Example {
  public:
  	Example()
  	{
  		std::cout << "create" << std::endl;
  	}
  	Example(int x)
  	{
  		std::cout << "create with" << x << std::endl;
  	}
  	~Example()
  	{
  		std::cout << "die"<< std::endl;
  	}
  };
  class ScopedPtr
  {
  private:
  	Example* m_Ptr;
  public:
  	ScopedPtr(Example* ptr)
  		:m_Ptr(ptr)
  	{
  
  	}
  	~ScopedPtr()
  	{
  		delete m_Ptr;
  	}
  };
  
  int main()
  {
  	{	
  		Example* e2 = new Example();
  		ScopedPtr e = new Example();		
  	}
  	
  	return 0;
  }
  //create
  //create
  //die
  ```

-------



### c++中的智能指针

+ 智能指针本质上是原始指针的一种包装，将new和delete自动化

+ unique_ptr，是**作用域指针**（上一节的案例即为一种作用域指针），unique_ptr无法被复制，一旦复制会指向同一块内存，同生则生，同死则死。

  + 在include<memory>头文件后

  + 使用std：：unique_ptr<Entity>entity=std::make_unique<Entity>();优于unique_ptr<Entity> entity（new entity（）），这可以避免异常（不会由于得到一个没有用的悬空指针而造成内存泄漏）

  + 不可以std::unique_ptr<Entity> entity=new Entity();创建，因为unique_ptr构造函数是explicit的无法使用隐式转换。

    ```c++
    #include <iostream>
    #include <string>
    #include <memory>
    
    class Example {
    public:
    	Example()
    	{
    		std::cout << "create" << std::endl;
    	}
    	Example(int x)
    	{
    		std::cout << "create with" << x << std::endl;
    	}
    	void Print() {};
    	~Example()
    	{
    		std::cout << "die"<< std::endl;
    	}
    };
    
    int main()
    {
    	{
    		std::unique_ptr<Example> example(new Example());//right
            std::unique_ptr<Example> example2=std::make_unique<Example>();//better
    		//std::unique_ptr<Example> example=new Example()//wrong
    		example->Print();		
    	}
    	
    	return 0;
    }
    //create
    //create
    //die
    //die
    ```

    

+ shared_ptr,使用引用计数（引用计数是一种介意跟踪你的指针有多少引用的方法，一旦引用到0，指针就被删除）

  + 创建时使用std::shared_ptr<Entity>sharedEntity = std::make_shared<Entity>();更好，它的原因不同于unique_ptr，而是因为shared_ptr需要分配另一块内存存储引用计数，如果使用new关键词再用构造函数的话会进行两次分配(多了一次new Entity的分配)，效率不高，儿make_share则可以将它们合并到一次。

    ```c++
    #include <iostream>
    #include <string>
    #include <memory>
    
    class Example {
    public:
    	Example()
    	{
    		std::cout << "create" << std::endl;
    	}
    	Example(int x)
    	{
    		std::cout << "create with" << x << std::endl;
    	}
    	void Print() {};
    	~Example()
    	{
    		std::cout << "die"<< std::endl;
    	}
    };
    
    int main()
    {
    	{ 
    		std::shared_ptr<Example>sharedExample2;//持有sharedExample的引用，实例不会被销毁
    		{
    			std::shared_ptr<Example> sharedExample = std::make_shared<Example>();
    			sharedExample2 = sharedExample;
    		}
    		std::cout << "still alive" << std::endl;
    
    	}
    
    
    	return 0;
    }
    //create
    //still alive
    //die
    ```

    

  + 通常伴随shared_ptr一起使用的还有weak_ptr,声明方式同上，可以给它赋值为shared_ptr而不会增加引用

    ```c++
    {
        std::weak_ptr<Entity> e0;
        {
            std::shared_ptr<Entity> sharedEntity = std::make_shared<Entity>();
            e0 = sharedEntity;
        } //此时，此析构被调用，内存被释放
    }
    ```

    

+ 智能指针是值得讨论的话题，但目前来说无法替代new和delete，在需要的时候应该尽量使用unique_ptr,因为开销很少，当需要在对象之间共享时，应该使用shared_ptr

--------



### c++中的复制与拷贝构造函数

+ 拷贝是指将一个对象或者语句或一段数据从一个地方复制到另一个地方。有时候我们并不想重新复制一遍，而是在原来的数据上进行修改。

+ 在使用赋值运算符时，复制是一直都会执行的，有时候是具体的内容，有时是内存的地址（引用除外，引用只是起了个别名）

（cherno在视频中带我们用原始的方式定义了字符串类，然而在拷贝时却发生了错误，这是因为进行了**浅拷贝**，在字符串类的定义中存在指针的构造，而浅拷贝只会复制指针的地址而非创造新的指针，因此在调用析构函数时，指针指向的内存会释放两遍，产生了报错，所以我们需要深拷贝）

 ```c++
 #include <iostream>
 #include <string>
 class String
 {
 private:
 	char* m_Buffer;
 	unsigned int m_Size;
 public:
 	String(const char* string)
 	{
 		m_Size = strlen(string);
 		m_Buffer = new char[m_Size + 1];
 		for (int i = 0; i < m_Size+1; i++)
 		{
 			m_Buffer[i] = string[i];
 
 		}
 		//memcpy(m_Buffer,string,m_Size+1);
 	}
 
 	String(const String& other)//此为自定义拷贝构造函数，而默认构造函数仅仅复制了成员变量地址
 		:m_Size(other.m_Size)
 	{
 		m_Buffer = new char[m_Size+1];
 		memcpy(m_Buffer, other.m_Buffer, m_Size+1);
 		std::cout << "copy" << std::endl;
 	}
 	~String()
 	{
 		delete[] m_Buffer;
 		std::cout << "die"<< std::endl;
 	}
 	char& operator[](unsigned int index) {
 	
 		return m_Buffer[index];
 	}
 	friend std::ostream& operator<<(std::ostream& stream, const String& string);//重载<<为友元，便于直接使用m_Buffer
 };
 
 std::ostream& operator<<(std::ostream& stream, const String& string)
 {
 	stream << string.m_Buffer;
 	return stream;
 }
 void PrintString(const String& string)//const引用传参，防止字符串被修改，且避免多余复制进行
 {
 	std::cout << string << std::endl;
 }
 int main()
 {
 	String string = "mortyda";
 	String string2 = string;
 	string2[4] = 'i';
 	//std::cout << string << std::endl;
 	//std::cout << string2 << std::endl;
 	PrintString(string);
 	PrintString(string2);
 	std::cin.get();
 }
 ```



+ **深拷贝**：深拷贝是复制整个对象。这里使用的方法是拷贝构造函数。
  + 拷贝构造函数：一种构造函数，当你复制第二个字符串时它会被调用。c++中有默认提供的拷贝构造函数。
  + c++默认会有一个拷贝构造函数，它所做的就是复制内存，将被复制的对象的内存浅拷贝进目标。如果想进行深拷贝则需要在拷贝构造函数中重新创建空间，分配内存并进行拷贝。（在cherno演示中还写了一个print函数用于输出字符串，此时传入的参数应该是字符串的引用，避免了再一次的复制，此外，在参数前用const修饰避免了参数的改变）

---------



### c++中的箭头操作符

+ 就是（*ptr）.func（）的快捷方式即ptr->func()即指针用箭头访问对象的方法或变量

+ cherno对->进行了运算符重载，代码更加简洁。

  ```c++
  #include <iostream>
  class Entity
  {
  private:
      int x;
  public:
      void Print() const   //添加const
      {
          std::cout << "hello!" << std::endl;
      }
  };
  
  class ScopedPtr
  {
  private:
      Entity* m_Ptr;
  public:
      ScopedPtr(Entity* ptr)
          : m_Ptr(ptr)
      {
      }
      ~ScopedPtr()
      {
          delete m_Ptr;
      }
      Entity* operator->()
      {
          return m_Ptr;
      }
      const Entity* operator->() const //添加const
      {
          return m_Ptr;
      }
  };
  
  int main()
  {
      {
          const ScopedPtr entity = new Entity(); //如果是const，则上面代码要改为const版本的。
          entity->Print();
      }
      std::cin.get();
      return 0;
  }
  ```

  

+ 使用箭头操作符可以获取内存中某个成员变量的偏移量（这一部分受教很多，指针->属性这一方法访问属性实际上是将指针和属性的值相加得到属性地址进行访问，所以使用空指针即在0基础上进行偏移，而通过对象访问实质上也是地址的访问；而cherno在演示-->的第一种用法时只是说相当于对指针解引用然后再调用函数，我找不到点操作符的机制，所以暂且认为箭头操作符只是效果上跟解引用一样，实质上是通过偏移量相加进行访问的吧）

  ```c++
  struct vec2
  {
      int x,y;
      float pos,v;
  };
  int main()
  {   
      int offset = (int)&((vec2*)nullptr)->x; // x,y,pos,v的offset分别为0,4,8,12
      std::cout<<offset<<std::endl;
      std::cin.get();
      return 0;
  }
  ```


> 引自B站评论：
> 因为"指针->属性"访问属性的方法实际上是通过把指针的值和属性的偏移量相加，得到属性的内存地址进而实现访问。 而把指针设为nullptr(0)，然后->属性就等于0+属性偏移量。编译器能知道你指定属性的偏移量是因为你把nullptr转换为类指针，而这个类的结构你已经写出来了(float x,y,z)，float4字节，所以它在编译的时候就知道偏移量(0,4,8)，所以无关对象是否创建

--------



### c++中的动态数组Vector

> STL库本质上是一个装满容器，容器类型的库，这些容器包含特定的数据。STL库可以模板化任何东西，因此被称为标准模板库，容器包含的底层数据类型都可以由我们自己决定，所有内容都由模板组成。

+ 一般的动态数组经常是固定长度，无法进行拓展。而Vector则可以进行动态拓展
+ vector数组在一开始可以不指定它的长度。在分配长度后，如果超过了分配的大小，系统会在内存中重新创建一个比第一个更大的新数组，将所有元素都复制到这里并删除旧的那一个

+ 这里讨论了栈分配和堆分配，cherno建议视情况而定，并推荐尽量栈分配，指针应是最后的选择。在技术上，储存vertex对象比储存地址更好，因为是连续的紧挨着的一串内存，在操作时位于一条缓存线上（以后会讲）。唯一问题是在调整vector大小时可能比较缓慢。

  ```c++
  #include <iostream>
  #include <string>
  #include <vector>
  
  struct Vertex
  {
  	float x, y, z;
  };
  
  std::ostream& operator<<(std::ostream& stream, const Vertex& vertex)
  {
  	stream << vertex.x << "," << vertex.y << "," << vertex.z;
  	return stream;
  }
  
  int main()
  {
  	//Vertex* vertices = new Vertex[5];
  	std::vector<Vertex> vertices;//创建，与java不同的是，此处可以传递int等原始类型
  	vertices.push_back({ 1,2,3 });//添加
  	vertices.push_back({ 4,5,6 });
  	for (Vertex& v : vertices) 
  	{
  		std::cout << v << std::endl;
  	
  	}
  	vertices.erase(vertices.begin() + 1);//删除，传参为迭代器
  	for (Vertex& v : vertices)
  	{
  		std::cout << v << std::endl;
  
  	}
  	std::cin.get();
  	return 0;
  }
  ```

+ 参数传递时，如果不对数组进行修改，请使用引用类型传参。

  ```c++
  void Function(const std::vector<T>& vec){};
  ```

+ vector的使用优化。了解所在环境以及工作原理和可用工具，是进行优化的关键。比如如下代码：

  ```c++
  	std::vector<Vertex> vertices;
  	vertices.push_back(Vertex(1,2,3));
  	vertices.push_back(Vertex(4,5,6));
  	vertices.push_back(Vertex(7,8,9));
  ```

+ 这里实质上Vertex的拷贝构造函数复制了6次，有两点不足，一是vector的长度没有初始化，导致每一次增加就会进行一次扩容，在此之前加vertices.reserve(3);可以指定需要的内存（而非创建3个对象）来解决。二是每一次对象都是先在main函数的栈上创建，然后再放到数组中去，导致复制了三次。应将push_back（）替换为emplace_back（）,使得在实际vector内存中使用以下参数来创建一个对象。

  ```c++
      vertices.reserve(3);
      vertices.emplace_back(1, 2, 3);
      vertices.emplace_back(4, 5, 6);
      vertices.emplace_back(7, 8, 9);
  ```

  

--------



### c++中使用库（静态链接）

+ c++使用库两种方式，一是保留库的副本获取实际依赖库的源代码并自己进行编译（一般大型项目）；二是以二进制文件的形式进行链接（小项目，更快更容易）。这里先介绍第二种，以glfw库为例。

+ 库一般包含着两部分，includes（包含目录）和library（库目录），includes目录是一堆头文件，library目录包含预先构建的二进制文件（对于glfw库来说，其中包含动态库和静态库）

+ 动态库与静态库区别（这里仅仅是简述）：静态链接表明此库会被放到可执行文件中（.exe）动态链接则是在程序运行时被链接的，此时装载动态链接库。技术上静态链接要更快，因为编译器或链接器连接时会优化。

+ dll是一种运行时动态链接库，lib为静态链接库，而下图中的dll.lib文件实质上是一个静态库，存储动态库的所有函数，符号的位置，以便在编译时链接他们，否则需要用函数名来链接。

+ 在include时用<>还是“”（实际上没有区别）cherno习惯当外部依赖库时用<>,当内部使用，和解决方案一起编译时则用“”

+ ![1](1.png)

### c++中处理多返回值

+ 方法一（最好）：若有个函数叫ParseShader需要返回两个字符串。可以选择的解决方法是：创建一个叫做ShadweProgramSource的结构体，它只包含这两个字符串。若还想返回一个整数或其他不同类型的东西，可以把它添加到结构体中并返回它。

  ```c++
  //cherno的例子
  struct ShaderProgramSource
  {
  	std::string VertexSource;
  	std::string FragmentSource;
  	int a;
  }
  static ShaderProgramSource ParseShader(const std::string& filepath)
  {
  
  }
  return {vs,fs};
  
  ```

  

+ 方法二：把函数定义成`void`，然后通过**参数引用传递**的形式“返回”两个字符串，这个实际上是修改了目标值，而不是返回值，但某种意义上它确实是返回了两个字符串，而且没有复制操作，技术上可以说是很好的。但这样做会使得函数的形参太多了，可读性降低，有利有弊 。（也可以使用指针传递，指针传递有一个好处就是可以传入null值的指针）

  ```c++
  #include <iostream>
  void GetUserAge(const std::string& user_name,bool& work_status,int& age)
  {
      if (user_name.compare("cherno") == 0)
      {
          work_status = true;
          age = 18;
      }
      else
      {
          work_status = false;
          age = -1;
      }
  }
  
  int main()
  {
      bool work_status = false;
      int age = -1;
      GetUserAge("cherno", work_status, age);
      std::cout << "查询结果：" << work_status << "    " << "年龄：" << age << std::endl;
      getchar();
      return 0;
  }
  ```

+ 方法三：将array（数组）或vector作为函数的返回值

  ```cpp
  //设置是array的类型是stirng，大小是2
  std::array<std::string, 2> ChangeString() {
      std::string a = "1";
      std::string b = "2";
  
      std::array<std::string, 2> result;
      result[0] = a;
      result[1] = b;
      return result;
  //若返回两种以上则可以使用vector。它和array的区别是array在栈上创建，而vector会把它的底层存储在堆上，所以std::array会更快。
  }
  ```

+ 方法四：使用std::pair返回两个返回值

  可以**返回两个不同类型**的数据。
  使用std::pair这种抽象数据结构，该数据结构可以绑定两个异构成员。这种方式的弊端是**只能返回两个值。**

  ```c++
  #include <iostream>
  
  std::pair<bool, int> GetUserAge(const std::string& user_name)
  {
      std::pair<bool, int> result;
  
      if (user_name.compare("xiaoli") == 0)
      {
          result = std::make_pair(true, 18);
      }
      else
      {
          result = std::make_pair(false, -1);
      }
  
      return result;
  }
  
  int main()
  {
      std::pair<bool, int> result = GetUserAge("xiaolili");
      std::cout << "查询结果：" << result.first << "   " << "年龄：" << result.second << std::endl;
      getchar();
      return 0;
  }
  ```

+ 方法五：使用std::tuple返回三个或者三个以上返回值

  std::tuple这种抽象数据结构可以将三个或者三个以上的异构成员绑定在一起，返回std::tuple作为函数返回值理论上可以返回三个或者三个以上的返回值。
  `tuple`相当于一个类，它可以包含多个变量，但他不关心类型，用`tuple`需要包含头文件

  ```c++
  #include <iostream>
  #include <tuple>
  
  std::tuple<bool, int,int> GetUserAge(const std::string& user_name)
  {
      std::tuple<bool, int,int> result;
  
      if (user_name.compare("xiaoli") == 0)
      {
          result = std::make_tuple(true, 18,0);
      }
      else
      {
          result = std::make_tuple(false, -1,-1);
      }
  
      return result;
  }
  
  int main()
  {
      std::tuple<bool, int,int> result = GetUserAge("xiaolili");
  
      bool work_status;
      int age;
      int user_id;
  
      std::tie(work_status, age, user_id) = result;
      std::cout << "查询结果：" << work_status << "    " << "年龄：" << age <<"   "<<"用户id:"<<user_id <<std::endl;
      getchar();
      return 0;
  }
  ```

### c++中的模板

> 模板允许你定义一个可以根据你的用途进行编译的模板（有意义下）。故所谓模板，就是让编译器基于DIY的规则去为你写代码 。

+ 模板在调用函数时才开始根据所给的参数进行函数创建

  ```c++
  #include <iostream>
  
  
  template<typename T>
  void Print(T value)
  {
  	std::cout << value << std::endl;
  }
  
  
  int main() {
  	shiyongdechangji
  	Print(3);
  	Print("sasa");
  
  	return 0;
  
  }
  ```

+ 模板的使用范围很广，下面是在类中使用的场景（模版功能强大，但不应该滥用）

  ```c++
  #include <iostream>
  template<int N>
  
  class Array {
  private:
  	int m_Array[N];
  public:
  	int GerSize(){return N;}
  };
  
  int main() {
  	
  	Array<3> exa;
  	std::cout << exa.GerSize() << std::endl;
  	return 0;
  
  }
  //再进一步：
  
  #include <iostream>
  
  
  template<typename T,int N>
  class Array {
  private:
  	T m_Array[N];
  public:
  	int GerSize(){return N;}
  };
  
  int main() {
  	
  	Array<std::string,3> exa;
  	std::cout << exa.GerSize() << std::endl;
  	return 0;
  }
  ```

###　c++中的堆与栈内存比较

+ 堆与栈存在于内存之中，物理位置都一样，**栈**通常是一个预定义大小的内存区域，通常约为2兆字节左右。**堆**也是一个预定义了默认值的区域，**但是它可以**随着应用程序的进行而改变。

+ 是否使用new关键字决定了堆分配与栈分配

+ 栈分配方式：

  > **在栈上**，分配的内存都是**连续**的。添加一个int，则**栈指针（栈顶部的指针）**就移动4个字节，所以连续分配的数据在内存上都是**连续**的。栈分配数据是直接把数据堆在一起（所做的就是移动栈指针），所以栈分配数据会很快 。
  > 如果离开作用域，在栈中分配的所有内存都会弹出，内存被释放。

  堆分配方式：

  > **在堆上**，分配的内存都是**不连续**的，`new`实际上做的是调用malloc函数，在内存块的**空闲列表**中找到空闲的内存块，然后把它用一个指针圈起来，然后返回这个指针。（但如果**空闲列表**找不到合适的内存块，则会询问**操作系统**索要更多内存，而这种操作是很麻烦的，潜在成本是巨大的）
  > 离开作用域后，堆中的内存仍然存在

+ 尽量使用栈分配，效率高开销少。

### c++中的宏

+ 宏是在**预处理阶段**评估，比模板早一点，用于将代码中的文本**替换**为其他东西（**纯文本替换**）（不一定是简单的替换，调用宏的方式可以自定义）

  ```c++
  #include <iostream>
  #defind WAIT std::cin.get() //简单替换
  #define log(x) std::cout<<x<<std::endl//带参数的替换
  int main() {	
  	log("assad");
      WAIT;
  }
  ```

+ 宏可以帮助调试,根据编译器模式为debug还是release预定义不同的变量，在项目属性-C/C++-预处理器-预处理器定义，里面为debug和release分别添加不同的变量 PR_REBUG 和 PR_RELEASE，那么在项目选择不同编译模式的时候，就会分别预定义这两个变量，以此来控制不同版本的代码状况。

  ```c++
  #ifdef PR_DEBUG
  #define LOG(x) std::cout<<x<<std::endl;
  #else
  #define LOG(x)
  #endif
  int main()
  {	
  	LOG("ydc");
  	std::cin.get();
  }
  ```

  或者使用if语句代替ifdef判断，一般更好一些。只需设置Debug(PR_DEBUG == 1)

  使用if 0相当于false，无视包含的宏

  ```c++
  #include <iostream>
  
  #if 0   //从这里到最后的endif的宏都被无视掉了，某种意义上的删除
  
  #if PR_DEBUG == 1   
  #defind LOG(x) std::cout << x << std::endl  
  #elif defind(PR_RELEASE)  
  #endif    
  
  #endif  //结束
  
  int main() {
      LOG("hello");
      return 0;
  }
  ```

+ 宏定义可以不写在同一行，代码较多时用反斜杠换行

  ```c++
  #define MAIN int main()\
  {\
  	std::cin.get;\
  }
  
  MAIN
  ```

### auto关键字

+ auto关键字可以自动推断出变量的类型，并且根据类型的改变而自动调整。使用auto在api改变时不需要改变相关系的代码，但也可能会造成依赖变量类型的代码出现错误，因此这种情况下不用

+ 在使用长类型比如迭代器或者长类型名时用auto很方便。

  ```c++
  //迭代器
  int main() {
  	std::vector<std::string> strings;
  	strings.push_back("apple");
  	strings.push_back("orange");
      //it 类型为std::vector<std::string>::iterator
  	for (auto it = strings.begin(); it != strings.end(); it++)
  	{
  		std::cout << *it << std::endl;
  	}
  }
  ```

  ```cpp
  //长类型名
  #include <iostream>
  #include <string>
  #include <vector>
  #include <unordered_map>
  
  class Device{};
  
  class DeviceManager
  {
  private:
      std::unordered_map<std::string, std::vector<Device *>> m_Devices;
  public:
      const std::unordered_map<std::string, std::vector<Device *>> &GetDevices() const
      {
          return m_Devices;
      }
  };
  
  int main()
  {
      DeviceManager dm;
      const std::unordered_map<std::string, std::vector<Device *>> &devices = dm.GetDevices();//不使用auto
      const auto& devices = dm.GetDevices(); //使用auto
  
      std::cin.get();
  }
  ```

+ 当类型名过长的时候可以使用using或者typedef：

  ```cpp
  using DeviceMap = std::unordered_map<std::string, std::vector<Device*>>;
  typedef std::unordered_map<std::string, std::vector<Device*>> DeviceMap;
  ```

### c++静态数组

+ stl库的一部分，创建时指定大小，长度不可变；

  ```c++
  #include <array>  // 先要包含头文件
  int main() {
      std::array<int, 5> data;  //定义，有两个参数，一个指定类型，一个指定大小
      data[0] = 1;
      data[4] = 10;
      return 0;
  }
  ```

+ array和原生数组都是创建在栈上的（vector是在堆上创建底层数据储存的）(这里是指数组控制的内存在哪里而不是对象存放在哪里，比如一个vector数组在栈上创建，那么这个对象就在栈上，但是指向堆中的一系列内存)

+ 原生数组越界的时候不会报错，而array会有越界检查，会报错提醒，且array可以访问大小。

  ```c++
  #include<iostream>
  #include<array>
  
  void PrintArray(const std::array<int, 5>& data)  //指定大小
  {
      for (int i = 0;i < data.size();i++)  //访问大小
      {
          std::cout << data[i] << std::endl;
      }
  }
  int main()
  {
      std::array<int, 5> data;
      data[0] = 0;
      data[1] = 1;
      data[2] = 2;
      data[3] = 3;
      data[4] = 4;
      PrintArray(data);
      std::cin.get();
  }
  }
  ```

+ 传入未知大小的array数组作为参数（cherno的问答）

  ```c++
  #include <iostream>
  #include <array>
  //其实这里是未知大小，未知类型，都用模板处理
  template <typename T>
  void printarray(const T &data)
  {
      for (int i = 0; i < data.size(); i++)
      {
          std::cout << data[i] << std::endl;
      }
  }
  
  template <typename T, unsigned long N> // or // template <typename T, size_t N>
  void printarray2(const std::array<T, N> &data)
  {
      for (int i = 0; i < N; i++)
      {
          std::cout << data[i] << std::endl;
      }
  }
  
  int main()
  {
      std::array<int, 5> data;
      data[0] = 2;
      data[4] = 1;
      printarray(data);
      printarray2(data);
  }
  //代码参考：https://github.com/UrsoCN/NotesofCherno/blob/main/Cherno57.cpp
  ```

### c++的函数指针

+ 函数实际上是CPU的指令，本质是二进制文件，可以获取这些二进制文件的地址，即函数指针

  ```c++
  void HelloWorld()
  {
  	std::cout<<"hello"<<std::endl;
  }
  int main()
  {
  	auto function = &HelloWorld; // & 可省略,其中的 function 可自定义
      //function的类型为 void(*function)()
  	function();
  	std::cin.get();
  }
  ```

  function的类型为 void(*function)()

  有传参时，且用typedef替换时：

  ```cpp
  void HelloWorld(int a)
  {
  	std::cout<<"value: "<<a<<std::endl;
  }
  int main()
  {
  	typedef void(*HelloFunction)(int);
  	HelloFunction function = HelloWorld;
  	function(5);
  	std::cin.get();
  }
  ```

+ 函数指针的用法：可以**作为形参**传入另一个函数并使用。

  ```cpp
  void PrintValue(int value)
  {
  	std::cout<<value<<std::endl;
  }
  void ForEach(const std::vector<int>& values, void(*func)(int))
  {
  	for (int value : values)
  	{
  		func(value);
  	}
  }
  int main()
  {
  	std::vector<int> values = {1,2,3,4,5};
  	ForEach(values,PrintValue);
  	std::cin.get();
  }
  ```

+ lambda表达式：一种匿名的，用完即弃的，在过程中生成的函数；
  格式：`[] ({形参表}) {函数内容}`

  ```c++
  void ForEach(const std::vector<int>& values, void(*function)(int)) {
      for (int temp : values) {
          function(temp);     //正常调用lambda函数
      }
  }
  
  int main() {
      std::vector<int> valus = { 1, 2, 3, 4, 5 };
      ForEach(values, [](int val){ std::cout << val << std::endl; });     //如此简单的事就交给lambda来解决就好了
  }
  ```

### c++中的lambda

+ 在使用函数指针的时候就可以考虑使用lambda表达式，作为函数指针传入某些函数，如此处的std::findif

+ 可以捕获外面的变量，或直接调用lambda函数

  ```c++
  int main()
  {
  	std::vector<int> array =  {1,2,3,4,5};
     //find_if是一个搜索类的函数，区别于find的是：它可以接受一个函数指针来定义搜索的规则，返回满足这个规则的第一个元素的迭代器。
  	auto it = std::find_if(array.begin(),array.end(),[](int v){return v > 3;});
  	std::cout<<*it<<std::endl;
  	std::string s = "value:";
  	auto lambda = [=](int value) {std::cout <<  s << value<< std::endl; };
  	lambda(10);
  	std::cin.get();
  }
  //如果使用捕获,则：
  //添加头文件： #include <functional>
  //修改相应的函数签名 std::function <void(int)> func替代 void(*func)(int)
  //捕获[]使用方式：
  //[=]，则是将所有变量值传递到lambda中
  //[&]，则是将所有变量引用传递到lambda中
  //[a]是将变量a通过值传递，如果是[&a]就是将变量a引用传递
  //它可以有0个或者多个捕获
  //用法详情：https://en.cppreference.com/w/cpp/language/lambda
  ```

+ 使用**mutable**，**允许lambda函数体**修改通过拷贝传递捕获的参数

  ```cpp
  int a = 5;
  auto lambda = [=](int value) mutable { a = 5; std::cout << "Value: " << value << a << std::endl; };
  ```

### 为什么不使用 using namespace std

+ 会难以区分各种函数或者类的来源是否为标准库，且编译器可能无法区分
+ 绝不在头文件中使用 using namespace std，防止他人引用时出现冲突，产生难以最终的错误

### C++的命名空间namespace

+ 相比于c，c++具有命名空间，避免了命名冲突

  ```c++
  #include <iostream>
  #include <string>
  #include <algorithm>
  namespace apple {
      void print(const char *text) {
          std::cout << text << std::endl;
      }
  }
  
  namespace orange {
      void print(const char *text) {
          std::string temp = text;
          std::reverse(temp);
          std::cout << temp << std::endl;
      }
  }
  int main() {
      //using namespace apple::print; //单独引出一个print函数
      //using namespace apple;//引出apple名称空间的所有成员
  
      apple::print("hello");  //输出正常text
      orange::print("world"); //输出反转的text
  }
  ```

+ 命名空间可以嵌套，在c++新标准中使用inline内联命名空间可以代替嵌套，不需要使用using语句就可以直接在外层命名空间使用该命名空间内部的内容，而且无需使用命名空间前缀

  ```c++
  namespace foo {
      namespace bar {
          class Cat { /*...*/ };
      }
  }
  
  // 调用方式
  foo::bar::Cat
   
  //新标准--------------------------------------------------------------------------------------------------
  inline namespace FifthEd {
      // ...
  } 后续再打开命名空间的时候可以写inline也可以不写
  namespace FifthEd {  
      // ...
  }
  // 两处声明的命名空间同名，它们同属一个命名空间，inline必须出现在命名空间第一次出现的地方，后续再打开命名空间的时候可以写inline也可以不写，为隐式内敛
  //直接使用foo::调用成员
  ```

+ 命名空间可以匿名，几个匿名的命名空间互相独立，且作用域与其所在的作用域相同，故应注意避免冲突

  ```c++
  // i的全局声明
  int i;
  // i在未命名的命名空间中的声明
  namespace {
      int i;  
  }
  // 二义性错误: i的定义既出现在全局作用域中, 又出现在未嵌套的未命名的命名空间中
  i = 10;
  ```

  （*未命名的命名空间取代文件中的静态声明：*
  *在标准C++引入命名空间的概念之前，程序需要将名字声明成`static`的以使其对于整个文件有效。在文件中进行静态声明的做法是从C语言继承而来的。在C语言中，声明为`static`的全局实体在其所在的文件外不可见。* ***在文件中进行静态声明的做法已经被C++标准取消了，现在的做法是使用未命名的命名空间。***）

### c++中的线程

+ 线程是操作系统能够进行运算调度的最小单位，它被包含在进程之中，进程包含一个或者多个线程。C++多线程使用多个函数实现各自功能，然后将不同函数生成不同线程，并同时执行这些线程（不同线程可能存在一定程度的执行先后顺序，但总体上可以看做同时执行

+ 使用线程使得程序执行更加清晰，速度更快，在大型项目中十分重要。

+ c++多线程使用案例

  ```cpp
  #include<iostream>
  #include<thread>
  static bool is_Finished = false;
  void DoWork()
  {
  	using namespace std::literals::chrono_literals; // 为 1s 提供作用域
  	std::cout << "Started thread ID: "<<std::this_thread::get_id()<<std::endl;
  	while (!is_Finished)
  	{
  		std::cout<<"Working..."<<std::endl;
  		std::this_thread::sleep_for(1s);//等待1s
  	}
  }
  int main()
  {
  	std::thread worker(DoWork);//创建线程
  	std::cin.get(); // 同时阻塞主线程
  	is_Finished = true;// 让worker线程终止的条件
  	worker.join();// 让主线程等待worker线程结束后继续执行
  	std::cout << "Finished thread ID: " << std::this_thread::get_id() << std::endl;
  	std::cin.get();
  }
  ```

### C++的计时

+ c++可以使用计时来监控程序执行的时间，对象生存的时间，评判算法优劣等等。

+ 计时有两种选择，一种是用平台特定的API，另一种是用std::chrono（推荐）;建立一个Timer类，分别在其构造函数和析构函数中显示时间并计算出时间差，以此记录此作用域的生存时间：

  ```cpp
  struct Timer   //写一个计时器类。
  {
      std::chrono::time_point<std::chrono::steady_clock> start, end;
      std::chrono::duration<float> duration;
  
      Timer()
      {
          start = std::chrono::steady_clock::now(); //如果使用auto关键字会出现警告
      }
  
      ~Timer()
      {
          end = std::chrono::steady_clock::now();
          duration = end - start;
  
          float ms = duration.count() * 1000;
          std::cout << "Timer took " << ms << " ms" << std::endl;
      }
  };
  void Function()
  {
      Timer timer;
      for (int i = 0; i < 100; i++)
          std::cout << "Hello\n"; //相比于std::endl更快
  }
  
  int main()
  {
      Function();
  }
  ```

### c++多维数组

+ 实质上就是在堆上分配空间，储存指针的指针的指针（指针*N）即n维数组(这些指针均为数组的首地址)

  ```c++
  int* array = new int[50];//一维
  int** a2d = new int* [50];//二维
  int*** a3d = new int** [50];//三维
  for (int i = 0; i < 50; i++)
  	a2s = new int[50];
  
  for (int i = 0; i < 50; i++)
  	delete[] a2d[i];
  delete[] a2d;
  //删除多维数组一定要小心，必须在每一维中分别删除，避免仅仅删除一维的而造成内存泄漏
  ```

+ 由于在堆上分配空间，基本上不会是连续的内存，因此多维数组的性能往往不如一维数组

  优化：把二维数组转化成一维数组来进行存储(多维数组)

  ```c++
  int *array = new int[6 * 5];  //模拟二维
      for (int y = 0; y < 5; y++)   //数组优化，将二维数组转化为一维数组
      {
          for (int x = 0; x < 6; x++)
          {
              array[y * 5 + x] = 2;
          }
      }
  
      std::cin.get();
  }
  ```

### C++内置的排序函数

+ sort( vec.begin(), vec.end(), 谓语)

  此处谓语用于指明排序方法（若无谓语默认升序），谓语可以是lambda表达式或者内置函数

  ```c++
  #include<iostream>
  #include<vector>
  #include<algorithm>
  #include<functional>
  
  int main()
  {
      std::vector<int>  values = {3, 5, 1, 4, 2};             
      std::sort(values.begin(), values.end(),std::greater<int>()); //内置函数
      std::sort(values.begin(), values.end(), [](int a, int b)
      {
              return a < b;
      });//lambda表达式，其中参数a，b按次序前后对应
      for (int value : values)
      std::cout << value << std::endl; // 5 4 3 2 1
      std::cin.get();
  }
  ```

### C++的类型双关(type punning)

+ 将同一块内存的东西通过不同type的指针给取出来.

  ```c++
  #include <iostream>
  int main()
  {
      int a = 50;
      double value = *(double*)&a;//此处产生了新的变量，但是也是拷贝了不属于原变量的未定义的多余4个字节
      std::cout << value << std::endl;
  
      std::cin.get();
  }
  //可以用引用，这样就可以避免拷贝成一个新的变量，这样更糟糕
  #include <iostream>
  int main()
  {
      int a = 50;
      double& value = *(double*)&a;
      std::cout << value << std::endl;
  
      std::cin.get();
  }
  ```

+ 结构体的内存分布甚至可以看成数组,而指针的类型决定了地址的单位偏移量

  ```c++
  #include <iostream>
  struct Entity
  {
      int x, y;
  };
  
  int main()
  {
      Entity e = {5, 8};
      int *position = (int *)&e;
      std::cout << position[0] << ", " << position[1] << std::endl;
  
      int y = *(int *)((char *)&e + 4);
      std::cout << y << std::endl;
  }
  ```

### C++的联合体( union )

+ 使用方法`union { };`，结尾有分号。通常union是匿名使用的，但是匿名union不能含有成员函数

+ union的特点是共用内存 。可以像使用结构体或者类一样使用它们，也可以给它添加静态函数或者普通函数、方法等。

  ```c++
  //案例
  #include<iostream>
  #include<vector>
  #include<algorithm>
  struct vec2
  {
  	float x,y;
  };
  struct vec4
  {
  	union
  	{
  		struct
  		{
  			float x,y,z,w;
  		};
  		struct
  		{
  			vec2 a,b;
  		};
  	};
  };
  void PrintVec2(const vec2& vec)
  {
  	std::cout<<vec.x<<","<<vec.y<<std::endl;
  }
  int main()
  {
  	vec4 vector = {1.0f,2.0f,3.0f,4.0f};
  	PrintVec2(vector.a); // 输出 1，2
  	PrintVec2(vector.b); // 输出 3，4
  	vector.z = 10.0f;
  	PrintVec2(vector.a); // 输出 1，2
  	PrintVec2(vector.b); // 输出 10，4
  	std::cin.get();
  }
  ```

### C++的虚析构函数

+ 用子类创建对象定义父类对象，必须指明父类对象的析构函数是virtual的（即指明还会有一个析构函数需要被调用），否则在析构时只会调用父类析构函数，不会调用子类虚构函数，此时很可能会造成内存泄漏（子类虚构函数的工作未完成，内存未释放）

  ```c++
  #include <iostream>
  
  class Base
  {
  public:
      Base() { std::cout << "Base Constructor\n"; }
      virtual ~Base() { std::cout << "Base Destructor\n"; }
  };
  
  class Derived : public Base
  {
  public:
      Derived()
      {
          m_Array = new int[5];
          std::cout << "Derived Constructor\n";
      }
      ~Derived()
      {
          delete[] m_Array;
          std::cout << "Derived Destructor\n";
      }
  
  private:
      int *m_Array;
  };
  
  int main()
  {
      Base *base = new Base();
      delete base;
      std::cout << "-----------------" << std::endl;
      Derived *derived = new Derived();
      delete derived;
      std::cout << "-----------------" << std::endl;
      Base *poly = new Derived();
      delete poly; 
      // Base Constructor
      // Base Destructor
      
      // Base Constructor
      // Derived Constructor
      // Derived Destructor
      // Base Destructor
      
      // Base Constructor
      // Derived Constructor
      // Derived Destructor 
      // Base Destructor
  }
  ```

  > 引自B站评论区：
  > 此处这位外国友人说错了，定义基类的虚析构并不是什么相加，而是：**基类中只要定义了虚析构（且只能在基类中定义虚析构，子类析构才是虚析构，如果在二级子类中定义虚析构，编译器不认，且virtual失效），在编译器角度来讲，那么由此基类派生出的所有子类地析构均为对基类的虚析构的重写，当多态发生时，用父类引用，引用子类实例时，此时的虚指针保存的子类虚表的地址，该函数指针数组中的第一元素永远留给虚析构函数指针。所以当delete 父类引用时，即第一个调用子类虚表中的子类重写的虚析构函数此为第一阶段。然后进入第二阶段：（二阶段纯为内存释放而触发的逐级析构与虚析构就没有半毛钱关系了）而当子类发生析构时，子类内存开始释放，因内存包涵关系，触发父类析构执行，层层向上递进，至到子类所包涵的所有内存释放完成。**

### c++中的类型转换

+  c++四种类型转换：`static_cast` `dynamic_cast` `reinterpret_cast` `const_cast`

+ static_cast用于进行比较“自然”和低风险的转换，如整型和浮点型、字符型之间的互相转换,不能用于指针类型的强制转换

  ```c++
  double dPi = 3.1415926;
  int num = static_cast<int>(dPi);  //num的值为3
  double d = 1.1;
  void *p = &d;
  double *dp = static_cast<double *>(p);
  ```

+ reinterpret_cast 用于进行各种不同类型的指针之间强制转换,通常为运算对象的位模式提供较低层次上的重新解释。

  ```cpp
  int *ip;
  char *pc = reinterpret_cast<char *>(ip);
  ```

+ const_cast 添加或者移除const性质

  注：这种const必须为底层const（例如常量指针是一个底层const，指向可改，指向的值不可改；指针常量是一个顶层const，指针指向不可改，指向的值可改）

  ```cpp
  const string &shorterString(const string &s1, const string &s2)
  {
      return s1.size() <= s2.size() ? s1 : s2;
  }
  
  //上面函数返回的是常量string引用，当需要返回一个非常量string引用时，可以增加下面这个函数
  string &shorterString(string &s1, string &s2) //函数重载
  {
      auto &r = shorterString(const_cast<const string &>(s1), 
                              const_cast<const string &>(s2));
      return const_cast<string &>(r);
  }
  ```

+ dynamic_cast 安全地在继承体系里面向上、向下或横向转换指针和引用的类型，多态转换；不检查转换安全性，仅运行时检查，如果不能转换，返回NULL

  ```c++
  #include <iostream>
  class Base
  {
  public:
      Base() { std::cout << "Base Constructor\n"; }
      virtual ~Base() { std::cout << "Base Destructor\n"; }
  };
  
  class Derived : public Base
  {
  public:
      Derived()
      {
          m_Array = new int[5];
          std::cout << "Derived Constructor\n";
      }
      ~Derived()
      {
          delete[] m_Array;
          std::cout << "Derived Destructor\n";
      }
  
  private:
      int *m_Array;
  };
  
  class AnotherClass : public Base
  {
  public:
      AnotherClass(){};
      ~AnotherClass(){};
  };
  
  int main()
  {
      // double value = 5.25;
      // // int a = value;
      // // int a = (int)value;
      // double a = (int)value + 5.3; // 10.3 // C style cast here
  
      // double s = static_cast<int>(value) + 5.3; // C++ style cast here
  
      // std::cout << a << std::endl;
      // std::cout << s << std::endl;
  
      Derived *derived = new Derived();
  
      Base *base = derived;
  
      // AnotherClass *ac = static_cast<AnotherClass*>(base);  //NULL
      Derived *ac = dynamic_cast<Derived *>(base);
  
      delete derived;
  }
  ```

### C++预编译头文件

+ 在编译阶段，被引入的头文件每一次都要重新编译一遍，编译时间会很长。预编译头文件即是将头文件预先编译为二进制文件，如果头文件此后不需要修改，在编译时就直接用编译好的二进制文件，可以大大缩短编译时间。一些不大会改变的文件也可以放入预编译用于缩短运行时间。

  流程：例子：//引用大佬的笔记，原链接在此[Cherno的C++教学视频笔记（已完结） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/352420950)

  > 新建一个工程和解决方案，添加Main.cpp,pch.cpp,pch.h三个文件，内容分别如下：

  ```cpp
  // Main.cpp
  #include"pch.h"
  
  int main()
  {
  	std::cout<<"Hello!"<<std::endl;
  	std::cin.get();
  }
  // pch.cpp
  #include"pch.h"
  // pch.h
  #pragma once
  #include<iostream>
  #include<vector>
  #include<memory>
  #include<string>
  #include<thread>
  #include<chrono>
  #include<unordered_map>
  #include<Windows.h>
  ```

  > 在pch.cpp右键，属性-配置属性-C/C++-预编译头-预编译头，里面选择创建, 并在下一行预编译头文件里面添加 pch.h
  >
  > 在项目名称上右键，属性-配置属性-C/C++-预编译头-预编译头，里面选择使用，并在下一行预编译头文件里面添加 pch.h
  >
  > 打开计时工具：工具-选项-项目和解决方案-VC++项目设置-生成计时，就可以看到每次编译的时间
  >
  > 进行对比：
  >
  > 进行预编译头文件前后的首次编译耗时分别为：2634ms和1745ms
  >
  > 进行预编译头文件前后的二次编译（即修改Main.cpp内容后）的耗时分别为：1235ms和312ms
  >
  > 可以看到进行预编译头文件后，时间大大降低

### c++中的dynamic_cast 详解

+ dynamic_cast 安全地在继承体系里面向上、向下或横向转换指针和引用的类型，多态转换；

+ 不检查转换安全性，仅运行时检查，如果不能转换，返回NULL

+ dynamic_cast只用于多态类类型，即基类必须有虚函数或者说虚表

+ dynamic_cast使用了RTTI（运行时类型识别）,即存储了所有类型的运行时类型信息用与判断（也储存了更多自身的信息以及检查类型是否匹配所以会增加一些开销）

  ```c++
  #include<iostream>
  class Base
  {
  public:
  	virtual void print(){}
  };
  class Player : public Base
  {
  };
  class Enemy : public Base
  {
  };
  int main()
  {
  	Player* player = new Player();
  	Base* base = new Base();
  	Base* actualEnemy = new Enemy();
  	Base* actualPlayer = new Player();
  
  	// c风格
  	Base* pb1 = player; // 隐式转换
  	Player*  bp1 = (Player*)base; // 显式转换，危险
  	Enemy* pe1 = (Enemy*)player; // 显式转换，危险
  
  	// dynamic_cast
  	Base* pb2 = dynamic_cast<Base*>(player); 
  	Player* bp2 = dynamic_cast<Player*>(base); // 返回NULL
  
  	Enemy* pe2 = dynamic_cast<Enemy*>(player); // 同级转换，失败返回NULL
  	Player* aep = dynamic_cast<Player*>(actualEnemy); // 同级转换，失败返回NULL
  	Player* app = dynamic_cast<Player*>(actualPlayer); // 成功
      
      //转换结果可作为判断条件
      if(bp2) { } 
  }
  ```

### C++的基准测试

+ 使用<chrono>进行计时以进行基准测试，衡量代码的优劣

  ```c++
  #include <iostream>
  #include <memory>
  #include <chrono>  
  #include <array>
  class Timer {
  public:
      Timer() {
          m_StartTimePoint =  std::chrono::high_resolution_clock::now();//启动定时器的时间
      }
      ~Timer() {
          Stop();
      }
      void Stop() {
          auto endTimePoint = std::chrono::high_resolution_clock::now();//停止定时器的时间
          auto start = std::chrono::time_point_cast<std::chrono::microseconds>(m_StartTimePoint).time_since_epoch().count();
          //microseconds 将数据转换为微秒
          //time_since_epoch() 测量自时间起始点到现在的时长
          auto end = std::chrono::time_point_cast<std::chrono::microseconds>(endTimePoint).time_since_epoch().count();
          auto duration = end - start;
          double ms = duration * 0.001; ////转换为毫秒数
          std::cout << duration << "us(" << ms << "ms)\n";
      }
  private:
      std::chrono::time_point<std::chrono::high_resolution_clock> m_StartTimePoint;
  };
  
  int main() 
  {
      struct Vector2 {
          float x, y;
      };
  
      {
          std::array<std::shared_ptr<Vector2>, 1000> sharedPtrs;
          Timer timer;
          for (int i = 0; i < sharedPtrs.size(); i++) {
              sharedPtrs[i] = std::make_shared<Vector2>();
          }
      }
  
      {
          std::array<std::shared_ptr<Vector2>, 1000> sharedPtrs;
          Timer timer;
          for (int i = 0; i < sharedPtrs.size(); i++) {
              sharedPtrs[i] = std::shared_ptr<Vector2>(new Vector2());
          }
      }
  
      {
          Timer timer;
          std::array<std::unique_ptr<Vector2>, 1000> sharedPtrs;
          for (int i = 0; i < sharedPtrs.size(); i++) {
              sharedPtrs[i] = std::make_unique<Vector2>();
          }
      }
  ```

### C++的结构化绑定(Structured Binding)(c++17)

+ 结构化绑定是c++17的新特性，可以用于处理多返回值，可以将tuple、pair、struct结构的成员直接返回，而不是返回结构。这样返回多值时无需创建新的结构，更加清晰简便。

  ```c++
  #include <iostream>
  #include <string>
  #include <tuple>
  
  
  std::tuple<std::string, int> CreatPerson() 
  {
      return {"Cherno", 24};
  }
  
  int main()
  {
      auto[name, age] = CreatPerson(); 
      std::cout << name;
  ```

### 如何处理optional数据(std::optional)(c++17)

+ std::optional类型可以是某类型的对象，也可以是什么都没有。有一种应用情况是，使用optional来判断读取文件是否成功。

  ```c++
  //判断文件是否读取成功：
  //不使用optional：----------------------------------------------------------------------------------------  
  #include <iostream>
  #include <fstream>
  #include <string>
  std::string ReadFile(const std::string &filepath, bool &outSuccess) {
      std::ifstream stream(filepath);
      if (stream) {
          std::string result;
          getline(stream,result);
          stream.close();
          outSuccess = true;  
          return result;
      }
      outSuccess = false; 
      return std::string();
  }
  //通过修改bool值来判断是否读取成功
  int main() {
      bool flag;
      auto data = ReadFile("data.txt", flag);
      if (flag) {
  
      }
  }
  //使用optional-------------------------------------------------------------------------------------------
  #include <iostream>
  #include <fstream>
  #include <optional>
  #include <string>
  
  std::optional<std::string> ReadFileAsString(const std::string& filepath)
  {
      std::ifstream stream(filepath);
      if (stream)
      {
          std::string result;
          getline(stream, result);
          stream.close();
          return result;
      }
      return {};//返回一个optional空值
  }
  
  int main()
  {
       std::optional<std::string> data = ReadFileAsString("data.txt");
      if (data.has_value())
      {   
          std::cout << "File read successfully!" << data.value() << std::endl;      
      }
      else
      {
          std::cout << "File could not be opened!" << std::endl;
      }
  
      std::cin.get();
      //或者使用value_or()方法
      //std::string result = data.value_or("Not resprent");(自定义默认值输出)
  	//std::cout<<result<<std::endl;
  	//std::cin.get();
  }
  
  ```

### 单一变量存放多种类型的数据std::variant（c++17）

+ 相比union，variant更加类型安全，不会造成未定义行为，它和结构体十分类似，实际占用内存要比union大

+ std::variantt的大小是所有属性大小之和，而union的大小是最大属性大小。

  ```cpp
  #include<iostream>
  #include<variant>
  int main()
  {
  	std::variant<std::string,int> data; // <>里面的类型不能重复
  	data = "cherno";
  	// 索引的第一种方式：std::get，但是要与上一次赋值类型相同，不然会报错
  	std::cout<<std::get<std::string>(data)<<std::endl;
  	// 索引的第二种方式，std::get_if，传入地址，会返回为指针
      //也可以使用index()来判断当前索引属性
  	if (auto value = std::get_if<std::string>(&data))
  	{
  		std::string& v = *value;
  	}
  	data = 2;
  	std::cout<<std::get<int>(data)<<std::endl;
  	std::cin.get();
  }
  ```

### C++如何存储任意类型的数据(std::any)（c++17）

+ 也是C++17引入的可以存储多种类型变量的结构，但是不像std::variant那样需要列出类型

+ 就运行机制来说，对于小类型(small type)，any将它们存储为一个严格对齐的Union， 对于大类型，会用void*，动态分配内存 

+ any一般很少用。

  ```cpp
  include <iostream>
  #include <any>
  // 这里的new的函数，是为了设置一个断点，通过编译器观察主函数中何处调用了new，看其堆栈。
  void *operator new(size_t size)
  {
      return malloc(size);
  }
  
  int main()
  {
      std::any data;
      data = 2;
      data = "Cherno";
      data = std::string("Cherno");
  
      std::string& string = std::any_cast<std::string&>(data); //用any_cast指定转换的类型,如果这个时候any不是想要转换的类型，则会抛出一个类型转换的异常
      // 通过引用减少复制操作，以免影响性能
  }
  ```

### 如何让C++运行得更快(std::async)

+ 使用std::async封装异步编程

### 如何让C++字符串更快(std::string_view)

+ std::string以及他的许多函数都习惯在堆上分配内存，这会降低程序的速度。

  ```c++
  #include<iostream>
  #include<string>
  static uint32_t s_AllocCount = 0;
  //调试技巧：对new操作符进行重载，可以记录开辟内存的行为以及大小
  void* operator new(size_t size)
  {
  	s_AllocCount++;
  	std::cout<<"Allocing: "<<size<<" bytes\n";
  	return malloc(size);
  }
  void PrintName(const std::string& name)
  {
  	std::cout<<name<<std::endl;
  }
  int main()
  {
  	std::string fullName = "Yan Chernikov";
  	std::string firstName = fullName.substr(0,4);
  	std::string lastName = fullName.substr(5,8);
  	PrintName(firstName);
  	PrintName(lastName);
  	std::cout<<s_AllocCount<<" allocations\n";
  	std::cin.get();
  }
  Allocing: 8 bytes
  //Allocing : 8 bytes
  //Allocing : 8 bytes
  //Yan
  //Cherno
  //3 allocations
  //此处分配了三处内存，可见std::string分配内存很频繁，注意：即使仅仅是引用操作都会开辟内存
  ```

+ 使用c++17新特性string_view类型可以避免字符串开辟内存，它要做的类似于“观察”一个已经存在的字符串

  ```c++
  #include<iostream>
  #include<string>
  static uint32_t s_AllocCount = 0;
  void* operator new(size_t size)
  {
  	s_AllocCount++;
  	std::cout<<"Allocing: "<<size<<" bytes\n";
  	return malloc(size);
  }
  void PrintName(std::string_view name)
  {
  	std::cout<<name<<std::endl;
  }
  int main()
  {
  	std::string fullName = "Yan Chernikov";
  	std::string_view firstName(fullName.c_str(),4);
  	std::string_view lastName(fullName.c_str()+5,8);
  	PrintName(firstName);
  	PrintName(lastName);
  
  	std::cout<<s_AllocCount<<" allocations\n";
  	std::cin.get();
  }
  
  //Allocing: 8 bytes
  //Yan
  //hernikov
  //1 allocations
  //仅仅分配一次
  ```

+ 将std::string改为存粹的c风格字符串const char*后分配零次内存，再次优化

  ```c++
  #include<iostream>
  #include<string>
  static uint32_t s_AllocCount = 0;
  void* operator new(size_t size)
  {
  	s_AllocCount++;
  	std::cout<<"Allocing: "<<size<<" bytes\n";
  	return malloc(size);
  }
  void PrintName(std::string_view name)
  {
  	std::cout<<name<<std::endl;
  }
  int main()
  {
  	const char* fullName = "Yan Chernikov";
  	std::string_view firstName(fullName,4);//删去.c_str()
  	std::string_view lastName(fullName+5,8);
  	PrintName(firstName);
  	PrintName(lastName);
  
  	std::cout<<s_AllocCount<<" allocations\n";
  	std::cin.get();
  }
  //Yan
  //hernikov
  //0 allocations
  //0次分配
  ```

### C++的可视化基准测试(Chrome://tracing工具)

+ Chrome://tracing，谷歌的一个调试工具，可以清晰地用可视化的方法展示代码运行的情况
+ cherno演示了具体操作方法，并使用宏定义控制计时代码的有效性，以及函数传参的自动化，以及多线程调试的具体方法

+ ```c++
  #pragma once
  #include <string>
  #include <chrono>
  #include <algorithm>
  #include <fstream>
  #include <cmath>
  #include <thread>
  #include <iostream>
  
  
  struct ProfileResult
  {
      std::string Name;
      long long Start, End;
      uint32_t ThreadID; //线程ID
  };
  struct InstrumentationSession
  {
      std::string Name;
  };
  class Instrumentor//此类用于格式化一个json文件并将其写入一个文件
  {
  private:
      InstrumentationSession* m_CurrentSession;
      std::ofstream m_OutputStream;
      int m_ProfileCount;
  public:
      Instrumentor()
          : m_CurrentSession(nullptr), m_ProfileCount(0)
      {
      }
  
      void BeginSession(const std::string& name, const std::string& filepath = "results.json")//指定文件名创建新文件
      {
          m_OutputStream.open(filepath);//打开一个文件
          WriteHeader();//写文件头
          m_CurrentSession = new InstrumentationSession{ name };
      }
  
      void EndSession()
      {
          WriteFooter();//写文件页脚
          m_OutputStream.close();
          delete m_CurrentSession;
          m_CurrentSession = nullptr;
          m_ProfileCount = 0;
      }
  
      void WriteProfile(const ProfileResult& result)//整个类的核心，用于单独写时间分析数据
      {
          if (m_ProfileCount++ > 0)
              m_OutputStream << ",";
  
          std::string name = result.Name;
          std::replace(name.begin(), name.end(), '"', '\'');
  
          m_OutputStream << "{";
          m_OutputStream << "\"cat\":\"function\",";
          m_OutputStream << "\"dur\":" << (result.End - result.Start) << ',';
          m_OutputStream << "\"name\":\"" << name << "\",";
          m_OutputStream << "\"ph\":\"X\",";
          m_OutputStream << "\"pid\":0,";
          m_OutputStream << "\"tid\":" << result.ThreadID << ","; //多线程
          m_OutputStream << "\"ts\":" << result.Start;
          m_OutputStream << "}";
  
          m_OutputStream.flush();//一旦输出更多json进入输入流就刷新，以防程序终止或崩溃而丢失数据
      }
  
      void WriteHeader()
      {
          m_OutputStream << "{\"otherData\": {},\"traceEvents\":[";
          m_OutputStream.flush();
      }
  
      void WriteFooter()
      {
          m_OutputStream << "]}";
          m_OutputStream.flush();
      }
  
      static Instrumentor& Get()
      {
          static Instrumentor instance;
          return instance;
      }
  };
  //Instrumentation（插桩）指注入代码进行分析
  class InstrumentationTimer
  {
  public:
      InstrumentationTimer(const char* name)
  	:m_Name(name),m_Stopped(false)
  	{
  		m_StartTimepoint = std::chrono::high_resolution_clock::now();
  	}
  	void Stop()
  	{
  		auto endTimepoint = std::chrono::high_resolution_clock::now();
  		long long start = std::chrono::time_point_cast<std::chrono::microseconds>(m_StartTimepoint).time_since_epoch().count();
  		long long end = std::chrono::time_point_cast<std::chrono::microseconds>(endTimepoint).time_since_epoch().count();
  		std::cout << m_Name << ":" << (end - start) << "ms"<<std::endl;
          uint32_t threadID = std::hash<std::thread::id>{}(std::this_thread::get_id());//c++标准库使得作为一个整数取得线程ID十分困难，但是可以使用线程ID实现的一个hash函数转换
          Instrumentor::Get().WriteProfile({ m_Name,start,end,threadID });//将线程id写入正确文件中
          m_Stopped = true;
  	}
  	~InstrumentationTimer() {
  		if (!m_Stopped)
  		{
  			Stop();
  		}
  	
  
  	}
  private:
  	const char* m_Name;
  	//std::chrono::time_point_cast<std::chrono::milliseconds>(m_StartTimepoint).time_since
  	std::chrono::time_point<std::chrono::steady_clock> m_StartTimepoint;
  	bool m_Stopped;
  };
  #define PROFILING 1//使用宏定义控制对于计时器代码的使用
  #if PROFILING 
  #define PROFILE_SCOPE(name)  InstrumentationTimer timer##__LINE__(name)
  //可以宏定义#define PROFILE_FUNCTION() PROFILE_SCOPE(__FUNCTION__)以传入函数名进行替换，注意__FUNCTION__只传入函数名而非函数签名,因此相同函数的重载无效（函数签名应用__FUNCSIG__）
  //甚至可以在外面添加一个作用域，由于宏定义__FUNCSIG__，也可以得到作用域的命名空间
  //总之，在想分析的地方使用宏定义，简洁清晰
  #else 
  #define PROFILE_SCOPE(name)
  #endif
  
  void Function1()
  {
      PROFILE_SCOPE("Function1");
  	for (int i = 0; i < 1000; i++)
  	{
  		std::cout << "hello world #" << i << std::endl;
  	}
  }
  void Function2()
  {
      PROFILE_SCOPE("Function2");//那样这里即可直接替换为 PROFILE_FUNCTION()由预处理器直接完成而不需要传入字符串
  	for (int i = 0; i < 1000; i++)
  	{
  		std::cout << "hello world #" << sqrt(i) << std::endl;
  	}
  }
  void RunBenchmarks()
  {
      PROFILE_SCOPE("RunBenchmarks");
      std::cout << "runbenchmarks...\n";
      std::thread a([] {Function1(); });//使用lambda表达式，稍微改变benchmark，让其使用多线程
      std::thread b([] {Function2(); });
      a.join();
      b.join();
  }
  int main()
  {
      Instrumentor::Get().BeginSession("profile");
      RunBenchmarks();
     
      Instrumentor::Get().EndSession();
  	std::cin.get();
  }
  
  ```

### c++的单例模型

+ 类存在的意义就是可以被多次实例化，但有时我们只想要一个单一实例，只有单一的数据集。然后可能会有一些功能，同时具备代表数据的类成员变量，代表对特定数据集执行操作的成员函数。

  ```c++
  #include<iostream>
  class Singleton
  {
  public:
  	Singleton(const Singleton&) = delete; // 删除拷贝复制函数
  	static Singleton& Get() 
  	{
  		return s_Instance;
  	}
  	void Function(){}//单例模型可以实现功能，可以具有成员变量
  private:
  	Singleton(){} // 构造函数写为private，无法实例化
  	static Singleton s_Instance; 
  };
  Singleton Singleton::s_Instance; // 此处实例化
  int main()
  {
  	Singleton::Get().Function();
  }
  ```

+ 简单的随机数类的例子

  ```c++
  #include<iostream>
  class Random
  {
  public:
      Random(const Random&) = delete; // 删除拷贝复制函数
      static Random& Get() // 通过Get函数来获取唯一的一个实例
      {
          static Random instance; // 此处实例化一次
          return instance;
      }
      static float Float(){ return Get().IFloat();} // 调用内部函数
  private:
      float IFloat() { return m_RandomGenerator; } // 函数实现放进private
      Random(){} 
      float m_RandomGenerator = 0.5f;
  };
  // 与namespace很像
  namespace RandomClass {
      static float s_RandomGenerator = 0.5f;
      static float Float(){return s_RandomGenerator;}
  }
  int main()
  {
      float randomNum = Random::Float();
      std::cout<<randomNum<<std::endl;
      std::cin.get();
  }
  ```


### C++的小字符串优化

+ VS中使用std::string时，若字符串size小于16则在release模式下不会在堆上分配内存，而是会在栈上缓存（debug模式下都会分配）。

  ```c++
  #include<iostream>
  void* operator new(size_t size)
  {
  	std::cout<<"Allocated: "<<size<<" bytes\n";
  	return malloc(size);
  }
  int main()
  {
  	std::string longName = "chernooooooooooo";
  	std::string shortName = "cherno";
  	std::cin.get();
  }
  //Allocated: 32 bytes
  
  ```

### c++跟踪内存分配的简单方法

+ 重写new和delete操作符函数，并在里面打印分配和释放了多少内存，也可在重载的这两个函数里面设置断点，通过查看调用栈即可知道什么地方分配或者释放了内存(注意这里重写的并不是表达式new和delete，二十向下分解的new和delete操作符，它们要做的是分配相关内存)

  ```c++
  void* operator new(size_t size)
  {
  	std::cout << "Allocing " << size << " bytes\n";
  	return malloc(size);
  }
  
  void operator delete(void* memory, size_t size)
  {
  	std::cout << "Free " << size << " bytes\n";
  	free(memory);
  }
  
  struct Entity
  {
  	int x, y, z;
  };
   
  int main()
  {
  	{
  		std::string name = "cherno";
  	}
  	Entity* e = new Entity();
  	delete e;
  
  	std::cin.get();
  }
  ```

+ 也可以写一个简单统计内存分配的类，在每次new的时候统计分配内存，在每次delete时统计释放内存，可计算出已经分配的总内存

  ```c++
  #include<iostream>
  struct AllocationMertics
  {
  	uint32_t TotalAllocated = 0;
  	uint32_t TotalFreed = 0;
  	uint32_t CurrentUsage() {return TotalAllocated - TotalFreed;}
  };
  static AllocationMertics s_AllocationMetrics;
  void* operator new(size_t size)
  {
  	s_AllocationMetrics.TotalAllocated+=size;
  	return malloc(size);
  }
  void operator delete(void* memory, size_t size)
  {
  	s_AllocationMetrics.TotalFreed += size;
  	free(memory);
  }
  static void PrintMemoryUsage()
  {
  	std::cout<<"Memory usage: "<<s_AllocationMetrics.CurrentUsage()<<" bytes\n";
  }
  int main()
  {
  	PrintMemoryUsage();
  	{
  		std::string name = "cherno";
  		PrintMemoryUsage();
  	}
  	PrintMemoryUsage();
  	std::cin.get();
  }
  ```

###  C++的左值与右值(lvalue and rvalue)

+ 左值是有某种存储支持的变量，右值是临时值

  ```c++
  //最简单的例子
  int main()
  {
      
      std::string firstName = "Yan";
      std::string lastName = "Chernikov";//等号左右两边分别为左值和右值
      std::string fullname = firstName + lastName;//等号左边是左值，而右边(firstName + lastName)整体是组成的一个右值（临时值）
      return 0;
  }
  ```

+ 左值引用是什么：

  ```c++
  //简单例子
  int GetValue()
  { 
      return 10;
  }
  
  int main()
  {
      int i = GetValue();//函数作为右值，因为仅仅是返回一个“10”，他被赋给右值i
      GetValue() = 5;//错误; 显而易见右值赋给右值是错误的，编译器会提醒GetValue()必须是可修改的左值也就是非const的
      return 0;
  }
  //那如果函数返回的是左值（有某种存储支持的变量）呢？这就引出什么是左值引用
  
  int &GetValue()
  { // 左值引用
      static int value = 10;
      return value;
  }
  
  int main()
  {
      int i = GetValue();
      GetValue() = 5;//由于函数返回左值，可以给它赋右值
      return 0;
  }
  ```

+ 非const左值引用只可接受左值，const左值引用可以同时接受左值和右值；为了接受右值，我们有右值引用，表示接受一个临时值的引用，它不可以接受左值

  ```c++
  #include <iostream>
  
  void SetValue(int value) {}
  
  void PrintName(std::string &name)
  { // 非const的左值引用只接受左值
      std::cout << "[lvalue]" << name << std::endl;
  }
  
  void PrintName(const std::string &&name)
  { // 右值引用不能绑定到左值
      std::cout << "[rvalue]" << name << std::endl;
  }
  
  int main()
  {
      SetValue(i);  // 左值参数调用
      SetValue(10); // 右值参数调用，当函数被调时，这个右值会被用来创建一个左值
  
      // 关于const，const引用可以同时接受左值和右值
      // int& a = 10; // 不能用左值作为右值的引用
      const int &a = 10; // 通过创建一个左值实现
  
      std::string firstName = "Yan";
      std::string lastName = "Chernikov";
      std::string fullname = firstName + lastName;
      PrintName(fullname);             // 接受左值
      PrintName(firstName + lastName); // 接受右值
  
      return 0;
  }
  ```

### C++持续集成（CI）

> CI(Continuous integration，中文意思是持续集成)是一种软件开发时间。持续集成强调开发人员提交了新代码之后，立刻进行构建、（单元）测试。根据测试结果，我们可以确定新代码和原有代码能否正确地集成在一起。 2、主要讲解如何在linode租一个服务器，来运行Jenkins

### static analyze

+ 主要讲了一个工具PVS-studio的用法，可以static analyze代码

- 开源的推荐 clang-tidy

> [如何在VS Code中运行clang-tidy?： https://zhuanlan.zhihu.com/p/446084601](https://zhuanlan.zhihu.com/p/446084601)

### C++的参数计算顺序

+ 在给函数传参时，参数计算顺序可能是无法确定的，不同编译器或c++版本给出的顺序是不同的，这里介绍了一个未确定行为的例子

  ```c++
  #include<iostream>
  void PrintSum(int a, int b)
  {
      std::cout<<a<<"+"<<b<<"="<<a+b<<std::endl;
  }
  int main()
  {
      int value = 0;
      PrintSum(value++,++value); //行为未定义
      std::cin.get();
  }
  ```

### C++移动语义

+ > 移动语义
  >
  >  很多时候我们只是单纯创建一些右值，然后赋给某个对象用作构造函数。这时候会出现的情况是：首先需要在main函数里创建这个右值对象，然后复制给这个对象相应的成员变量。 如果我们可以直接把这个右值变量**移动**到这个成员变量而不需要做一个额外的复制行为，**程序性能**就能**提高**

+ noexcept 指定符:指定函数是否抛出异常。 举例：void f() noexcept {};*// 函数 f() 不抛出*异常

  ```c++
  #include<iostream>
  class String
  {
  public:
  	String() = default;
  	String(const char* string) //构造函数
  	{
  		printf("Created\n");
  		m_Size = strlen(string);
  		m_Data = new char[m_Size];
  		memcpy(m_Data,string,m_Size);
  	}
  	String(const String& other) // 拷贝构造函数
  	{
  		printf("Copied\n");
  		m_Size = other.m_Size;
  		m_Data = new char[m_Size];
  		memcpy(m_Data,other.m_Data,m_Size);
  	}
  	String(String&& other) noexcept // 右值引用拷贝，相当于移动，就是复制一次指针，原来的指针给nullptr
  	{
  		printf("Moved\n");
  		m_Size = other.m_Size;
  		m_Data = other.m_Data;
  
  		other.m_Size = 0;
  		other.m_Data = nullptr;
  	}
  	~String()
  	{
  		printf("Destroyed\n");
  		delete m_Data;
  	}
  private:
  	uint32_t m_Size;
  	char* m_Data;
  };
  
  class Entity
  {
  public:
  	Entity(const String& name) : m_Name(name)
  	{
  	}
  	Entity(String&& name) : m_Name(std::move(name)) // std::move(name)也可以换成(String&&)name
  	{
  	}
  private:
  	String m_Name;
  };
  int main()
  {	
  	Entity entity("cherno");
  	std::cin.get();
  }
  //在实例化entity的时候，如果传入的是字符串常量（右值），则会调用拷贝的右值版本，避免了一次new，如果传入的是String（左值），则仍然会进行一次左值拷贝
  ```

### std::move与移动赋值操作符

+ 使用std::move，返回一个右值引用，可以将本来的copy操作变为move操作(将一个已经存在的对象移动给另一个已经存在的对象)

  ```c++
  #include<iostream>
  class String
  {
  public:
      String() = default;
      String(const char* string)
      {
          printf("Created\n");
          m_Size = strlen(string);
          m_Data = new char[m_Size];
          memcpy(m_Data, string, m_Size);
      }
      String(const String& other) 
      {
          printf("Copied\n");
          m_Size = other.m_Size;
          m_Data = new char[m_Size];
          memcpy(m_Data, other.m_Data, m_Size);
      }
      String(String&& other) noexcept
      {
          printf("Moved\n");
          m_Size = other.m_Size;
          m_Data = other.m_Data;
  
          other.m_Size = 0;
          other.m_Data = nullptr;
      }
      ~String()
      {
          printf("Destroyed\n");
          delete m_Data;
      }
      void Print() {
          for (uint32_t i = 0; i < m_Size; ++i)
              printf("%c", m_Data[i]);
  
          printf("\n");
      }
      String& operator=(String&& other) // 移动复制运算符重载
      {
          printf("Moved\n");
          if (this != &other)
          {
              delete[] m_Data;
  
              m_Size = other.m_Size;
              m_Data = other.m_Data;
  
              other.m_Data = nullptr;
              other.m_Size = 0;
          }
          return *this;
      }
  private:
      uint32_t m_Size;
      char* m_Data;
  };
  class Entity
  {
  public:
      Entity(const String& name) : m_Name(name)
      {
      }
      void PrintName() {
          m_Name.Print();
      }
      Entity(String&& name) : m_Name(std::move(name)) // std::move(name)也可以换成(String&&)name
      {
      }
  private:
      String m_Name;
  };
  int main()
  {
      String apple = "apple";
      String orange = "orange";
  
      printf("apple: ");
      apple.Print();
      printf("orange: ");
      orange.Print();
  
      apple = std::move(orange);
  
      printf("apple: ");
      apple.Print();
      printf("orange: ");
      orange.Print();
      std::cin.get();
  }
  //输出：
  Created
  Created
  apple: apple
  orange: orange
  Moved
  apple: orange
  orange:
  ```

### 自己实现一个简单的Array类

+ Array

  ```c++
  #include<iostream>
  template<typename T,size_t S>
  class Array
  {
  public:
  	constexpr int Size() const {return S;} // const放在成员函数后面，表示函数不能修改值；用constexpr来修饰表示返回值是常量字面值，可以被编译器优化
  	T& operator[](size_t index) {return m_Data[index]; } // 返回引用以对原数据进行修改
  	const T& operator[](size_t index) const {return m_Data[index]; }
  
  	T* Data(){return m_Data;} // 返回数组本身，实际上是个指针，其地址等价于&m_Data[0]
  	const T* Data() const {return m_Data;}
  private:
  	T m_Data[S];
  };
  int main()
  {	
  	Array<int,5> data;
  	memset(&data[0],0,data.Size()*sizeof(int));
  	data[2] = 2;
  	for(size_t i = 0;i<data.Size();i++)
  		std::cout<<data[i]<<std::endl;
  	std::cin.get();
  }
  ```

  

### 自己实现一个简单的Vector类

+ Vector

  ```c++
  template<typename T>
  class Vector
  {
  public:
  	Vector()
  	{
  		// Alloc 2 memory
  		ReAlloc(2); // 初始化构造的时候默认分类两个内存
  	}
  	~Vector()
  	{
  		delete [] m_Data; // 此处写的不完整，还需要释放每个元素的内存
  	}
  	void PushBack(T&& value) // 右值版本，用move
  	{
  		if(m_Size >= m_Capacity) // 发现当前分配内存快要满的时候，再多分配一点内存
  			ReAlloc(m_Capacity + m_Capacity/2);
  		m_Data[m_Size] = std::move(value);
  		m_Size++;
  	}
  	void PushBack(const T& value) // 左值版本，用copy
  	{
  		if (m_Size >= m_Capacity)
  			ReAlloc(m_Capacity + m_Capacity / 2);
  		m_Data[m_Size] = value;
  		m_Size++;
  	}
  	template<typename... Args>
  	T& EmplaceBack(Args&&... args) // Emplace与Push的区别在于，可以在里面调元素类的构造函数
  	{
  		if(m_Size>= m_Capacity)
  			ReAlloc(m_Capacity+m_Capacity/2);
  		new(&m_Data[m_Size]) T(std::forward<Args>(args)...); // 这句话看不太懂
  		// m_Data[m_Size] = T(std::forward<Args>(args)...); //可替代上一句话
  		return m_Data[m_Size++];
  	}
  	void PopBack()
  	{
  		if (m_Size > 0)
  		{
  			m_Size--;
  			m_Data[m_Size].~T(); // 弹出末尾元素，并且释放内存
  		}
  	}
  	void Clear()
  	{
  		for(size_t i = 0;i<m_Size;i++)
  			m_Data[i].~T();
  		m_Size = 0;
  	}
  
  	size_t Size() const { return m_Size;}
  	const T& operator[] (size_t index) const { return m_Data[index]; }
  	T& operator[] (size_t index)  { return m_Data[index]; }
  private:
  	void ReAlloc(size_t newCapacity)
  	{
  		// 1. alloc a new block of memory
  		// 2. copy/move old elements to new block of memory
  		// 3. delete
  		T* newBlock = new T[newCapacity];
  		if(m_Size > newCapacity)
  			m_Size = newCapacity;
  		for(size_t i = 0;i<m_Size;i++)
  			newBlock[i] = std::move(m_Data[i]); // 使用move的方式，避免copy
  		delete[] m_Data;
  		m_Data = newBlock;
  		m_Capacity = newCapacity;
  	}
  
  	T* m_Data = nullptr;
  	size_t m_Size;
  	size_t m_Capacity;
  };
  ```

  

### iterator

+ iterator可用于有序或者无序结构的遍历，如：

  ```c++
  #include<iostream>
  #include<unordered_map>
  int main()
  {	
  	typedef std::unordered_map<std::string,int> ScoreMap;
  	ScoreMap map;
  	map["ydc"] = 1;
  	map["tt"] = 2;
  	for (ScoreMap::iterator it = map.begin(); it != map.end(); it++)
  	{
  		auto& key = it->first;
  		auto& value = it->second;
  		std::cout<<key<<"="<<value<<std::endl;
  	}
  	for (auto& kv : map) // C++11引入的范围for循环
  	{
  		auto& key = kv.first;
  		auto& value = kv.second;
  		std::cout << key << "=" << value << std::endl;
  	}
  	for(auto& [key,value]:map) // C++17引入的structure binding
  		std::cout << key << "=" << value << std::endl;
  
  	std::cin.get();
  }
  ```

  

### 自己写一个iterator类

+ 主要是针对vector写一个iterator类，成员是一个指针，主要在iterator类里实现++,--,!=,[],->等操作符，以及在Vector类里面实现begin(),end()等成员函数：

  ```c++
  #include<iostream>
  template<typename Vector>
  class VectorIterator
  {
  public:
  	using ValueType = typename Vector::ValueType; // 此处要加上typename指出后面是一个类型名，以免引起歧义
  	using PointerType = ValueType*;// 这里可加可不加typename
  	using ReferenceType = ValueType&;
  public:
  	VectorIterator(PointerType ptr):m_Ptr(ptr) {}
  	VectorIterator operator++()
  	{
  		m_Ptr++;
  		return *this;
  	}
  	VectorIterator operator++(int) // 这里看不懂
  	{
  		VectorIterator iterator = *this;
  		++(*this);
  		return iterator;
  	}
  	VectorIterator operator--()
  	{
  		m_Ptr--;
  		return *this;
  	}
  	VectorIterator operator--(int) // 同++，也看不懂
  	{
  		VectorIterator iterator = *this;
  		--(*this);
  		return iterator;
  	}
  	ReferenceType operator[](int index)
  	{
  		return *(m_Ptr+index);
  	}
  	PointerType operator->() // 不知道这里为啥返回一个指针
  	{
  		return m_Ptr;
  	}
  	ReferenceType operator*()
  	{
  		return *m_Ptr;
  	}
  	bool operator==(const VectorIterator& other) const
  	{
  		return m_Ptr == other.m_Ptr;
  	}
  	bool operator!=(const VectorIterator& other) const
  	{
  		return !(*this==other);
  	}
  private:
  	PointerType m_Ptr; // 迭代器的一个主要成员就是一个指针
  };
  
  template<typename T>
  class Vector
  {
  public:
  	using ValueType = T; // 为了将此类型传给VectorIterator使用，故命名
  	using Iterator = VectorIterator<Vector<T>>; // 用于begin和end函数返回类型
  public:
  	Vector()
  	{
  		// Alloc 2 memory
  		ReAlloc(2); // 初始化构造的时候默认分类两个内存
  	}
  	~Vector()
  	{
  		delete[] m_Data; // 此处写的不完整，还需要释放每个元素的内存
  	}
  	void PushBack(T&& value) // 右值版本，用move
  	{
  		if (m_Size >= m_Capacity) // 发现当前分配内存快要满的时候，再多分配一点内存
  			ReAlloc(m_Capacity + m_Capacity / 2);
  		m_Data[m_Size] = std::move(value);
  		m_Size++;
  	}
  	void PushBack(const T& value) // 左值版本，用copy
  	{
  		if (m_Size >= m_Capacity)
  			ReAlloc(m_Capacity + m_Capacity / 2);
  		m_Data[m_Size] = value;
  		m_Size++;
  	}
  	template<typename... Args>
  	T& EmplaceBack(Args&&... args) // Emplace与Push的区别在于，可以在里面调元素类的构造函数
  	{
  		if (m_Size >= m_Capacity)
  			ReAlloc(m_Capacity + m_Capacity / 2);
  		new(&m_Data[m_Size]) T(std::forward<Args>(args)...); // 这句话看不太懂
  		// m_Data[m_Size] = T(std::forward<Args>(args)...); //可替代上一句话
  		return m_Data[m_Size++];
  	}
  	void PopBack()
  	{
  		if (m_Size > 0)
  		{
  			m_Size--;
  			m_Data[m_Size].~T(); // 弹出末尾元素，并且释放内存
  		}
  	}
  	void Clear()
  	{
  		for (size_t i = 0; i < m_Size; i++)
  			m_Data[i].~T();
  		m_Size = 0;
  	}
  	Iterator begin()
  	{
  		return Iterator(m_Data);
  	}
  	Iterator end()
  	{
  		return Iterator(m_Data+m_Size);
  	}
  
  	size_t Size() const { return m_Size; }
  	const T& operator[] (size_t index) const { return m_Data[index]; }
  	T& operator[] (size_t index) { return m_Data[index]; }
  private:
  	void ReAlloc(size_t newCapacity)
  	{
  		// 1. alloc a new block of memory
  		// 2. copy/move old elements to new block of memory
  		// 3. delete
  		T* newBlock = new T[newCapacity];
  		if (m_Size > newCapacity)
  			m_Size = newCapacity;
  		for (size_t i = 0; i < m_Size; i++)
  			newBlock[i] = std::move(m_Data[i]); // 使用move的方式，避免copy
  		delete[] m_Data;
  		m_Data = newBlock;
  		m_Capacity = newCapacity;
  	}
  
  	T* m_Data = nullptr;
  	size_t m_Size;
  	size_t m_Capacity;
  };
  
  int main()
  {	
  	Vector<std::string> data;
  	data.EmplaceBack("ydc");
  	data.EmplaceBack("tt");
  	for (Vector<std::string>::Iterator it = data.begin(); it != data.end(); it++)
  	{
  		std::cout << *it << std::endl;
  	}
  	for (auto& v : data)
  	{
  		std::cout<<v<<std::endl;
  	}
  	std::cin.get();
  }
  ```

  

