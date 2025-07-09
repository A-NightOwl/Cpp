**本章重点**
1. C/C++内存分布
2. C语言中动态内存管理方式
3. C++中动态内存管理
4. 常见问题

# 1.C/C++的内存分布

```c++
int globalVar = 1;
static int staticGlobalVar = 1;
void Test()
{
	static int staticVar = 1;
	int localVar = 1;
 
	int num1[10] = { 1, 2, 3, 4 };
	char char2[] = "abcd";
	char* pChar3 = "abcd";
	int* ptr1 = (int*)malloc(sizeof(int) * 4);
	int* ptr2 = (int*)calloc(4, sizeof(int));
	int* ptr3 = (int*)realloc(ptr2, sizeof(int) * 4);
	free(ptr1);
	free(ptr3);
}
```
1. 选择题：  
选项: A.栈 B.堆 C.数据段(静态区) D.代码段(常量区)  
globalVar在哪里？____ staticGlobalVar在哪里？____  
staticVar在哪里？____ localVar在哪里？____  
num1 在哪里？____  
char2在哪里？____ *char2在哪里？___  
pChar3在哪里？____ *pChar3在哪里？____  
ptr1在哪里？____ *ptr1在哪里？____  
2. 填空题：  
sizeof(num1) = ____;  
sizeof(char2) = ____; strlen(char2) = ____;  
sizeof(pChar3) = ____; strlen(pChar3) = ____;  
sizeof(ptr1) = ____;  
3. sizeof 和 strlen 区别？

![请添加图片描述](https://i-blog.csdnimg.cn/direct/c0dc8ff236254ccc9ef81129ac5bcf70.png)

答案:

```
1.
//globalVar 存储于 数据段

//staticGlobalVar 存储于 数据段

//staticVar 存储于 数据段

//localVar 存储于 栈

//localVar1 存储于 栈

//num1 存储于 栈

//char2 存储于 栈
//因为 char2 是一个数组，存在栈上面的，而 “abcd” 是存在常量区的常量，只是拷贝给了 char2

//*char2 存储于 栈
//数组名就是首元素的地址，也就是 char 是地址，*char2 相当于对 cahr2 进行解引用，找到它的内容，那么它的内容是存储在栈上面的

//pChar3 存储于 栈
//pChar3 是一个指针变量，这个指针变量是在栈上面开的

//*pChar3 存储于 代码段
//pChar3 是一个指针变量，它存的是一个地址，它指向常量区的字符串 “a b c d”，*pChar 就是对这个指针变量解引用，找到了它的内容，也就是 “abcd” ，所以它是存在代码段的

//ptr1 存储于 栈

//*ptr1 存储于 堆
2.
注意：sizeof 是求字节大小，strlen 是求字符串长度的。
 
sizeof(num1) = 40（算对象占用空间的大小）
 
sizeof(char2) = 5（char2 是一个字符数组，求大小要计算 ‘\0’）
 
sizeof(pChar3) = 4/8 （指针在 32 位平台大小是 4，64 位平台大小是 8）
 
sizeof(ptr1) = 4/8
 
strlen(char2) = 4（char2 是一个字符数组，求长度不计算 \0）
 
strlen(pChar3) = 4
```
**说明：**
1. 栈又叫堆栈--非静态局部变量/函数参数/返回值等等，栈是向下增长的。
2. 内存映射段是高效的I/O映射方式，用于装载一个共享的动态内存库。用户可使用系统接口创建共享共享内存，做进程间通信。
3. 堆用于程序运行时动态内存分配，堆是可以上增长的。
4. 数据段--存储全局数据和静态数据。
5. 代码段--可执行的代码/只读常量。

# 2.C语言中动态内存管理方式:malloc/calloc/realloc/free

```c++
void Test ()
{
	int* p1 = (int*) malloc(sizeof(int));
	free(p1);
	// 1.malloc/calloc/realloc的区别是什么？
	int* p2 = (int*)calloc(4, sizeof (int));
	int* p3 = (int*)realloc(p2, sizeof(int)*10);
	// 这里需要free(p2)吗？不需要
	free(p3 );
}
```

**注意：**

- 原地扩。需扩展的空间后方有足够的空间可供扩展，此时，realloc 函数直接在原空间后方进行扩展，并返回该内存空间首地址（即原来的首地址）。
- 异地扩。需扩展的空间后方没有足够的空间可供扩展，此时，realloc 函数会在堆区中重新找一块满足要求的内存空间，把原空间内的数据拷贝到新空间中，并主动将原空间内存释放（即还给操作系统），返回新内存空间的首地址。
- 扩容失败。需扩展的空间后方没有足够的空间可供扩展，并且堆区中也没有符合需要开辟的内存大小的空间。结果就是开辟内存失败，返回一个 NULL（空指针）。

# 3.C++内存管理方式
>C语言内存管理方式在C++中可以继续使用，但有些地方就无能为力，而且使用起来比较麻烦，因此C++又提出了自己的内存管理方式：**通过new和delete操作符进行动态内存管理。**
## 3.1 new和delete的基本操作
**（1）new  一个 int 类型的对象**
```C++
int main()
{
	// 用malloc动态申请一个int类型的空间
	int* p1 = (int*)malloc(sizeof(int));
	// 销毁p1
	free(p1);
 
 
	// 动态申请一个int类型的空间
	int* p1 = new int;
	// 销毁p1
	delete p1;
 
	return 0;
}
```
**（2）new 10 个 int 类型的对象**
```C++
int main()
{
	// 用malloc动态申请一个int类型的空间
	int* p2 = (int*)malloc(10 * sizeof(int));
	// 销毁
	free(p2);
 
 
	// 动态申请一个int类型的空间
	int* p2 = new int[10];
	// 销毁
	delete[] p2;
 
	return 0;
}
```
delete[ ] 对应的是new[ ] ，是申请多个空间时的样子。 

**（3）new 一个 int 类型对象，然后初始化为 10**
```C++
{
	int* p3 = (int*)malloc(sizeof(int));
	*p3 = 10; //赋值
	//销毁
	free(p3);
 
 
	// 动态申请一个int类型的空间并初始化为10
	int* p3 = new int(10);
	// 销毁
	delete p3;
 
	return 0;
}
```
**（4）new 10 个 int 类型对象，并进行初始化**
```C++
int main()
{
	//动态申请10个int类型的空间并初始化为1到10
	int* p8 = (int*)malloc(sizeof(int) * 10); //申请
	for (int i = 0; i < 10; i++) //赋值
	{
		p8[i] = i;
	}
	free(p8); //销毁
 
	//动态申请10个int类型的空间并初始化为1到10
	int* p4 = new int[10] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
	//销毁p4
	delete[] p4;
 
	return 0;
}
```
当然如用new在赋值的时候，如果只给前面的控件赋值，后面的空间会赋值为0


## 3.2 new和delete操作符自定义类型
对于**内置类型**来说，`malloc` 和 `new` 用法几乎一样，
但是对于自定义类型来说，`new`和`delete`相比于`malloc`，会调用构造函数和析构函数

这里以链表为列，看看`malloc`和`new`的区别

**malloc：单纯开空间**

```c++
//链表
struct ListNode
{
    ListNode* next;
    int val;
};
 
//申请节点
struct ListNode* BuyListNode(int x) 
{
    struct ListNode* node = (struct ListNode*)malloc(sizeof(struct ListNode));
    assert(node);
 
    node->next = NULL;
    node->val = x;
    return node;
}
 
int main()
{
    // 定义n1节点
    struct ListNode* n1 = BuyListNode(1);
    free(n1);
    return 0;
}
```
**new：开空间+调用构造函数初始化**

```c++
//链表
class ListNode
{
public:
    //构造函数
    ListNode(int val = 0)
        :_next(nullptr), _val(val) // 初始化列表
    {
        cout << "ListNode" << endl;
    }
    ~ListNode()
    {
        cout << "~ListNode" << endl;
 
    }
private:
    ListNode* _next;
    int _val;
};
 
int main()
{
    // 定义n2节点
    ListNode* n2 = new ListNode(2); // new会去调用ListNode的构造函数
    delete n2;
 
    return 0;
}
```

```c++
#include<iostream>
using namespace ::std;
class Date
{
public:
	Date(int year)
	{
		cout << "Date" << endl;
		_year = year;
	}
	~Date()
	{
		cout << "~Date()" << endl;
	}
private:
	int _year;
};
int main()
{
	Date* p = new Date(1);
	delete p;
	Date* p1 = new Date[4]{1,2,3,4};//支持隐式类型转换
	delete[] p1;
	Date* p2 = new Date[4]{ Date(1949),Date(2019),Date(2022),Date(2023) };
	delete[] p2;
	return 0;
}
```

**总结：**
1. `malloc/free` 是函数，而 `new/delete`是关键字
2. C++ 中如果是申请内置类型的对象或是数组，用 `new/delete` 和 `malloc/free` 没有什么区别。
3. 如果是自定义类型的话，`new` 和 `delete` 分别是 开空间+构造函数、析构函数+释放空间，而 `malloc` 和 `free` 仅仅是 开空间和释放空间，可以看到区别还是很大的。
4. 建议在 C++ 中无论是内置类型还是自定义类型的申请和释放，尽量都使用 `new` 和 `delete`。

## 3.3 operator new与operator delete函数
**补充：**
异常，简单地说就是代码执行到某处中断了，无法继续执行，这时的情况就叫异常。  
平常我们在遇到对空指针解引用的情况时，就是出现异常了。  

![请添加图片描述](https://i-blog.csdnimg.cn/direct/3713aa7eb97c4a7eb202f5fc81605af4.png)

1. 解读operate new源代码
```c++
void* __CRTDECL operator new(size_t size) _THROW1(_STD bad_alloc)
{
	void* p;
	while ((p = malloc(size)) == 0)
		if (_callnewh(size) == 0)
		{
			// 如果申请内存失败了，这里会抛出bad_alloc 类型异常
			static const std::bad_alloc nomem;
			_RAISE(nomem);
		}
	return (p);
}

```

![请添加图片描述](https://i-blog.csdnimg.cn/direct/620234ad344a4ff895cf1edaad701b11.png)


- 因此：operate new 是一个函数，调用了malloc，并且如果**开辟失败会出现异常**。
- 为什么要这样做呢？因为**面向对象语言不喜欢用返回值来处理问题，而更喜欢使用抛异常**，来解决问题，通过异常我们可以判断出现了什么情况的错误。

2. 解读operate delete源代码
```C++
void operator delete(void* pUserData)
{
	_CrtMemBlockHeader* pHead;
	RTCCALLBACK(_RTC_Free_hook, (pUserData, 0));
	if (pUserData == NULL)
		return;
		
	_mlock(_HEAP_LOCK);
	__TRY
	pHead = pHdr(pUserData);
	_ASSERTE(_BLOCK_TYPE_IS_VALID(pHead->nBlockUse));
	_free_dbg(pUserData, pHead->nBlockUse);
	__FINALLY
		_munlock(_HEAP_LOCK); 
	__END_TRY_FINALLY
		return;
}
```


![请添加图片描述](https://i-blog.csdnimg.cn/direct/0d3f83fca6e14508b270fb89bc695cf5.png)

因此：operate delete释放空间的操作跟free本质上是相同的。


## 3.4 new和delete的实现原理
**内置类型**

如果申请的是内置类型的空间，new和malloc，delete和free基本类似，  
不同的地方是：new/delete申请和释放的是单个元素的空间，new[]和delete[]申请的是连续空间，而且new在申请空间失败时会抛异常，malloc会返回NULL。

**自定义类型**
>**new的原理:**  
>开空间(operator new-->malloc)+构造函数
>1. 调用operator new函数申请空间
>2. 在申请的空间上执行构造函数，完成对象的构造

>**delete的原理:**   
>析构函数+释放空间(operator delete-->free)
>1. 在空间上执行析构函数，完成对象中资源的清理工作
>2. 调用operator delete函数释放对象的空间

![请添加图片描述](https://i-blog.csdnimg.cn/direct/5769fb89618145b280d06c24b111e53e.png)

>**new T[N] 的原理**
>
>1. 调用operator new[]函数，在operator new[]中实际调用operator new函数完成N个对象空间的申请
>2. 在申请的空间上执行N次构造函数

>**delete[ ] 的原理**
>
>1. 在释放的对象空间上执行N次析构函数，完成N个对象中资源的清理
>2. 调用operator delete[]释放空间，实际在operator delete[]中调用operator delete来释放空间

## 3.5 定位new
定位new表达式是在已分配的原始内存空间中**调用构造函数初始化一个对象**。
基本用法
```C++
#include<iostream>
using namespace ::std;
int main()
{
	Date* p2 = (Date *)operator new(sizeof(Date));
	new(p2)Date(1);//如果有默认构造,括号可以不写。
	delete p2;
	return 0;
}
```
>一般我们会在**池化技术**中使用，来提高效率。

# 4.常见问题
## 4.1 malloc/free和new/delete的区别
>malloc/free和new/delete的共同点是：都是从堆上申请空间，并且需要用户手动释放。

>不同的地方是：
>1. malloc和free是函数，  
new和delete是操作符
>2. malloc申请的空间不会初始化，  
new可以初始化
>3. malloc申请空间时，需要手动计算空间大小并传递，  
new只需在其后跟上空间的类型即可，如果是多个对象，[]中指定对象个数即可
>4. malloc的返回值为void*, 在使用时必须强转，  
new不需要，因为new后跟的是空间的类型
>5. malloc申请空间失败时，返回的是NULL，因此使用时必须判空，    
new不需要，但是new需要捕获异常
>6. 申请自定义类型对象时，malloc/free只会开辟空间或销毁空间，不会调用构造函数与析构函数，   
而new在申请空间后会调用构造函数完成对象的初始化，delete在释放空间前会调用析构函数完成空间中资源的清理


## 4.2 内存泄漏
内存泄露 memory leak，是指程序在申请内存后，**无法释放已申请的内存空间**，一次内存泄露危害可以忽略，但**内存泄露堆积后果很严重**，无论多少内存,迟早会被占光。
看这样一段代码：
```C++
#include<iostream>
using namespace ::std;
int main()
{
	char* p = new char[1024 * 1024 * 1024];
	return 0;
}
```
这个程序在编译器上会导致内存泄漏吗？
答案是——不会的。因为编译器会在程序结束时，帮你释放这些空间，这就跟手机卡了，关机再重启的道理是一样的，但是如果是一个24*7小时不停的程序呢？比如服务器，如果存在内存泄漏，如果说大了很快就会被检查出来，但是如果一次只泄漏几个字节，那么这台服务器会越用越卡，最终导致服务器宕机——出事故了，那相关人员就得跟着遭殃…

**分类**
1. 系统资源泄漏：
指程序使用系统分配的资源，比方套接字、文件描述符、管道等没有使用对应的函数释放掉，导致系统资源的浪费，严重可导致系统效能减少，系统执行不稳定。
2. 堆内存泄漏(Heap leak)
堆内存指的是程序执行中依据须要分配通过malloc / calloc / realloc / new等从堆中分配的一块内存，用完后必须通过调用相应的 free或者delete 删掉。假设程序的设计错误导致这部分内存没有被释放，那么以后这部分空间将无法再被使用，就会产生Heap Leak。

**那该如何解决内存泄漏呢？**

1. 养成良好的编程习惯，对开辟的空间即用即销毁。
2. 合理利用检查内存泄漏的工具，通常这类工具比较昂贵。
3. 利用RAII思想——资源获取即初始化，或者智能指针——充分地体现了RAII思想。
4. 公司内部规范使用内部实现的私有内存管理库。这套库自带内存泄漏检测的功能选项。
5. 私有内存管理库，就是公司内部实现的，这套库自带内存检查功能。

>总的来看无非就两类：用良好的习惯，规范，思想，尽可能的减少出错的几率。出了问题用现成的工具去检测问题，及时的改正问题。

资源泄漏：包括但不限于内存泄漏，比如说服务端保存数据，结果服务器有段时间崩了，从而导致客户的数据有所损失，或者在保存数据时，没有及时的读入数据从而导致数据的丢失，等等这都算是资源的泄漏。

